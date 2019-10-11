---
title: Fonctions table (TVF)-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182532"
---
# <a name="table-valued-functions-tvfs"></a>Fonctions table (TVF)
> [!NOTE]
> **EF5 uniquement** : les fonctionnalités, les API, etc. présentées dans cette page ont été introduites dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

La vidéo et la procédure pas à pas montrent comment mapper des fonctions table (TVF) à l’aide de l’Entity Framework Designer. Il montre également comment appeler une fonction TVF à partir d’une requête LINQ.

Les TVF sont actuellement pris en charge uniquement dans le flux de travail Database First.

La prise en charge de TVF a été introduite dans Entity Framework version 5. Notez que pour utiliser les nouvelles fonctionnalités telles que les fonctions table, les énumérations et les types spatiaux, vous devez cibler .NET Framework 4,5. Visual Studio 2012 cible .NET 4,5 par défaut.

Les TVF sont très similaires aux procédures stockées avec une différence clé : le résultat d’une fonction TVF est composable. Cela signifie que les résultats d’une fonction TVF peuvent être utilisés dans une requête LINQ alors que les résultats d’une procédure stockée ne le peuvent pas.

## <a name="watch-the-video"></a>Regarder la vidéo

**Présenté par**: Kornich Julia

[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Conditions préalables

Pour effectuer cette procédure pas à pas, vous devez :

- Installez la [base de données School](~/ef6/resources/school-database.md).

- Disposer d’une version récente de Visual Studio

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrir Visual Studio
2.  Dans le menu **fichier** , pointez sur **nouveau**, puis cliquez sur **projet** .
3.  Dans le volet gauche, cliquez sur **Visual C @ no__t-1**, puis sélectionnez le modèle **console** .
4.  Entrez **TVF** comme nom du projet, puis cliquez sur **OK** .

## <a name="add-a-tvf-to-the-database"></a>Ajouter une TVF à la base de données

-   Sélectionnez **affichage-&gt; Explorateur d’objets SQL Server**
-   Si la base de données locale ne figure pas dans la liste des serveurs : Cliquez avec le bouton droit sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser l' **authentification Windows** par défaut pour se connecter au serveur de base de données locale
-   Développez le nœud de base de données locale
-   Sous le nœud bases de données, cliquez avec le bouton droit sur le nœud de la base de données School, puis sélectionnez **nouvelle requête...**
-   Dans l’éditeur T-SQL, collez la définition de TVF suivante.

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL, puis sélectionnez **exécuter** .
-   La fonction GetStudentGradesForCourse est ajoutée à la base de données School.

 

## <a name="create-a-model"></a>Créer un modèle

1.  Cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, pointez sur **Ajouter**, puis cliquez sur **nouvel élément** .
2.  Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet **modèles** .
3.  Entrez **TVFModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter** .
4.  Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant** .
5.  Cliquez sur **nouvelle connexion** entrée (base de données **) \\mssqllocaldb** dans la zone de texte Nom du serveur, entrez **School** for le nom de la base de données, cliquez sur **OK** .
6.  Dans la boîte de dialogue choisir vos objets de base de données, sous les **Tables** node, sélectionnez la **personne**, **StudentGrade**et le **cours** tables
7.  Sélectionnez la fonction **GetStudentGradesForCourse** située sous les **procédures stockées et les fonctions** node note, à compter de Visual Studio 2012, la Entity designer vous permet d’importer par lot vos procédures stockées et fonctions.
8.  Cliquez sur **Terminer**
9.  Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché. Tous les objets que vous avez sélectionnés dans la zone **choisir vos objets de base de données** dialog sont ajoutés au modèle.
10. Par défaut, la forme de résultat de chaque fonction ou procédure stockée importée devient automatiquement un nouveau type complexe dans votre modèle d’entité. Toutefois, nous voulons mapper les résultats de la fonction GetStudentGradesForCourse à l’entité StudentGrade : Cliquez avec le bouton droit sur l’aire de conception et sélectionnez **Explorateur de modèles** dans l’Explorateur de modèles, sélectionnez **importations de fonctions**, puis double-cliquez sur la fonction **GetStudentGradesForCourse** dans la boîte de dialogue Modifier l’importation de fonction, puis sélectionnez **entités**  and choisir **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier dans lequel la méthode main est définie. Ajoutez le code suivant à la fonction main.

Le code suivant montre comment générer une requête qui utilise une fonction table. La requête projette les résultats dans un type anonyme qui contient le titre du cours associé et les étudiants associés avec un niveau supérieur ou égal à 3,5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment mapper des fonctions table (TVF) à l’aide de l’Entity Framework Designer. Elle a également démontré comment appeler une TVF à partir d’une requête LINQ.
