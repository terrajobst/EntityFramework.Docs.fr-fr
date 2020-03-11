---
title: Concepteur TPH héritage-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418426"
---
# <a name="designer-tph-inheritance"></a>Héritage TPH du concepteur
Cette procédure pas à pas montre comment implémenter l’héritage TPH (table par hiérarchie) dans votre modèle conceptuel avec l’Entity Framework Designer (concepteur EF). L’héritage TPH utilise une table de base de données pour conserver les données de tous les types d’entité dans une hiérarchie d’héritage.

Dans cette procédure pas à pas, nous allons mapper la table Person à trois types d’entité : person (type de base), Student (Derived person) et Instructor (dérive de Person). Nous allons créer un modèle conceptuel à partir de la base de données (Database First), puis modifier le modèle pour implémenter l’héritage TPH à l’aide du concepteur EF.

Il est possible de mapper à un héritage TPH à l’aide de Model First, mais vous devez écrire votre propre flux de travail de génération de base de données, ce qui est complexe. Vous affectez ensuite ce flux de travail à la propriété **de flux de travail de génération de base de données** dans le concepteur EF. Une alternative plus simple consiste à utiliser Code First.

## <a name="other-inheritance-options"></a>Autres options d’héritage

La table par type (TPT) est un autre type d’héritage dans lequel des tables distinctes de la base de données sont mappées à des entités qui participent à l’héritage.  Pour plus d’informations sur la façon de mapper l’héritage TPT (table par type) à l’aide du concepteur EF, consultez [EF designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).

L’héritage de type table par béton (TPC) et les modèles d’héritage mixte sont pris en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le concepteur EF. Si vous souhaitez utiliser l’héritage TPC ou mixte, deux options s’offrent à vous : Utilisez Code First ou modifiez manuellement le fichier EDMX. Si vous choisissez de travailler avec le fichier EDMX, la fenêtre Détails de mappage est placée en « mode sans échec » et vous ne pouvez pas utiliser le concepteur pour modifier les mappages.

## <a name="prerequisites"></a>Conditions préalables requises

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- [Exemple de base de données School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

-   Ouvrez Visual Studio 2012.
-   Sélectionnez **fichier-&gt; nouveau-&gt; projet**
-   Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **console** .
-   Entrez **TPHDBFirstSample** comme nom.
-   Sélectionnez **OK**.

## <a name="create-a-model"></a>Création d'un modèle

-   Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez **Ajouter-&gt; nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.
-   Entrez **TPHModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter**.
-   Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.
-   Cliquez sur **nouvelle connexion**.
    Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, (base de données locale **)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis cliquez sur **OK**.
    La boîte de dialogue choisir votre connexion de données est mise à jour avec votre paramètre de connexion à la base de données.
-   Dans la boîte de dialogue choisir vos objets de base de données, sous le nœud tables, sélectionnez la table **Person** .
-   Cliquez sur **Terminer**.

Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché. Tous les objets que vous avez sélectionnés dans la boîte de dialogue choisir vos objets de base de données sont ajoutés au modèle.

C’est ainsi que la table **Person** apparaît dans la base de données.

![Table Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implémenter l’héritage TPH (table par hiérarchie)

La table **Person** contient la colonne de **discriminateur** , qui peut avoir l’une des deux valeurs suivantes : « Student » et « Instructor ». Selon la valeur, la table **Person** est mappée à l’entité **Student** ou à l’entité **Instructor** . La table **Person** possède également deux colonnes : **HireDate** et **EnrollmentDate**, qui doit être **Nullable** , car une personne ne peut pas être un étudiant et un formateur en même temps (au moins dans cette procédure pas à pas).

### <a name="add-new-entities"></a>Ajouter de nouvelles entités

-   Ajoutez une nouvelle entité.
    Pour ce faire, cliquez avec le bouton droit sur un espace vide de l’aire de conception du Entity Framework Designer, puis sélectionnez **Ajouter-&gt;entité**.
-   Tapez **Instructor** pour le nom de l' **entité** et sélectionnez **Person** dans la liste déroulante du **type de base**.
-   Cliquez sur  **OK**.
-   Ajoutez une autre nouvelle entité. Tapez **student** pour le nom de l' **entité** et sélectionnez **Person** dans la liste déroulante du **type de base**.

Deux nouveaux types d’entité ont été ajoutés à l’aire de conception. Une flèche pointe des nouveaux types d’entités vers la **personne** type d’entité ; Cela indique que **Person** est le type de base pour les nouveaux types d’entités.

-   Cliquez avec le bouton droit sur la propriété **hiredate** de l’entité **Person** . Sélectionnez **couper** (ou utilisez la touche Ctrl-X).
-   Cliquez avec le bouton droit sur l’entité de l' **instructeur** , puis sélectionnez **coller** (ou utilisez la touche Ctrl-V).
-   Cliquez avec le bouton droit sur la propriété **hiredate** , puis sélectionnez **Propriétés**.
-   Dans la fenêtre  **Propriétés** , affectez la valeur **false**à la propriété **Nullable** .
-   Cliquez avec le bouton droit sur la propriété **EnrollmentDate** de l’entité **Person** . Sélectionnez **couper** (ou utilisez la touche Ctrl-X).
-   Cliquez avec le bouton droit sur l’entité **Student** et sélectionnez **Coller (ou utilisez la touche Ctrl-V).**
-   Sélectionnez la propriété **EnrollmentDate** et affectez la valeur **false**à la propriété **Nullable** .
-   Sélectionnez la **personne** type d’entité. Dans la fenêtre **propriétés** , affectez la valeur **true**à sa propriété **abstract** .
-   Supprimez la propriété de **discriminateur** de **Person**. La raison pour laquelle elle doit être supprimée est expliquée dans la section suivante.

### <a name="map-the-entities"></a>Mapper les entités

-   Cliquez avec le bouton droit sur l' **instructeur** et sélectionnez **mappage de table.**
    L’entité Instructor est sélectionnée dans la fenêtre Détails de mappage.
-   Cliquez sur **&lt;ajouter une table ou une vue&gt;**  dans la fenêtre des **Détails de mappage** .
    Le **&lt;ajouter une table ou une vue&gt;** champ devient une liste déroulante de tables ou de vues dans laquelle l’entité sélectionnée peut être mappée.
-   Sélectionnez **Person** dans la liste déroulante.
-   La fenêtre des **Détails de mappage** est mise à jour avec les mappages de colonnes par défaut et une option pour ajouter une condition.
-   Cliquez sur **&lt;ajouter une Condition&gt;** .
    Le **&lt;ajouter une Condition&gt;** champ devient une liste déroulante des colonnes pour lesquelles des conditions peuvent être définies.
-   Sélectionnez de **discriminateur** dans la liste déroulante.
-   Dans la colonne **opérateur** de la fenêtre des **Détails de mappage** , sélectionnez = dans la liste déroulante.
-   Dans la colonne **valeur/propriété** , tapez **Instructor**. Le résultat final doit ressembler à ceci :

    ![Détails de mappage](~/ef6/media/mappingdetails2.png)

-   Répétez ces étapes pour le type d’entité  **Student** , mais faites en sorte que la condition soit égale à la valeur **Student** .  
    *La raison pour laquelle nous voulions supprimer la propriété de **discriminateur** , c’est parce que vous ne pouvez pas mapper une colonne de table plusieurs fois. Cette colonne sera utilisée pour le mappage conditionnel. elle ne peut donc pas être utilisée pour le mappage de propriété. La seule façon de pouvoir les utiliser pour les deux, si une condition utilise **est null** ou **n’est pas null** comparaison.*

L'héritage TPH (table par hiérarchie) est maintenant implémenté.

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Utiliser le modèle

Ouvrez le fichier **Program.cs** dans lequel la méthode **main** est définie. Collez le code suivant dans la fonction **main** . Le code exécute trois requêtes. La première requête rétablit tous les objets **Person** . La deuxième requête utilise la méthode **OfType** pour retourner des objets de **formateur** . La troisième requête utilise la méthode **OfType** pour retourner les objets **Student** .

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
