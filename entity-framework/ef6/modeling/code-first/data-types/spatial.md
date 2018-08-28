---
title: Spatial - Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995210"
---
# <a name="spatial---code-first"></a>Spatial - Code First
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

La procédure pas à pas vidéo et pas à pas montre comment mapper des types de données spatiales avec Entity Framework Code First. Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.

Cette procédure pas à pas utilise Code First pour créer une nouvelle base de données, mais vous pouvez également utiliser [Code First pour une base de données existante](~/ef6/modeling/code-first/workflows/existing-database.md).

Prise en charge de type spatial a été introduite dans Entity Framework 5. Notez que pour utiliser les nouvelles fonctionnalités telles que le type spatial, les enums et les fonctions table, vous devez cibler .NET Framework 4.5. Visual Studio 2012 cible .NET 4.5 par défaut.

Pour utiliser les types de données spatiales, vous devez également utiliser un fournisseur Entity Framework qui prend en charge spatiale. Consultez [prise en charge de fournisseur pour les types spatiaux](~/ef6/fundamentals/providers/spatial-support.md) pour plus d’informations.

Il existe deux types de données spatiales principale : geography et geometry. Le type de données geography stocke des données ellipsoïdes (par exemple, GPS de latitude et longitude coordonnées). Le type de données geometry représente un système de coordonnées euclidien (plat).

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo montre comment mapper des types de données spatiales avec Entity Framework Code First. Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.

**Présenté par**: Julia Kornich

**Vidéo**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrez Visual Studio 2012
2.  Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**
3.  Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle
4.  Entrez **SpatialCodeFirst** en tant que le nom du projet et cliquez sur **OK**

## <a name="define-a-new-model-using-code-first"></a>Définir un nouveau modèle à l’aide de Code First

Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine). Le code suivant définit la classe de l’université.

L’université a la propriété d’emplacement du type de DbGeography. Pour utiliser le type de DbGeography, vous devez ajouter une référence à l’assembly System.Data.Entity et également ajouter le System.Data.Spatial à l’aide d’instruction.

Ouvrez le fichier Program.cs et collez le code suivant à l’aide d’instructions en haut du fichier :

``` csharp
using System.Data.Spatial;
```

Ajoutez la définition de classe University suivante au fichier Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
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

Notez qu’en plus de l’assembly EntityFramework, une référence à l’assembly System.ComponentModel.DataAnnotations est également ajoutée.

En haut du fichier Program.cs, ajoutez le code suivant à l’aide d’instruction :

``` csharp
using System.Data.Entity;
```

Dans Program.cs, ajoutez la définition du contexte. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier Program.cs dans lequel la méthode Main est définie. Ajoutez le code suivant dans la fonction Main.

Le code ajoute deux nouveaux objets de l’université au contexte. Propriétés spatiales sont initialisées à l’aide de la méthode DbGeography.FromText. Le point géographique représenté en tant que WellKnownText est transmise à la méthode. Le code enregistre ensuite les données. Ensuite, la requête LINQ qui retourne un objet de l’université où son emplacement le plus proche de l’emplacement spécifié, construction et l’exécution.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Afficher la base de données généré

Lorsque vous exécutez l’application la première fois, Entity Framework crée une base de données pour vous. Étant donné que nous Visual Studio 2012 est installé, la base de données est créé sur l’instance de base de données locale. Par défaut, Entity Framework nomme la base de données après le nom qualifié complet de contexte dérivé (dans cet exemple est **SpatialCodeFirst.UniversityContext**). Les fois suivantes, que la base de données existante sera utilisé.  

Notez que si vous apportez des modifications à votre modèle une fois que la base de données a été créé, vous devez utiliser des Migrations Code First pour mettre à jour le schéma de base de données. Consultez [Code First pour une base de données](~/ef6/modeling/code-first/workflows/new-database.md) pour obtenir un exemple d’utilisation de Migrations.

Pour afficher la base de données et les données, procédez comme suit :

1.  Dans le menu principal de Visual Studio 2012, sélectionnez **vue**  - &gt; **Explorateur d’objets SQL Server**.
2.  Si la base de données locale n’est pas dans la liste des serveurs, cliquez sur le bouton droit de la souris sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser la valeur par défaut **l’authentification Windows** pour se connecter à la l’instance de base de données locale
3.  Développez le nœud de base de données locale
4.  Dérouler la **bases de données** dossier pour voir la nouvelle base de données et accédez à la **universités** table
5.  Pour afficher les données, avec le bouton droit sur la table et sélectionnez **afficher les données**

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment utiliser les types de données spatiales avec Entity Framework Code First. 
