---
title: Héritage TPH concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 9a546f6450b5aa3b03c062d1ab2c6f9257ba8292
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995002"
---
# <a name="designer-tph-inheritance"></a>Héritage TPH Concepteur
Cette procédure pas à pas montre comment implémenter l’héritage de table par hiérarchie (TPH) dans votre modèle conceptuel avec Entity Framework Designer (Concepteur d’EF). L’héritage TPH utilise une table de base de données pour gérer les données de tous les types d’entités dans une hiérarchie d’héritage.

Dans cette procédure pas à pas, nous allons mapper la table Person à trois types d’entités : Person (type de base), Student (dérive de personne) et Instructor (dérive de personne). Nous allons créer un modèle conceptuel à partir de la base de données (Database First), puis la modifier le modèle pour implémenter l’héritage TPH à l’aide du Concepteur EF.

Il est possible de mapper à un héritage TPH à l’aide de Model First, mais vous seriez obligé d’écrire vos propres flux de travail de génération de base de données qui est complexe. Il vous faut ensuite attribuer ce flux de travail pour le **flux de génération de base de données** propriété dans le Concepteur EF. Une alternative plus simple consiste à utiliser Code First.

## <a name="other-inheritance-options"></a>Autres Options d’héritage

Table par Type (TPT) est un autre type d’héritage dans laquelle des tables distinctes de la base de données sont mappées aux entités qui participent à l’héritage.  Pour plus d’informations sur le mappage d’héritage Table par Type avec le Concepteur d’Entity Framework, consultez [l’héritage TPT de Concepteur EF](~/ef6/modeling/designer/inheritance/tpt.md).

L’héritage de Type de table par concret (TPC) et les modèles d’héritage mixte sont prises en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le Concepteur EF. Si vous souhaitez utiliser TPC ou mixte de l’héritage, vous avez deux options : utiliser Code First, ou modifier manuellement le fichier EDMX. Si vous choisissez d’utiliser le fichier EDMX, la fenêtre Détails de mappage est placée dans le « mode sans échec » et vous ne serez pas en mesure d’utiliser le concepteur pour modifier les mappages.

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

-   Ouvrez Visual Studio 2012.
-   Sélectionnez **fichier -&gt; nouveau -&gt; projet**
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.
-   Entrez **TPHDBFirstSample** comme nom.
-   Sélectionnez **OK**.

## <a name="create-a-model"></a>Créer un modèle

-   Cliquez sur le nom du projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **TPHModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.
-   Cliquez sur **nouvelle connexion**.
    Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la boîte de dialogue Choisir vos objets de base de données, sous le nœud Tables, sélectionnez le **personne** table.
-   Cliquez sur **Terminer**.

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche. Tous les objets que vous avez sélectionné dans la boîte de dialogue Choisir vos objets de base de données sont ajoutées au modèle.

Autrement dit la **personne** table se présente dans la base de données.

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implémenter l’héritage Table par hiérarchie

Le **personne** table comporte le **discriminateur** colonne, ce qui peut avoir une des deux valeurs : « L’étudiant » et « Instructor ». Selon la valeur la **personne** table sera mappée à la **étudiant** entité ou le **Instructor** entité. Le **personne** table possède également deux colonnes, **HireDate** et **EnrollmentDate**, qui doit être **nullable** , car une personne ne peut pas être un stagiaire et formateur en même temps (au moins pas dans cette procédure pas à pas).

### <a name="add-new-entities"></a>Ajouter de nouvelles entités

-   Ajouter une nouvelle entité.
    Pour ce faire, avec le bouton droit sur un espace vide de l’aire de conception d’Entity Framework Designer et sélectionnez **Add -&gt;entité**.
-   Type **Instructor** pour le **nom de l’entité** et sélectionnez **personne** dans la liste déroulante pour le **type de Base**.
-   Cliquez sur **OK**.
-   Ajouter une autre nouvelle entité. Type **étudiant** pour le **nom de l’entité** et sélectionnez **personne** dans la liste déroulante pour le **type de Base**.

Deux nouveaux types d’entités ont été ajoutés à l’aire de conception. Une flèche pointe à partir de nouveaux types d’entités pour le **personne** type d’entité ; cela indique que **personne** est le type de base pour les nouveaux types d’entité.

-   Avec le bouton droit le **HireDate** propriété de la **personne** entité. Sélectionnez **couper** (ou utilisez la touche Ctrl + X).
-   Avec le bouton droit le **Instructor** entité, puis sélectionnez **coller** (ou utilisez la touche Ctrl + V).
-   Cliquez sur le **HireDate** propriété et sélectionnez **propriétés**.
-   Dans le **propriétés** fenêtre, définissez la **Nullable** propriété **false**.
-   Avec le bouton droit le **EnrollmentDate** propriété de la **personne** entité. Sélectionnez **couper** (ou utilisez la touche Ctrl + X).
-   Avec le bouton droit le **étudiant** entité, puis sélectionnez **Coller (ou clé d’utilisation le Ctrl + V).**
-   Sélectionnez le **EnrollmentDate** propriété et définissez la **Nullable** propriété **false**.
-   Sélectionnez le **personne** type d’entité. Dans le **propriétés** fenêtre, définissez son **abstraite** propriété **true**.
-   Supprimer le **discriminateur** propriété à partir de **personne**. La raison pour laquelle qu'il doit être supprimé est expliqué dans la section suivante.

### <a name="map-the-entities"></a>Mapper les entités

-   Avec le bouton droit le **Instructor** et sélectionnez **mappage de Table.**
    L’entité Instructor est sélectionnée dans la fenêtre Détails de Mapping.
-   Cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.
    Le **&lt;ajouter une Table ou vue&gt;** champ devient une liste déroulante de tables ou des vues, auquel l’entité sélectionnée peut être mappée.
-   Sélectionnez **personne** dans la liste déroulante.
-   Le **détails de Mapping** fenêtre est mis à jour avec les mappages de colonnes par défaut et une option permettant d’ajouter une condition.
-   Cliquez sur  **&lt;ajouter une Condition&gt;**.
    Le **&lt;ajouter une Condition&gt;** champ devient une liste déroulante des colonnes pour lesquelles des conditions peuvent être définies.
-   Sélectionnez **discriminateur** dans la liste déroulante.
-   Dans le **opérateur** colonne de la **détails de Mapping** fenêtre, sélectionnez = dans la liste déroulante.
-   Dans le **valeur/propriété** colonne, tapez **Instructor**. Le résultat final doit ressembler à ceci :

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   Répétez ces étapes pour le **étudiant** type d’entité, mais vérifiez la condition égale à **étudiant** valeur.  
    *La raison pour laquelle nous souhaitons supprimer le **discriminateur** propriété, est, car vous ne pouvez pas mapper une colonne de table plusieurs fois. Cette colonne sera utilisée pour le mappage conditionnel, afin qu’il ne peut pas être utilisé pour le mappage de propriété ainsi. La seule façon, il peut être utilisé pour les deux, si une condition utilise une **Is Null** ou **Is Not Null** comparaison.*

L'héritage TPH (table par hiérarchie) est maintenant implémenté.

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Utiliser le modèle

Ouvrez le **Program.cs** fichier où le **Main** méthode est définie. Collez le code suivant dans le **Main** (fonction). Le code s’exécute trois requêtes. La première requête renvoie tous les **personne** objets. La deuxième requête utilise le **OfType** méthode pour retourner **Instructor** objets. La troisième requête utilise le **OfType** méthode pour retourner **étudiant** objets.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
