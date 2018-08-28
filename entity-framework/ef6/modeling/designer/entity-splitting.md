---
title: Fractionnement d’entité concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 214561f0a0381bced3ceae0b6acfcd45f5dd65c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995617"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="6414f-102">Fractionnement d’entité Concepteur</span><span class="sxs-lookup"><span data-stu-id="6414f-102">Designer Entity Splitting</span></span>
<span data-ttu-id="6414f-103">Cette procédure pas à pas montre comment mapper un type d’entité à deux tables en modifiant un modèle avec Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="6414f-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="6414f-104">Vous pouvez mapper une entité à plusieurs tables quand les tables partagent une clé commune.</span><span class="sxs-lookup"><span data-stu-id="6414f-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="6414f-105">Les concepts qui s'appliquent au mappage d'un type d'entité à deux tables sont facilement étendus au mappage d'un type d'entité à plusieurs tables.</span><span class="sxs-lookup"><span data-stu-id="6414f-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="6414f-106">L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="6414f-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="6414f-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6414f-108">Prerequisites</span></span>

<span data-ttu-id="6414f-109">Visual Studio 2012 ou Visual Studio 2010, Premium, Professionnel, Édition Ultimate ou Web Express.</span><span class="sxs-lookup"><span data-stu-id="6414f-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="6414f-110">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="6414f-110">Create the Database</span></span>

<span data-ttu-id="6414f-111">Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :</span><span class="sxs-lookup"><span data-stu-id="6414f-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="6414f-112">Si vous utilisez Visual Studio 2012 puis vous allez créer une base de données de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="6414f-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="6414f-113">Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="6414f-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="6414f-114">Tout d’abord, nous allons créer une base de données avec deux tables que nous allons à combiner en une seule entité.</span><span class="sxs-lookup"><span data-stu-id="6414f-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="6414f-115">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6414f-115">Open Visual Studio</span></span>
-   <span data-ttu-id="6414f-116">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="6414f-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6414f-117">Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="6414f-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6414f-118">Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données</span><span class="sxs-lookup"><span data-stu-id="6414f-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="6414f-119">Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé</span><span class="sxs-lookup"><span data-stu-id="6414f-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="6414f-120">Entrez **EntitySplitting** en tant que le nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="6414f-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="6414f-121">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="6414f-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="6414f-122">La nouvelle base de données s’affiche désormais dans l’Explorateur de serveurs</span><span class="sxs-lookup"><span data-stu-id="6414f-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="6414f-123">Si vous utilisez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6414f-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="6414f-124">Avec le bouton droit sur la base de données dans l’Explorateur de serveurs, puis sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="6414f-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="6414f-125">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="6414f-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="6414f-126">Si vous utilisez Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6414f-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="6414f-127">Sélectionnez **données -&gt; Transact SQL éditeur -&gt; nouvelle connexion à la requête...**</span><span class="sxs-lookup"><span data-stu-id="6414f-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="6414f-128">Entrez **.\\ SQLEXPRESS** en tant que nom du serveur et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="6414f-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="6414f-129">Sélectionnez le **EntitySplitting** de base de données dans la liste déroulante vers le bas en haut de l’éditeur de requête</span><span class="sxs-lookup"><span data-stu-id="6414f-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="6414f-130">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **exécuter SQL**</span><span class="sxs-lookup"><span data-stu-id="6414f-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="6414f-131">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="6414f-131">Create the Project</span></span>

-   <span data-ttu-id="6414f-132">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="6414f-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="6414f-133">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Application Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="6414f-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="6414f-134">Entrez **MapEntityToTablesSample** en tant que le nom du projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6414f-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="6414f-135">Cliquez sur **non** si vous êtes invité à enregistrer la requête SQL créée dans la première section.</span><span class="sxs-lookup"><span data-stu-id="6414f-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="6414f-136">Créer un modèle basé sur la base de données</span><span class="sxs-lookup"><span data-stu-id="6414f-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="6414f-137">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="6414f-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="6414f-138">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="6414f-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="6414f-139">Entrez **MapEntityToTablesModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6414f-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="6414f-140">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**</span><span class="sxs-lookup"><span data-stu-id="6414f-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="6414f-141">Sélectionnez le **EntitySplitting** connexion dans la liste déroulante et cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="6414f-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="6414f-142">Dans la boîte de dialogue Choisir vos objets de base de données, cochez la case à côté du **Tables** nœud.</span><span class="sxs-lookup"><span data-stu-id="6414f-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="6414f-143">Cette opération ajoute toutes les tables à partir de la **EntitySplitting** base de données au modèle.</span><span class="sxs-lookup"><span data-stu-id="6414f-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="6414f-144">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="6414f-144">Click **Finish**.</span></span>

<span data-ttu-id="6414f-145">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6414f-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="6414f-146">Mapper une entité à deux Tables</span><span class="sxs-lookup"><span data-stu-id="6414f-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="6414f-147">Dans cette étape nous mettrons à jour le **personne** type d’entité à combiner des données à partir de la **personne** et **PersonInfo** tables.</span><span class="sxs-lookup"><span data-stu-id="6414f-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="6414f-148">Sélectionnez le **E-mail** et **téléphone** propriétés de la ** PersonInfo ** entité, puis appuyez sur **Ctrl + X** clés.</span><span class="sxs-lookup"><span data-stu-id="6414f-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="6414f-149">Sélectionnez le ** personne ** entité, puis appuyez sur **Ctrl + V** clés.</span><span class="sxs-lookup"><span data-stu-id="6414f-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="6414f-150">Sur l’aire de conception, sélectionnez le **PersonInfo** entité, puis appuyez sur **supprimer** bouton sur le clavier.</span><span class="sxs-lookup"><span data-stu-id="6414f-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="6414f-151">Cliquez sur **non** lorsque vous êtes invité à supprimer le **PersonInfo** table à partir du modèle, nous sommes sur le point de la mapper à la **personne** entité.</span><span class="sxs-lookup"><span data-stu-id="6414f-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![DeleteTables](~/ef6/media/deletetables.png)

<span data-ttu-id="6414f-153">Les étapes suivantes requièrent le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="6414f-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="6414f-154">Si vous ne voyez pas cette fenêtre, cliquez sur l’aire de conception et sélectionnez **détails de Mapping**.</span><span class="sxs-lookup"><span data-stu-id="6414f-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="6414f-155">Sélectionnez le **personne** type d’entité et cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="6414f-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="6414f-156">Sélectionnez ** PersonInfo ** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="6414f-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="6414f-157">Le **détails de Mapping** fenêtre est mis à jour avec les mappages de colonnes par défaut, le résultat est parfaits pour notre scénario.</span><span class="sxs-lookup"><span data-stu-id="6414f-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="6414f-158">Le **personne** type d’entité est maintenant mappé à la **personne** et **PersonInfo** tables.</span><span class="sxs-lookup"><span data-stu-id="6414f-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="6414f-160">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="6414f-160">Use the Model</span></span>

-   <span data-ttu-id="6414f-161">Collez le code suivant dans la méthode Main.</span><span class="sxs-lookup"><span data-stu-id="6414f-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="6414f-162">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="6414f-162">Compile and run the application.</span></span>

<span data-ttu-id="6414f-163">Les instructions T-SQL suivantes ont été exécutées par rapport à la base de données suite à l’exécution de cette application.</span><span class="sxs-lookup"><span data-stu-id="6414f-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="6414f-164">Les deux **insérer** instructions ont été exécutées suite à l’exécution de contexte. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="6414f-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="6414f-165">Ils prennent les données à partir de la **personne** entité et la fractionner entre le **personne** et **PersonInfo** tables.</span><span class="sxs-lookup"><span data-stu-id="6414f-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   <span data-ttu-id="6414f-168">Ce qui suit **sélectionnez** a été exécutée à la suite de l’énumération des personnes de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6414f-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="6414f-169">Il combine les données à partir de la **personne** et **PersonInfo** table.</span><span class="sxs-lookup"><span data-stu-id="6414f-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Sélectionner](~/ef6/media/select.png)
