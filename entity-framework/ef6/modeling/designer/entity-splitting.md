---
title: Fractionnement d’entité concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250594"
---
# <a name="designer-entity-splitting"></a>Fractionnement d’entité Concepteur
Cette procédure pas à pas montre comment mapper un type d’entité à deux tables en modifiant un modèle avec Entity Framework Designer (Concepteur d’EF). Vous pouvez mapper une entité à plusieurs tables quand les tables partagent une clé commune. Les concepts qui s'appliquent au mappage d'un type d'entité à deux tables sont facilement étendus au mappage d'un type d'entité à plusieurs tables.

L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prérequis

Visual Studio 2012 ou Visual Studio 2010, Premium, Professionnel, Édition Ultimate ou Web Express.

## <a name="create-the-database"></a>Créer la base de données

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :

-   Si vous utilisez Visual Studio 2012 puis vous allez créer une base de données de base de données locale.
-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.

Tout d’abord, nous allons créer une base de données avec deux tables que nous allons à combiner en une seule entité.

-   Ouvrir Visual Studio
-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données
-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé
-   Entrez **EntitySplitting** en tant que le nom de la base de données
-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**
-   La nouvelle base de données s’affiche désormais dans l’Explorateur de serveurs
-   Si vous utilisez Visual Studio 2012
    -   Avec le bouton droit sur la base de données dans l’Explorateur de serveurs, puis sélectionnez **nouvelle requête**
    -   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**
-   Si vous utilisez Visual Studio 2010
    -   Sélectionnez **données -&gt; Transact SQL éditeur -&gt; nouvelle connexion à la requête...**
    -   Entrez **.\\ SQLEXPRESS** en tant que nom du serveur et cliquez sur **OK**
    -   Sélectionnez le **EntitySplitting** de base de données dans la liste déroulante vers le bas en haut de l’éditeur de requête
    -   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **exécuter SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Créer le projet

-   Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Application Console** modèle.
-   Entrez **MapEntityToTablesSample** en tant que le nom du projet et cliquez sur **OK**.
-   Cliquez sur **non** si vous êtes invité à enregistrer la requête SQL créée dans la première section.

## <a name="create-a-model-based-on-the-database"></a>Créer un modèle basé sur la base de données

-   Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **MapEntityToTablesModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**
-   Sélectionnez le **EntitySplitting** connexion dans la liste déroulante et cliquez sur **suivant**.
-   Dans la boîte de dialogue Choisir vos objets de base de données, cochez la case à côté du **Tables** nœud.
    Cette opération ajoute toutes les tables à partir de la **EntitySplitting** base de données au modèle.
-   Cliquez sur **Terminer**.

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.

## <a name="map-an-entity-to-two-tables"></a>Mapper une entité à deux Tables

Dans cette étape nous mettrons à jour le **personne** type d’entité à combiner des données à partir de la **personne** et **PersonInfo** tables.

-   Sélectionnez le **E-mail** et **téléphone** propriétés de la ** PersonInfo ** entité, puis appuyez sur **Ctrl + X** clés.
-   Sélectionnez le ** personne ** entité, puis appuyez sur **Ctrl + V** clés.
-   Sur l’aire de conception, sélectionnez le **PersonInfo** entité, puis appuyez sur **supprimer** bouton sur le clavier.
-   Cliquez sur **non** lorsque vous êtes invité à supprimer le **PersonInfo** table à partir du modèle, nous sommes sur le point de la mapper à la **personne** entité.

    ![Supprimer des Tables](~/ef6/media/deletetables.png)

Les étapes suivantes requièrent le **détails de Mapping** fenêtre. Si vous ne voyez pas cette fenêtre, cliquez sur l’aire de conception et sélectionnez **détails de Mapping**.

-   Sélectionnez le **personne** type d’entité et cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.
-   Sélectionnez ** PersonInfo ** dans la liste déroulante.
    Le **détails de Mapping** fenêtre est mis à jour avec les mappages de colonnes par défaut, le résultat est parfaits pour notre scénario.

Le **personne** type d’entité est maintenant mappé à la **personne** et **PersonInfo** tables.

![Mappage de 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Utiliser le modèle

-   Collez le code suivant dans la méthode Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Compilez et exécutez l'application.

Les instructions T-SQL suivantes ont été exécutées par rapport à la base de données suite à l’exécution de cette application. 

-   Les deux **insérer** instructions ont été exécutées suite à l’exécution de contexte. SaveChanges(). Ils prennent les données à partir de la **personne** entité et la fractionner entre le **personne** et **PersonInfo** tables.

    ![Insérer 1](~/ef6/media/insert1.png)

    ![Insérer 2](~/ef6/media/insert2.png)
-   Ce qui suit **sélectionnez** a été exécutée à la suite de l’énumération des personnes de la base de données. Il combine les données à partir de la **personne** et **PersonInfo** table.

    ![Sélectionner](~/ef6/media/select.png)
