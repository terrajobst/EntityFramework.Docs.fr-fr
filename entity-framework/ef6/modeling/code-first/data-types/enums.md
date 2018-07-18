---
title: Prise en charge de l’enum - Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
caps.latest.revision: 3
ms.openlocfilehash: 698c84a9f0679c67a086574de2f253f214fb8d0b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121332"
---
# <a name="enum-support---code-first"></a>Prise en charge de l’enum - Code First
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Cette procédure pas à pas vidéo et pas à pas montre comment utiliser les types enum avec Entity Framework Code First. Il montre également comment utiliser les énumérations dans une requête LINQ.

Cette procédure pas à pas utilise Code First pour créer une nouvelle base de données, mais vous pouvez également utiliser [Code First pour mapper à une base de données existante](~/ef6/modeling/code-first/workflows/existing-database.md).

Prise en charge de l’énumération a été introduite dans Entity Framework 5. Pour utiliser les nouvelles fonctionnalités telles que les énumérations, les types de données spatiales et les fonctions table, vous devez cibler .NET Framework 4.5. Visual Studio 2012 cible .NET 4.5 par défaut.

Dans Entity Framework, une énumération peut avoir des types sous-jacents suivants : **octets**, **Int16**, **Int32**, **Int64** , ou **SByte**.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo montre comment utiliser les types enum avec Entity Framework Code First. Il montre également comment utiliser les énumérations dans une requête LINQ.

**Présenté par**: Julia Kornich

**Vidéo**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.

 

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrez Visual Studio 2012
2.  Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**
3.  Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle
4.  Entrez **EnumCodeFirst** en tant que le nom du projet et cliquez sur **OK**

## <a name="define-a-new-model-using-code-first"></a>Définir un nouveau modèle à l’aide de Code First

Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine). Le code suivant définit la classe de service.

Le code définit également l’énumération DepartmentNames. Par défaut, l’énumération est de **int** type. La propriété nom de la classe de service est du type DepartmentNames.

Ouvrez le fichier Program.cs et collez les définitions de classe suivantes.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Définir le DbContext de Type dérivé

Outre la définition des entités, vous devez définir une classe qui dérive de DbContext et expose DbSet&lt;TEntity&gt; propriétés. Le DbSet&lt;TEntity&gt; propriétés permettent le contexte de connaître les types que vous souhaitez inclure dans le modèle.

Une instance du type DbContext dérivée gère les objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, modifier le suivi et la persistance des données à la base de données.

Les types DbContext et DbSet sont définis dans l’assembly EntityFramework. Nous allons ajouter une référence à cette DLL à l’aide du package EntityFramework NuGet.

1.  Dans l’Explorateur de solutions, cliquez sur le nom du projet.
2.  Sélectionnez **gérer les Packages NuGet...**
3.  Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package.
4.  Cliquez sur **installer**

Notez que l’assembly EntityFramework, en plus des références aux assemblys System.ComponentModel.DataAnnotations et System.Data.Entity sont également ajoutés.

En haut du fichier Program.cs, ajoutez le code suivant à l’aide d’instruction :

``` csharp
using System.Data.Entity;
```

Dans Program.cs, ajoutez la définition du contexte. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier Program.cs dans lequel la méthode Main est définie. Ajoutez le code suivant dans la fonction Main. Le code ajoute un nouvel objet de service pour le contexte. Ensuite, il enregistre les données. Le code exécute également une requête LINQ qui retourne un département où le nom est DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Afficher la base de données généré

Lorsque vous exécutez l’application la première fois, Entity Framework crée une base de données pour vous. Étant donné que nous Visual Studio 2012 est installé, la base de données est créé sur l’instance de base de données locale. Par défaut, Entity Framework nomme la base de données après le nom qualifié complet de contexte dérivé (pour cet exemple est **EnumCodeFirst.EnumTestContext**). Les fois suivantes, que la base de données existante sera utilisé.  

Notez que si vous apportez des modifications à votre modèle une fois que la base de données a été créé, vous devez utiliser des Migrations Code First pour mettre à jour le schéma de base de données. Consultez [Code First pour une base de données](~/ef6/modeling/code-first/workflows/new-database.md) pour obtenir un exemple d’utilisation de Migrations.

Pour afficher la base de données et les données, procédez comme suit :

1.  Dans le menu principal de Visual Studio 2012, sélectionnez **vue**  - &gt; **Explorateur d’objets SQL Server**.
2.  Si la base de données locale n’est pas dans la liste des serveurs, cliquez sur le bouton droit de la souris sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser la valeur par défaut **l’authentification Windows** pour se connecter à la Instance de base de données locale
3.  Développez le nœud de base de données locale
4.  Dérouler la **bases de données** dossier pour voir la nouvelle base de données et accédez à la **département** Note de Code First ne crée pas une table qui mappe au type énumération de table
5.  Pour afficher les données, avec le bouton droit sur la table et sélectionnez **afficher les données**

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment utiliser les types enum avec Entity Framework Code First. 
