---
title: Fractionnement de la Table concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120822"
---
# <a name="designer-table-splitting"></a>Table du Concepteur de fractionnement
Cette procédure pas à pas montre comment mapper plusieurs types d’entités à une table unique en modifiant un modèle avec Entity Framework Designer (Concepteur d’EF).

L’une des raisons que vous souhaiterez utiliser le fractionnement de table sont ce qui peut retarder le chargement de certaines propriétés lorsque vous utilisez le chargement pour charger vos objets différé. Vous pouvez séparer les propriétés qui peuvent contenir de très grande quantité de données dans une entité distincte et chargez uniquement si nécessaire.

L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

Cette procédure pas à pas utilise Visual Studio 2012.

-   Ouvrez Visual Studio 2012.
-   Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.
-   Dans le volet gauche, cliquez sur Visual C\#, puis sélectionnez le modèle Application Console.
-   Entrez **TableSplittingSample** en tant que le nom du projet et cliquez sur **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Créer un modèle basé sur la base de données School

-   Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **TableSplittingModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**
-   Cliquez sur Nouvelle connexion. Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la boîte de dialogue Choisir vos objets de base de données, dérouler les **Tables** nœud et vérifiez la **personne** table. Cette opération ajoute la table spécifiée à la **School** modèle.
-   Cliquez sur **Terminer**.

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche. Tous les objets que vous avez sélectionné dans le **choisir vos objets de base de données** boîte de dialogue sont ajoutés au modèle.

## <a name="map-two-entities-to-a-single-table"></a>Mapper des deux entités à une seule Table

Dans cette section, vous allez fractionner la **personne** entité en deux entités puis des mapper à une seule table.

> [!NOTE]
> Le **personne** entité ne contient pas toutes les propriétés qui peuvent contenir des données volumineuses ; il est simplement utilisé comme un exemple.

-   Cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **entité**.
    Le **nouvelle entité** boîte de dialogue s’affiche.
-   Type **HireInfo** pour le **nom de l’entité** et **PersonID** pour le **propriété Key** nom.
-   Cliquez sur **OK**.
-   Un nouveau type d'entité est créé et affiché sur l'aire de conception.
-   Sélectionnez le **HireDate** propriété de la **personne** type d’entité, puis appuyez sur **Ctrl + X** clés.
-   Sélectionnez le **HireInfo** entité, puis appuyez sur **Ctrl + V** clés.
-   Créer une association entre **personne** et **HireInfo**. Pour ce faire, cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **Association**.
-   Le **ajouter une Association** boîte de dialogue s’affiche. Le **PersonHireInfo** nom est attribué par défaut.
-   Spécifier la multiplicité **1(One)** aux deux extrémités de la relation.
-   Appuyez sur **OK**.

L’étape suivante requiert la **détails de Mapping** fenêtre. Si vous ne voyez pas cette fenêtre, cliquez sur l’aire de conception et sélectionnez **détails de Mapping**.

-   Sélectionnez le **HireInfo** type d’entité et cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.
-   Sélectionnez **personne** à partir de la **&lt;ajouter une Table ou vue&gt;** liste de champs de liste déroulante. La liste contient des tables ou des vues, auquel l’entité sélectionnée peut être mappée.
    Les propriétés appropriées doivent être mappées par défaut.

    ![Mappage](~/ef6/media/mapping.png)

-   Sélectionnez le **PersonHireInfo** association sur l’aire de conception.
-   Avec le bouton droit sur l’aire de conception et sélectionnez l’association de **propriétés**.
-   Dans le **propriétés** fenêtre, sélectionnez le **contraintes référentielles** propriété et cliquez sur le bouton de sélection.
-   Sélectionnez **personne** à partir de la **Principal** liste déroulante.
-   Appuyez sur **OK**.

 

## <a name="use-the-model"></a>Utiliser le modèle

-   Collez le code suivant dans la méthode Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Compilez et exécutez l'application.

Les instructions T-SQL suivantes ont été exécutées sur le **School** base de données suite à l’exécution de cette application. 

-   Ce qui suit **insérer** a été exécutée suite à l’exécution de contexte. SaveChanges() et combine les données à partir de la **personne** et **HireInfo** entités

    ![Insert](~/ef6/media/insert.png)

-   Ce qui suit **sélectionnez** a été exécutée suite à l’exécution de contexte. People.FirstOrDefault() et sélectionne uniquement les colonnes mappées aux **personne**

    ![Select1](~/ef6/media/select1.png)

-   Ce qui suit **sélectionnez** a été exécutée à la suite de l’accès à la existingPerson.Instructor de propriété de navigation et sélectionne uniquement les colonnes mappées aux **HireInfo**

    ![Select2](~/ef6/media/select2.png)
