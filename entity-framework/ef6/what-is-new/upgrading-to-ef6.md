---
title: Mise à niveau vers Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 711f1940080de27bd23cb8f641a5c7f2711dd65b
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182005"
---
# <a name="upgrading-to-entity-framework-6"></a>Mise à niveau vers Entity Framework 6

Dans les versions précédentes d’EF, le code a été fractionné entre core bibliothèques (principalement System.Data.Entity.dll) dans le cadre du .NET Framework et hors-bande (OOB) (principalement EntityFramework.dll) inclus dans un package NuGet. EF6 prend le code des bibliothèques core et l’incorpore dans les bibliothèques OOB. Cette action était nécessaire pour permettre à EF qu’il veut ouvrir la source et pour qu’elle soit en mesure de faire évoluer à un rythme différent à partir de .NET Framework. La conséquence de cela est que les applications devront être régénérés sur les types déplacés.

Il doit s’agir simple pour les applications qui utilisent de DbContext comme expédié dans Entity Framework 4.1 et versions ultérieures. Un peu plus de travail est obligatoire pour les applications qui utilisent ObjectContext, mais il n’est pas toujours difficile à gérer.

Voici une liste de vérification des choses que vous devez faire pour mettre à niveau une application existante à EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Installez le package NuGet d’EF6

Vous devez mettre à niveau vers le nouveau runtime Entity Framework 6.

1. Avec le bouton droit sur votre projet, puis sélectionnez **gérer les Packages NuGet...**  
2. Sous le **Online** onglet sélectionnez **EntityFramework** et cliquez sur **installer**  
   > [!NOTE]
   > Si une version précédente de l’installation package EntityFramework NuGet cela met à niveau à EF6.

Vous pouvez également exécuter la commande suivante à partir de la Console du Gestionnaire de Package :

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Vérifiez que les références d’assembly à System.Data.Entity.dll sont supprimés.

Installation du package NuGet d’EF6 doit supprimer automatiquement toutes les références à System.Data.Entity à partir de votre projet pour vous.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Remplacez tous les modèles EF Designer (EDMX) pour utiliser la génération de code EF 6.x

Si vous avez tous les modèles créés avec le Concepteur EF, vous devez mettre à jour les modèles de génération de code pour générer du code compatible EF6.

> [!NOTE]
> Seuls les modèles de générateur de DbContext EF 6.x sont actuellement disponibles pour Visual Studio 2012 et 2013.

1. Supprimer les modèles de génération de code existants. Ces fichiers seront généralement nommés  **\<edmx_file_name\>.tt** et  **\<edmx_file_name\>. Context.tt** et être imbriqué sous votre fichier edmx dans l’Explorateur de solutions. Vous pouvez sélectionner les modèles dans l’Explorateur de solutions, puis appuyez sur la **Del** clé pour les supprimer.  
   > [!NOTE]
   > Dans les projets de Site Web les modèles ne seront pas être imbriqués sous votre fichier edmx, mais répertoriées à ses côtés dans l’Explorateur de solutions.  

   > [!NOTE]
   > Dans les projets VB.NET, vous devrez activer « Afficher tous les fichiers » être en mesure de voir les fichiers de modèle imbriqué.
2. Ajouter le modèle de génération de code EF 6.x approprié. Ouvrez votre modèle dans le Concepteur EF, clic droit sur l’aire de conception et sélectionnez **ajouter un élément de génération de Code...**
    - Si vous utilisez l’API DbContext (recommandé) puis **EF 6.x DbContext Générateur** seront disponibles sous le **données** onglet.  
      > [!NOTE]
      > Si vous utilisez Visual Studio 2012, vous devrez installer les outils de 6 EF pour que ce modèle. Consultez [obtenir Entity Framework](~/ef6/fundamentals/install.md) pour plus d’informations.  

    - Si vous utilisez l’API ObjectContext, vous devez sélectionner le **Online** onglet et recherchez **EF 6.x EntityObject Generator**.  
3. Si vous avez appliqué toutes les personnalisations pour les modèles de génération de code, vous devrez réappliquer les modèles mis à jour.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Mettre à jour les espaces de noms pour les types d’EF core utilisé

Les espaces de noms pour les types DbContext et Code First n’ont pas changé. Cela signifie que pour de nombreuses applications qui utilisent EF 4.1 ou plus tard vous ne devez pas apporter de modifications.

Type ObjectContext qui étaient présentes dans System.Data.Entity.dll ont été déplacés vers les nouveaux espaces de noms. Cela signifie que vous devrez peut-être mettre à jour votre *à l’aide de* ou *importation* directives pour la build EF6.

La règle générale pour les modifications de l’espace de noms est que n’importe quel type dans System.Data.* est déplacé vers System.Data.Entity.Core.*. En d’autres termes, insérez simplement **Entity.Core.** Après avoir System.Data. Exemple :

- System.Data.EntityException = > System.Data. **Entity.Core**. EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core**. Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core**. Objects.DataClasses.RelationshipManager  

Ces types sont dans le *Core* espaces de noms car elles ne sont pas utilisées directement pour la plupart des applications DbContext. Certains types qui faisaient partie de System.Data.Entity.dll sont toujours utilisés couramment et directement des applications basées sur le DbContext et par conséquent, n'ont pas été transférés dans le *Core* espaces de noms. Ces équivalents sont :

- System.Data.EntityState = > System.Data. **Entité**. EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Cette classe a été renommée ; une classe avec l’ancien nom existe toujours et qu’il fonctionne, mais il désormais marqués comme obsolète.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Cette classe a été renommée ; une classe avec l’ancien nom existe toujours et qu’il fonctionne, mais il désormais marqué comme obsolète.)  
- Classes spatiales (par exemple, DbGeography, DbGeometry) ont été déplacés de System.Data.Spatial = > System.Data. **Entité**. Spatial

> [!NOTE]
> Certains types dans l’espace de noms System.Data se trouvent dans System.Data.dll, qui n’est pas un assembly EF. Ces types n’ont pas été déplacées et par conséquent, leurs espaces de noms restent inchangées.
