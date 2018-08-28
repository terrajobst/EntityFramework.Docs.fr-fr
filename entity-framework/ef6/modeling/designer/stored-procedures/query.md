---
title: Requête concepteur procédures stockées - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 29b7745c2229ce4a38ad81e11406474424adfa24
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994970"
---
# <a name="designer-query-stored-procedures"></a>Requêtes du Générateur de procédures stockées
Cette procédure pas à pas montrent comment utiliser Entity Framework Designer (Concepteur d’EF) pour importer des procédures stockées dans un modèle, puis appelez les procédures stockées importés pour récupérer les résultats. 

Notez que Code First ne prend pas en charge le mappage de procédures stockées ou fonctions. Toutefois, vous pouvez appeler des procédures stockées ou fonctions à l’aide de la méthode System.Data.Entity.DbSet.SqlQuery. Exemple :
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

-   Ouvrez Visual Studio 2012.
-   Sélectionnez **fichier -&gt; nouveau -&gt; projet**
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.
-   Entrez **EFwithSProcsSample** comme nom.
-   Sélectionnez **OK**.

## <a name="create-a-model"></a>Créer un modèle

-   Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **Add -&gt; un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **EFwithSProcsModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.
-   Cliquez sur **nouvelle connexion**.  
    Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.  
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la boîte de dialogue Choisir vos objets de base de données, vérifiez le **Tables** case à cocher pour sélectionner toutes les tables.  
    En outre, sélectionnez les procédures stockées suivantes sous la **de procédures stockées et fonctions** nœud : **GetStudentGrades** et **GetDepartmentName**. 

    ![Import](~/ef6/media/import.jpg)

    *À partir de Visual Studio 2012, le Concepteur EF prend en charge l’importation en bloc des procédures stockées. Le **importation sélectionné des procédures stockées et fonctions dans theentity modèle** est activée par défaut.*
-   Cliquez sur **Terminer**.

Par défaut, la forme du résultat de chaque procédure stockée importée ou d’une fonction qui retourne plusieurs colonnes deviennent automatiquement un nouveau type complexe. Dans cet exemple, nous voulons mapper les résultats de la **GetStudentGrades** fonctionner à la **StudentGrade** entité et les résultats de la **GetDepartmentName** à **aucun** (**aucun** est la valeur par défaut).

Pour une importation de fonction retourner un type d’entité, les colonnes retournées par la procédure stockée correspondante doivent correspondre exactement les propriétés scalaires du type d’entité retourné. Une importation de fonction peut également retourner des collections de types simples, des types complexes ou aucune valeur.

-   Cliquez sur l’aire de conception et sélectionnez **Explorateur de modèles**.
-   Dans **Explorateur de modèles**, sélectionnez **Function Imports**, puis double-cliquez sur le **GetStudentGrades** (fonction).
-   Dans la boîte de dialogue Modifier une importation de fonction, sélectionnez **entités** et choisissez **StudentGrade**.  
    *Le **importation de fonction est composable** case à cocher en haut de la **Function Imports** boîte de dialogue vous permet de mapper aux fonctions composables. Si vous ne cochez pas cette case, seules les fonctions composables (fonctions table) seront affiche dans le **procédure stockée / nom de la fonction** liste déroulante. Si vous ne cochez pas cette case, seules les fonctions non composables seront affichera dans la liste.*

## <a name="use-the-model"></a>Utiliser le modèle

Ouvrez le **Program.cs** fichier où le **Main** méthode est définie. Ajoutez le code suivant dans la fonction Main.

Le code appelle deux procédures stockées : **GetStudentGrades** (retourne **StudentGrades** spécifié *StudentId*) et **GetDepartmentName** (retourne le nom du service dans le paramètre de sortie).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Paramètres de sortie
-----------------

Si les paramètres de sortie sont utilisés, leurs valeurs ne sera pas disponibles jusqu'à ce que les résultats ont été lues entièrement. Il s’agit en raison du comportement sous-jacent de DbDataReader, consultez [extraction de données à l’aide d’un DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) pour plus d’informations.
