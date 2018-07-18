---
title: Fonctions table (TVF) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121374"
---
# <a name="table-valued-functions-tvfs"></a>Fonctions table (TVF)
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

La procédure pas à pas vidéo et pas à pas montre comment mapper des fonctions table (TVF) à l’aide d’Entity Framework Designer. Il montre également comment appeler une fonction table à partir d’une requête LINQ.

Fonctions table est actuellement pris en charge uniquement dans le flux de travail première base de données.

Prise en charge de la fonction table a été introduite dans Entity Framework version 5. Notez que pour utiliser les nouvelles fonctionnalités telles que les fonctions table, énumérations et types spatiaux, vous devez cibler .NET Framework 4.5. Visual Studio 2012 cible .NET 4.5 par défaut.

Fonctions table est très semblables aux procédures stockées avec une différence importante : le résultat d’une fonction table est composable. Cela signifie que les résultats à partir d’une fonction table peuvent être utilisés dans une requête LINQ tandis que les résultats d’une procédure stockée ne peut pas.

## <a name="watch-the-video"></a>Regardez la vidéo

**Présenté par**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Conditions préalables

Pour effectuer cette procédure pas à pas, vous devez :

- Installer le [base de données School](~/ef6/resources/school-database.md).

- Une version récente de Visual Studio

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrir Visual Studio
2.  Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**
3.  Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle
4.  Entrez **TVF** en tant que le nom du projet et cliquez sur **OK**

## <a name="add-a-tvf-to-the-database"></a>Ajouter une fonction table à la base de données

-   Sélectionnez **vue -&gt; Explorateur d’objets SQL Server**
-   Si la base de données locale n’est pas dans la liste des serveurs : avec le bouton droit sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser la valeur par défaut **l’authentification Windows** pour se connecter au serveur de base de données locale
-   Développez le nœud de base de données locale
-   Sous le nœud bases de données, cliquez sur le nœud de base de données School et sélectionnez **nouvelle requête...**
-   Dans l’éditeur T-SQL, collez la définition de fonction table suivante

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

-   Cliquez sur le bouton droit de la souris sur l’éditeur T-SQL et sélectionnez **Execute**
-   La fonction GetStudentGradesForCourse est ajoutée à la base de données School

 

## <a name="create-a-model"></a>Créer un modèle

1.  Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**
2.  Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le **modèles** volet
3.  Entrez **TVFModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**
4.  Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**
5.  Cliquez sur **nouvelle connexion** entrée **(localdb)\\mssqllocaldb** dans le texte de nom de serveur zone entrée **School** pour la base de données nom puis cliquez sur **OK**
6.  Dans la choisir vos objets de base de données boîte de dialogue, sous le **Tables** nœud, sélectionnez le **personne**, **StudentGrade**, et **cours** tables
7.  Sélectionnez le **GetStudentGradesForCourse** fonction situé sous le **de procédures stockées et fonctions** nœud Remarque, à compter de Visual Studio 2012, le Concepteur d’entités vous permet à l’importation de lot Procédures stockées et fonctions
8.  Cliquez sur **terminer**
9.  Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche. Tous les objets que vous avez sélectionné dans le **choisir vos objets de base de données** boîte de dialogue sont ajoutés au modèle.
10. Par défaut, la forme du résultat de chaque procédure stockée importée ou la fonction deviennent automatiquement un nouveau type complexe dans votre modèle d’entité. Mais nous voulons mapper les résultats de la fonction GetStudentGradesForCourse à l’entité StudentGrade : cliquez sur l’aire de conception et sélectionnez **Explorateur de modèles** dans Explorateur de modèles, sélectionnez **Function Imports**, puis double-cliquez sur le **GetStudentGradesForCourse** fonction dans le modifier importation de fonction boîte de dialogue, sélectionnez **entités** et choisissez **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier dans lequel la méthode Main est définie. Ajoutez le code suivant dans la fonction Main.

Le code suivant montre comment créer une requête qui utilise une fonction table. La requête projette les résultats dans un type anonyme qui contient le titre du cours connexe et les étudiants associées avec un niveau supérieur ou égal à 3.5.

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

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment mapper des fonctions Table (TVF) à l’aide d’Entity Framework Designer. Il a également montré comment appeler une fonction table à partir d’une requête LINQ.
