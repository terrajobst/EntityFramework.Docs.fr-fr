---
title: Fractionnement d’entité du concepteur-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418461"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="de4c5-102">Fractionnement d’entité du concepteur</span><span class="sxs-lookup"><span data-stu-id="de4c5-102">Designer Entity Splitting</span></span>
<span data-ttu-id="de4c5-103">Cette procédure pas à pas montre comment mapper un type d’entité à deux tables en modifiant un modèle avec le Entity Framework Designer (concepteur EF).</span><span class="sxs-lookup"><span data-stu-id="de4c5-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="de4c5-104">Vous pouvez mapper une entité à plusieurs tables quand les tables partagent une clé commune.</span><span class="sxs-lookup"><span data-stu-id="de4c5-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="de4c5-105">Les concepts qui s’appliquent au mappage d’un type d’entité à deux tables sont facilement étendus pour le mappage d’un type d’entité à plus de deux tables.</span><span class="sxs-lookup"><span data-stu-id="de4c5-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="de4c5-106">L’illustration suivante montre les fenêtres principales qui sont utilisées lors de l’utilisation du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="de4c5-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="de4c5-108">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="de4c5-108">Prerequisites</span></span>

<span data-ttu-id="de4c5-109">Visual Studio 2012 ou Visual Studio 2010, édition intégrale, Premium, Professional ou Web Express.</span><span class="sxs-lookup"><span data-stu-id="de4c5-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="de4c5-110">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="de4c5-110">Create the Database</span></span>

<span data-ttu-id="de4c5-111">Le serveur de base de données installé avec Visual Studio diffère selon la version de Visual Studio que vous avez installée :</span><span class="sxs-lookup"><span data-stu-id="de4c5-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="de4c5-112">Si vous utilisez Visual Studio 2012, vous allez créer une base de données de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="de4c5-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="de4c5-113">Si vous utilisez Visual Studio 2010, vous allez créer une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="de4c5-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="de4c5-114">Tout d’abord, nous allons créer une base de données avec deux tables que nous allons combiner dans une entité unique.</span><span class="sxs-lookup"><span data-stu-id="de4c5-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="de4c5-115">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de4c5-115">Open Visual Studio</span></span>
-   <span data-ttu-id="de4c5-116">**Vue-&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="de4c5-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="de4c5-117">Cliquez avec le bouton droit sur **connexions de données-&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="de4c5-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="de4c5-118">Si vous n’êtes pas connecté à une base de données à partir de Explorateur de serveurs avant de devoir sélectionner **Microsoft SQL Server** comme source de données</span><span class="sxs-lookup"><span data-stu-id="de4c5-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="de4c5-119">Connectez-vous à la base de données locale ou SQL Express, en fonction de celle que vous avez installée</span><span class="sxs-lookup"><span data-stu-id="de4c5-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="de4c5-120">Entrez **EntitySplitting** comme nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="de4c5-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="de4c5-121">Sélectionnez **OK** . vous serez invité à créer une nouvelle base de données, sélectionnez **Oui** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="de4c5-122">La nouvelle base de données s’affiche à présent dans Explorateur de serveurs</span><span class="sxs-lookup"><span data-stu-id="de4c5-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="de4c5-123">Si vous utilisez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="de4c5-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="de4c5-124">Dans Explorateur de serveurs, cliquez avec le bouton droit sur la base de données, puis sélectionnez **nouvelle requête** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="de4c5-125">Copiez le code SQL suivant dans la nouvelle requête, cliquez avec le bouton droit sur la requête et sélectionnez **exécuter** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="de4c5-126">Si vous utilisez Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="de4c5-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="de4c5-127">Sélectionner **les données-&gt; l’éditeur Transact SQL-&gt; nouvelle connexion à la requête...**</span><span class="sxs-lookup"><span data-stu-id="de4c5-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="de4c5-128">Entrez **.\\SQLExpress** comme nom de serveur, puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="de4c5-129">Sélectionnez la base de données **EntitySplitting** dans la liste déroulante en haut de l’éditeur de requête.</span><span class="sxs-lookup"><span data-stu-id="de4c5-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="de4c5-130">Copiez le code SQL suivant dans la nouvelle requête, cliquez avec le bouton droit sur la requête et sélectionnez **Exécuter SQL** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="de4c5-131">Création du projet</span><span class="sxs-lookup"><span data-stu-id="de4c5-131">Create the Project</span></span>

-   <span data-ttu-id="de4c5-132">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="de4c5-133">Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **application console** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="de4c5-134">Entrez **MapEntityToTablesSample** comme nom du projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="de4c5-135">Cliquez sur **non** si vous êtes invité à enregistrer la requête SQL créée dans la première section.</span><span class="sxs-lookup"><span data-stu-id="de4c5-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="de4c5-136">Créer un modèle basé sur la base de données</span><span class="sxs-lookup"><span data-stu-id="de4c5-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="de4c5-137">Cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="de4c5-138">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.</span><span class="sxs-lookup"><span data-stu-id="de4c5-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="de4c5-139">Entrez **MapEntityToTablesModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="de4c5-140">Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**</span><span class="sxs-lookup"><span data-stu-id="de4c5-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="de4c5-141">Sélectionnez la connexion **EntitySplitting** dans la liste déroulante, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="de4c5-142">Dans la boîte de dialogue choisir vos objets de base de données, activez la case à cocher en regard du nœud **Tables** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="de4c5-143">Cette opération ajoute toutes les tables de la base de données **EntitySplitting** au modèle.</span><span class="sxs-lookup"><span data-stu-id="de4c5-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="de4c5-144">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-144">Click **Finish**.</span></span>

<span data-ttu-id="de4c5-145">Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché.</span><span class="sxs-lookup"><span data-stu-id="de4c5-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="de4c5-146">Mapper une entité à deux tables</span><span class="sxs-lookup"><span data-stu-id="de4c5-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="de4c5-147">Dans cette étape, nous allons mettre à jour le type d’entité **Person** pour combiner les données des tables **Person** et **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="de4c5-148">Sélectionnez les de **messagerie** et les propriétés de **téléphone** de l’entité **PersonInfo **, puis appuyez sur les touches **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="de4c5-149">Sélectionnez l’entité **Person **et appuyez sur les touches **Ctrl + V** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="de4c5-150">Sur l’aire de conception, sélectionnez l’entité **PersonInfo** et appuyez sur la touche **Suppr** du clavier.</span><span class="sxs-lookup"><span data-stu-id="de4c5-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="de4c5-151">Cliquez sur **non** lorsque vous y êtes invité si vous souhaitez supprimer la table **PersonInfo** du modèle, nous sommes sur le sujet de la mapper à l’entité **Person** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Supprimer des tables](~/ef6/media/deletetables.png)

<span data-ttu-id="de4c5-153">Les étapes suivantes nécessitent la fenêtre **Détails de mappage** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="de4c5-154">Si vous ne voyez pas cette fenêtre, cliquez avec le bouton droit sur l’aire de conception, puis sélectionnez **Détails de mapping**.</span><span class="sxs-lookup"><span data-stu-id="de4c5-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="de4c5-155">Sélectionnez la **personne** type d’entité, puis cliquez sur **&lt;ajouter une table ou une vue&gt;**  dans la fenêtre des **Détails de mappage** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="de4c5-156">Sélectionnez **PersonInfo ** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="de4c5-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="de4c5-157">La fenêtre de des **Détails de mappage** est mise à jour avec les mappages de colonnes par défaut, ce qui est parfait pour notre scénario.</span><span class="sxs-lookup"><span data-stu-id="de4c5-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="de4c5-158">La **personne** type d’entité est maintenant mappé aux tables **Person** et **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mappage 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="de4c5-160">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="de4c5-160">Use the Model</span></span>

-   <span data-ttu-id="de4c5-161">Collez le code suivant dans la méthode main.</span><span class="sxs-lookup"><span data-stu-id="de4c5-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="de4c5-162">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="de4c5-162">Compile and run the application.</span></span>

<span data-ttu-id="de4c5-163">Les instructions T-SQL suivantes ont été exécutées sur la base de données suite à l’exécution de cette application.</span><span class="sxs-lookup"><span data-stu-id="de4c5-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="de4c5-164">Les deux instructions **Insert** suivantes ont été exécutées à la suite de l’exécution du contexte. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="de4c5-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="de4c5-165">Ils prennent les données de l’entité **Person** et les fractionnent entre les tables **Person** et **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insérer 1](~/ef6/media/insert1.png)

    ![Insérer 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="de4c5-168">La commande **Select** suivante a été exécutée suite à l’énumération des personnes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="de4c5-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="de4c5-169">Il combine les données de la table **Person** et **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="de4c5-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Sélectionnez](~/ef6/media/select.png)
