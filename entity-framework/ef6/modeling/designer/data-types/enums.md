---
title: Prise en charge enum - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
caps.latest.revision: 3
ms.openlocfilehash: cbf9b01fcbe21274ff3644c6ae6bc8fdfd338e3b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121349"
---
# <a name="enum-support---ef-designer"></a>Prise en charge de l’enum - Entity Framework Designer
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Cette procédure pas à pas vidéo et pas à pas montre comment utiliser les types enum avec Entity Framework Designer. Il montre également comment utiliser les énumérations dans une requête LINQ.

Cette procédure pas à pas utilise Model First pour créer une nouvelle base de données, mais le Concepteur EF peut également servir avec la [Database First](~/ef6/modeling/designer/workflows/database-first.md) flux de travail pour mapper à une base de données existante.

Prise en charge de l’énumération a été introduite dans Entity Framework 5. Pour utiliser les nouvelles fonctionnalités telles que les énumérations, les types de données spatiales et les fonctions table, vous devez cibler .NET Framework 4.5. Visual Studio 2012 cible .NET 4.5 par défaut.

Dans Entity Framework, une énumération peut avoir des types sous-jacents suivants : **octets**, **Int16**, **Int32**, **Int64** , ou **SByte**.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo montre comment utiliser les types enum avec Entity Framework Designer. Il montre également comment utiliser les énumérations dans une requête LINQ.

**Présenté par**: Julia Kornich

**Vidéo**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrez Visual Studio 2012
2.  Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**
3.  Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle
4.  Entrez **EnumEFDesigner** en tant que le nom du projet et cliquez sur **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Créer un nouveau modèle à l’aide du Concepteur EF

1.  Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**
2.  Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles
3.  Entrez **EnumTestModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**
4.  Dans la page de l’Assistant Entity Data Model, sélectionnez **modèle vide** dans la boîte de dialogue Choisir le contenu du modèle
5.  Cliquez sur **terminer**

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.

L'assistant exécute les actions suivantes :

-   Génère le fichier EnumTestModel.edmx qui définit le modèle conceptuel, le modèle de stockage et le mappage entre les deux. Définit la propriété Metadata Artifact Processing du fichier .edmx à incorporer dans l’Assembly de sortie pour les fichiers de métadonnées générées sont intégrés dans l’assembly.
-   Ajoute une référence aux assemblys suivants : EntityFramework System.ComponentModel.DataAnnotations et System.Data.Entity.
-   Crée des fichiers EnumTestModel.tt et EnumTestModel.Context.tt et les ajoute dans le fichier .edmx. Ces fichiers de modèle T4 génèrent le code qui définit le type DbContext dérivée et les types POCO qui mappent aux entités dans le modèle .edmx.

## <a name="add-a-new-entity-type"></a>Ajouter un nouveau Type d’entité

1.  Cliquez sur une zone vide de l’aire de conception, sélectionnez **Add -&gt; entité**, s’affiche la boîte de dialogue nouvelle entité
2.  Spécifiez **département** pour le type de nommer et spécifier **DepartmentID** pour le nom de la propriété de clé, conservez le type **Int32**
3.  Cliquez sur **OK**.
4.  Cliquez sur l’entité et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**
5.  Renommer la nouvelle propriété à **nom**
6.  Modifier le type de la nouvelle propriété à **Int32** (par défaut, la nouvelle propriété est de type chaîne) pour modifier le type, ouvrez la fenêtre Propriétés et affectez à la propriété de Type **Int32**
7.  Ajouter une autre propriété scalaire et renommez-le **Budget**, modifiez le type de **décimal**

## <a name="add-an-enum-type"></a>Ajouter un Type Enum

1.  Dans Entity Framework Designer, cliquez sur la propriété Name, sélectionnez **convertir enum**

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  Dans le **Enum ajouter** boîte de dialogue Entrez **DepartmentNames** pour le nom de Type Enum, modifiez le Type sous-jacent **Int32**, puis ajoutez les membres suivants pour le type : anglais, Mathématiques et des prix

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  Appuyez sur **OK**
4.  Enregistrer le modèle et générer le projet
    > [!NOTE]
    > Lorsque vous générez, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs. Vous pouvez ignorer ces avertissements, car une fois que nous avons choisi générer la base de données à partir du modèle, les erreurs disparaîtront.

Si vous examinez la fenêtre Propriétés, vous pouvez remarquer que le type de la propriété de nom a été changé en **DepartmentNames** et le type d’énumération qui vient d’être ajoutée a été ajouté à la liste des types.

Si vous basculez vers la fenêtre Explorateur de modèles, vous verrez que le type a été également ajouté au nœud Types Enum.

![ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Vous pouvez également ajouter des nouveaux types enum à partir de cette fenêtre en cliquant sur le bouton droit de la souris et en sélectionnant **ajouter un Enum Type**. Une fois que le type est créé il apparaîtra dans la liste des types et vous pourrez associer à une propriété

## <a name="generate-database-from-model"></a>Générer la base de données à partir du modèle

Maintenant, nous pouvons générer une base de données qui est basé sur le modèle.

1.  Cliquez sur un espace vide sur l’aire de conception et sélectionnez le Concepteur d’entités **générer la base de données à partir du modèle**
2.  Choisir votre boîte de dialogue de connexion de données de l’Assistant génération de la base de données est affiché cliquez sur le **nouvelle connexion** bouton spécifier **(localdb)\\mssqllocaldb** pour le nom du serveur et  **EnumTest** pour la base de données et cliquez sur **OK**
3.  Une boîte de dialogue vous demandant si vous souhaitez créer une base de données s’affiche, cliquez sur **Oui**.
4.  Cliquez sur **suivant** et l’Assistant Création de base de données génère le langage de définition de données (DDL) pour créer une base de données le DDL généré est affiché dans le résumé et la Note de boîte de dialogue de paramètres, le DDL ne contenant pas de définition pour un table qui mappe au type énumération
5.  Cliquez sur **Terminer** le script DDL ne s’exécute pas en cliquant sur Terminer.
6.  L’Assistant Création de base de données effectue les opérations suivantes : ouvre le **EnumTest.edmx.sql** dans l’éditeur T-SQL génère les sections de mappage du schéma et magasin de l’EDMX fichier ajoute les informations de chaîne de connexion au fichier App.config
7.  Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL et sélectionnez **Execute** établir une connexion à la boîte de dialogue serveur s’affiche, entrez les informations de connexion de l’étape 2 sur **Connect**
8.  Pour afficher le schéma généré, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server, puis sélectionnez **actualiser**

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier Program.cs dans lequel la méthode Main est définie. Ajoutez le code suivant dans la fonction Main. Le code ajoute un nouvel objet de service pour le contexte. Ensuite, il enregistre les données. Le code exécute également une requête LINQ qui retourne un département où le nom est DepartmentNames.English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

```
DepartmentID: 1 Name: English
```

Pour afficher les données dans la base de données, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server et sélectionnez **Actualiser**. Ensuite, cliquez sur le bouton droit de la souris sur la table et sélectionnez **afficher les données**.

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment mapper des types d’énumération à l’aide d’Entity Framework Designer et comment utiliser des énumérations dans le code. 
