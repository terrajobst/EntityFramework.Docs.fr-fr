---
title: Héritage TPT concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996346"
---
# <a name="designer-tpt-inheritance"></a>Héritage TPT Concepteur
Cette procédure pas à pas montre comment implémenter l’héritage de table par type (TPT) dans votre modèle à l’aide d’Entity Framework Designer (Concepteur d’EF). L'héritage TPT (table par type) utilise une table distincte de la base de données pour maintenir des données des propriétés non héritées et des propriétés de clé pour chaque type de la hiérarchie d'héritage.

Dans cette procédure pas à pas, nous allons mapper le **cours** (type de base), **OnlineCourse** (dérive de cours), et **OnsiteCourse** (dérive **cours**) entités à des tables portant le même nom. Nous allons créer un modèle à partir de la base de données, puis la modifier le modèle pour implémenter l’héritage TPT.

Vous pouvez également commencer par le premier modèle, puis générez la base de données à partir du modèle. Le Concepteur EF utilise la stratégie de TPT par défaut et par conséquent, tout l’héritage dans le modèle sera mappée à des tables distinctes.

## <a name="other-inheritance-options"></a>Autres Options d’héritage

Table par hiérarchie (TPH) est un autre type d’héritage dans la base d’une données table est utilisée pour gérer les données de tous les types d’entités dans une hiérarchie d’héritage.  Pour plus d’informations sur le mappage d’héritage Table par hiérarchie avec le Concepteur d’entités, consultez [l’héritage TPH de Concepteur EF](~/ef6/modeling/designer/inheritance/tph.md). 

Notez que, la Table par concret Type héritage (TPC) et l’héritage mixte modèles sont pris en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le Concepteur EF. Si vous souhaitez utiliser TPC ou mixte de l’héritage, vous avez deux options : utiliser Code First, ou modifier manuellement le fichier EDMX. Si vous choisissez d’utiliser le fichier EDMX, la fenêtre Détails de mappage est placée dans le « mode sans échec » et vous ne serez pas en mesure d’utiliser le concepteur pour modifier les mappages.

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

-   Ouvrez Visual Studio 2012.
-   Sélectionnez **fichier -&gt; nouveau -&gt; projet**
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.
-   Entrez **TPTDBFirstSample** comme nom.
-   Sélectionnez **OK**.

## <a name="create-a-model"></a>Créer un modèle

-   Cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **TPTModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez ** Générer à partir de base de données **, puis cliquez sur **suivant**.
-   Cliquez sur **nouvelle connexion**.
    Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la boîte de dialogue Choisir vos objets de base de données, sous le nœud Tables, sélectionnez le **département**, **cours, OnlineCourse et OnsiteCourse** tables.
-   Cliquez sur **Terminer**.

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche. Tous les objets que vous avez sélectionné dans la boîte de dialogue Choisir vos objets de base de données sont ajoutées au modèle.

## <a name="implement-table-per-type-inheritance"></a>Implémenter l’héritage Table par Type

-   Sur l’aire de conception, cliquez sur le **OnlineCourse** type d’entité, puis sélectionnez **propriétés**.
-   Dans le **propriétés** fenêtre, définissez la propriété de Type de Base **cours**.
-   Cliquez sur le **OnsiteCourse** type d’entité, puis sélectionnez **propriétés**.
-   Dans le **propriétés** fenêtre, définissez la propriété de Type de Base **cours**.
-   Avec le bouton droit de l’association (la ligne) entre le **OnlineCourse** et **cours** types d’entité.
    Sélectionnez **supprimer du modèle**.
-   Avec le bouton droit de l’association entre le **OnsiteCourse** et **cours** types d’entité.
    Sélectionnez **supprimer du modèle**.

Nous va maintenant supprimer le **CourseID** propriété à partir de **OnlineCourse** et **OnsiteCourse** étant donné que ces classes héritent **CourseID** à partir de le **cours** type de base.

-   Avec le bouton droit le **CourseID** propriété de la **OnlineCourse** type d’entité, puis **supprimer du modèle**.
-   Avec le bouton droit le **CourseID** propriété de la **OnsiteCourse** type d’entité, puis **supprimer du modèle**
-   L'héritage TPT (table par type) est maintenant implémenté.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Utiliser le modèle

Ouvrez le **Program.cs** fichier où le **Main** méthode est définie. Collez le code suivant dans le **Main** (fonction). Le code s’exécute trois requêtes. La première requête renvoie tous les **cours** associées au département spécifié. La deuxième requête utilise le **OfType** méthode pour retourner **OnlineCourses** associées au département spécifié. Retourne la troisième requête **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
