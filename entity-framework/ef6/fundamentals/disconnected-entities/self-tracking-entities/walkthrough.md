---
title: Entités procédure pas à pas - EF6 de suivi automatique
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 1c450bbb20c246d9b9d58707ac03eb48eadfa970
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251282"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="f4d32-102">Procédure pas à pas de suivi automatique entités</span><span class="sxs-lookup"><span data-stu-id="f4d32-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f4d32-103">Nous ne recommandons plus d’utiliser le modèle des entités de suivi automatique.</span><span class="sxs-lookup"><span data-stu-id="f4d32-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="f4d32-104">Il reste disponible uniquement pour prendre en charge les applications existantes.</span><span class="sxs-lookup"><span data-stu-id="f4d32-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="f4d32-105">Si votre application doit utiliser des graphiques d’entités déconnectés, envisagez d’autres alternatives comme les [Entités traçables](http://trackableentities.github.io/), qui présentent une technologie similaire aux entités de suivi automatique, mais développée de manière plus active par la communauté, ou bien l’écriture de code personnalisé à l’aide des API de suivi de changements de bas niveau.</span><span class="sxs-lookup"><span data-stu-id="f4d32-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="f4d32-106">Cette procédure pas à pas illustre le scénario dans lequel un service Windows Communication Foundation (WCF) expose une opération qui retourne un graphique d’entité.</span><span class="sxs-lookup"><span data-stu-id="f4d32-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="f4d32-107">Ensuite, une application cliente manipule ce graphique et soumet les modifications à une opération de service qui valide et enregistre les mises à jour dans une base de données à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f4d32-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="f4d32-108">Avant d’effectuer cette procédure pas à pas, vérifiez que vous lire le [Self-Tracking Entities](index.md) page.</span><span class="sxs-lookup"><span data-stu-id="f4d32-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="f4d32-109">Cette procédure pas à pas effectue les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="f4d32-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="f4d32-110">Crée une base de données pour accéder à.</span><span class="sxs-lookup"><span data-stu-id="f4d32-110">Creates a database to access.</span></span>
-   <span data-ttu-id="f4d32-111">Crée une bibliothèque de classes qui contient le modèle.</span><span class="sxs-lookup"><span data-stu-id="f4d32-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="f4d32-112">Remplace le modèle Générateur d’entité de suivi automatique.</span><span class="sxs-lookup"><span data-stu-id="f4d32-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="f4d32-113">Déplace les classes d’entité vers un projet distinct.</span><span class="sxs-lookup"><span data-stu-id="f4d32-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="f4d32-114">Crée un service WCF qui expose des opérations pour interroger et enregistrer des entités.</span><span class="sxs-lookup"><span data-stu-id="f4d32-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="f4d32-115">Crée des applications (Console et WPF) qui utilisent le service client.</span><span class="sxs-lookup"><span data-stu-id="f4d32-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="f4d32-116">Nous allons utiliser à la première base de données dans cette procédure pas à pas, mais les mêmes techniques s’appliquent également à Model First.</span><span class="sxs-lookup"><span data-stu-id="f4d32-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="f4d32-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="f4d32-117">Pre-Requisites</span></span>

<span data-ttu-id="f4d32-118">Pour effectuer cette procédure pas à pas, vous devez une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4d32-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="f4d32-119">Créer une base de données</span><span class="sxs-lookup"><span data-stu-id="f4d32-119">Create a Database</span></span>

<span data-ttu-id="f4d32-120">Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :</span><span class="sxs-lookup"><span data-stu-id="f4d32-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="f4d32-121">Si vous utilisez Visual Studio 2012 puis vous allez créer une base de données de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="f4d32-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="f4d32-122">Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="f4d32-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="f4d32-123">Procédons à générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="f4d32-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="f4d32-124">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4d32-124">Open Visual Studio</span></span>
-   <span data-ttu-id="f4d32-125">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="f4d32-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="f4d32-126">Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="f4d32-127">Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données</span><span class="sxs-lookup"><span data-stu-id="f4d32-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="f4d32-128">Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé</span><span class="sxs-lookup"><span data-stu-id="f4d32-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="f4d32-129">Entrez **STESample** en tant que le nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="f4d32-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="f4d32-130">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="f4d32-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="f4d32-131">La nouvelle base de données s’affiche désormais dans l’Explorateur de serveurs</span><span class="sxs-lookup"><span data-stu-id="f4d32-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="f4d32-132">Si vous utilisez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f4d32-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="f4d32-133">Avec le bouton droit sur la base de données dans l’Explorateur de serveurs, puis sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="f4d32-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="f4d32-134">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="f4d32-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="f4d32-135">Si vous utilisez Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="f4d32-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="f4d32-136">Sélectionnez **données -&gt; Transact SQL éditeur -&gt; nouvelle connexion à la requête...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="f4d32-137">Entrez **.\\ SQLEXPRESS** en tant que nom du serveur et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="f4d32-138">Sélectionnez le **STESample** de base de données dans la liste déroulante vers le bas en haut de l’éditeur de requête</span><span class="sxs-lookup"><span data-stu-id="f4d32-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="f4d32-139">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **exécuter SQL**</span><span class="sxs-lookup"><span data-stu-id="f4d32-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="f4d32-140">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="f4d32-140">Create the Model</span></span>

<span data-ttu-id="f4d32-141">Tout d’abord, nous avons besoin d’un projet de placer le modèle dans.</span><span class="sxs-lookup"><span data-stu-id="f4d32-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="f4d32-142">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4d32-143">Sélectionnez **Visual C\#**  dans le volet gauche, puis **bibliothèque de classes**</span><span class="sxs-lookup"><span data-stu-id="f4d32-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="f4d32-144">Entrez **STESample** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="f4d32-145">Nous allons maintenant créer un modèle simple dans le Concepteur EF pour accéder à notre base de données :</span><span class="sxs-lookup"><span data-stu-id="f4d32-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="f4d32-146">**Projet -&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="f4d32-147">Sélectionnez **données** dans le volet gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="f4d32-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="f4d32-148">Entrez **BloggingModel** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4d32-149">Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="f4d32-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="f4d32-150">Entrez les informations de connexion pour la base de données que vous avez créé dans la section précédente</span><span class="sxs-lookup"><span data-stu-id="f4d32-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="f4d32-151">Entrez **BloggingContext** comme nom de la chaîne de connexion et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="f4d32-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="f4d32-152">Cochez la case à côté **Tables** et cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="f4d32-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="f4d32-153">Échange à la génération de Code STE</span><span class="sxs-lookup"><span data-stu-id="f4d32-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="f4d32-154">Nous devons à présent de désactiver la génération de code par défaut et l’échange à Self-Tracking Entities.</span><span class="sxs-lookup"><span data-stu-id="f4d32-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="f4d32-155">Si vous utilisez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f4d32-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="f4d32-156">Développez **BloggingModel.edmx** dans **l’Explorateur de solutions** et supprimer le **BloggingModel.tt** et **BloggingModel.Context.tt** 
     *Cela désactive la génération de code par défaut*</span><span class="sxs-lookup"><span data-stu-id="f4d32-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="f4d32-157">Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une zone vide **ajouter un élément de génération de Code...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="f4d32-158">Sélectionnez **Online** dans le volet gauche et recherchez **STE Générateur**</span><span class="sxs-lookup"><span data-stu-id="f4d32-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="f4d32-159">Sélectionnez le **générateur STE pour C\#**  modèle, entrez **STETemplate** comme nom et cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="f4d32-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="f4d32-160">Le **STETemplate.tt** et **STETemplate.Context.tt** fichiers sont ajoutés imbriqué sous le fichier BloggingModel.edmx</span><span class="sxs-lookup"><span data-stu-id="f4d32-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="f4d32-161">Si vous utilisez Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="f4d32-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="f4d32-162">Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une zone vide **ajouter un élément de génération de Code...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="f4d32-163">Sélectionnez **Code** dans le volet gauche, puis **Générateur d’entité de suivi automatique ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="f4d32-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="f4d32-164">Entrez **STETemplate** comme nom et cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="f4d32-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="f4d32-165">Le **STETemplate.tt** et **STETemplate.Context.tt** fichiers sont ajoutés directement à votre projet</span><span class="sxs-lookup"><span data-stu-id="f4d32-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="f4d32-166">Déplacer les Types d’entités dans un projet distinct</span><span class="sxs-lookup"><span data-stu-id="f4d32-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="f4d32-167">Pour utiliser Self-Tracking Entities notre application cliente doit accéder aux classes d’entité générée à partir de notre modèle.</span><span class="sxs-lookup"><span data-stu-id="f4d32-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="f4d32-168">Étant donné que nous ne souhaitons pas exposer l’ensemble du modèle à l’application cliente, nous allons déplacer les classes d’entité dans un projet distinct.</span><span class="sxs-lookup"><span data-stu-id="f4d32-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="f4d32-169">La première étape consiste à arrêter la génération de classes d’entité dans le projet existant :</span><span class="sxs-lookup"><span data-stu-id="f4d32-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="f4d32-170">Avec le bouton droit sur **STETemplate.tt** dans **l’Explorateur de solutions** et sélectionnez **propriétés**</span><span class="sxs-lookup"><span data-stu-id="f4d32-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="f4d32-171">Dans le **propriétés** fenêtre clair **TextTemplatingFileGenerator** à partir de la **CustomTool** propriété</span><span class="sxs-lookup"><span data-stu-id="f4d32-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="f4d32-172">Développez **STETemplate.tt** dans **l’Explorateur de solutions** et supprimez tous les fichiers imbriqués dans celui-ci</span><span class="sxs-lookup"><span data-stu-id="f4d32-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="f4d32-173">Ensuite, nous allons ajouter un nouveau projet et générer les classes d’entité qu’il contient</span><span class="sxs-lookup"><span data-stu-id="f4d32-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="f4d32-174">**Fichier -&gt; Add -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="f4d32-175">Sélectionnez **Visual C\#**  dans le volet gauche, puis **bibliothèque de classes**</span><span class="sxs-lookup"><span data-stu-id="f4d32-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="f4d32-176">Entrez **STESample.Entities** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4d32-177">**Projet -&gt; ajouter un élément existant...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="f4d32-178">Accédez à la **STESample** dossier du projet</span><span class="sxs-lookup"><span data-stu-id="f4d32-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="f4d32-179">Sélectionnez cette option pour afficher les **tous les fichiers (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="f4d32-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="f4d32-180">Sélectionnez le **STETemplate.tt** fichier</span><span class="sxs-lookup"><span data-stu-id="f4d32-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="f4d32-181">Cliquez sur la flèche déroulante à côté du **ajouter** bouton et sélectionnez **ajouter en tant que lien**</span><span class="sxs-lookup"><span data-stu-id="f4d32-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Ajouter le modèle lié](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="f4d32-183">Nous allons également s’assurer que les classes d’entité sont générés dans le même espace de noms comme contexte.</span><span class="sxs-lookup"><span data-stu-id="f4d32-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="f4d32-184">Simplement, cela réduit le nombre d’à l’aide des instructions, que nous devons ajouter tout au long de notre application.</span><span class="sxs-lookup"><span data-stu-id="f4d32-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="f4d32-185">Avec le bouton droit sur le texte lié **STETemplate.tt** dans **l’Explorateur de solutions** et sélectionnez **propriétés**</span><span class="sxs-lookup"><span data-stu-id="f4d32-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="f4d32-186">Dans le **propriétés** ensemble de la fenêtre **Namespace d’outil personnalisé** à **STESample**</span><span class="sxs-lookup"><span data-stu-id="f4d32-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="f4d32-187">Le code généré par le modèle STE sera besoin d’une référence à **System.Runtime.Serialization** afin de compiler.</span><span class="sxs-lookup"><span data-stu-id="f4d32-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="f4d32-188">Cette bibliothèque est nécessaire pour WCF **DataContract** et **DataMember** attributs qui sont utilisés sur les types d’entité sérialisables.</span><span class="sxs-lookup"><span data-stu-id="f4d32-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="f4d32-189">Cliquez avec le bouton droit sur le **STESample.Entities** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="f4d32-190">Dans Visual Studio 2012 – la case à cocher à côté **System.Runtime.Serialization** et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="f4d32-191">Dans Visual Studio 2010 - sélectionnez **System.Runtime.Serialization** et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="f4d32-192">Enfin, le projet avec notre contexte qu’il contient est besoin d’une référence aux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="f4d32-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="f4d32-193">Cliquez avec le bouton droit sur le **STESample** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="f4d32-194">Dans Visual Studio 2012 - sélectionnez **Solution** dans le volet gauche, cochez la case à côté **STESample.Entities** et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="f4d32-195">Dans Visual Studio 2010 - sélectionner le **projets** onglet, sélectionnez **STESample.Entities** et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="f4d32-196">Une autre option pour déplacer les types d’entité vers un projet distinct consiste à déplacer le fichier de modèle, plutôt que de les lier à partir de son emplacement par défaut.</span><span class="sxs-lookup"><span data-stu-id="f4d32-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="f4d32-197">Si vous procédez ainsi, vous devez mettre à jour le **inputFile** variable dans le modèle pour fournir le chemin d’accès relatif au fichier edmx (dans cet exemple serait **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="f4d32-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="f4d32-198">Créer un Service WCF</span><span class="sxs-lookup"><span data-stu-id="f4d32-198">Create a WCF Service</span></span>

<span data-ttu-id="f4d32-199">Il est à présent temps ajouter un Service WCF pour exposer nos données, nous allons commencer par créer le projet.</span><span class="sxs-lookup"><span data-stu-id="f4d32-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="f4d32-200">**Fichier -&gt; Add -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="f4d32-201">Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application de Service WCF**</span><span class="sxs-lookup"><span data-stu-id="f4d32-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="f4d32-202">Entrez **STESample.Service** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4d32-203">Ajoutez une référence à la **System.Data.Entity** assembly</span><span class="sxs-lookup"><span data-stu-id="f4d32-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="f4d32-204">Ajoutez une référence à la **STESample** et **STESample.Entities** projets</span><span class="sxs-lookup"><span data-stu-id="f4d32-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="f4d32-205">Nous avons besoin de copier la chaîne de connexion EF à ce projet afin qu’il est trouvé lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f4d32-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="f4d32-206">Ouvrez le **App.Config** de fichiers pour le \*\* STESample \*\* projet, puis copiez le **connectionStrings** élément</span><span class="sxs-lookup"><span data-stu-id="f4d32-206">Open the **App.Config** file for the \*\*STESample \*\*project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="f4d32-207">Coller le **connectionStrings** en tant qu’un élément enfant de le **configuration** élément de la **Web.Config** de fichiers dans le **STESample.Service** projet</span><span class="sxs-lookup"><span data-stu-id="f4d32-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="f4d32-208">Il est maintenant temps d’implémenter le service réel.</span><span class="sxs-lookup"><span data-stu-id="f4d32-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="f4d32-209">Ouvrez **IService1.cs** et remplacez le contenu par le code suivant</span><span class="sxs-lookup"><span data-stu-id="f4d32-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="f4d32-210">Ouvrez **Service1.svc** et remplacez le contenu par le code suivant</span><span class="sxs-lookup"><span data-stu-id="f4d32-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="f4d32-211">Consommer le Service à partir d’une Application Console</span><span class="sxs-lookup"><span data-stu-id="f4d32-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="f4d32-212">Nous allons créer une application console qui utilise notre service.</span><span class="sxs-lookup"><span data-stu-id="f4d32-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="f4d32-213">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4d32-214">Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application Console**</span><span class="sxs-lookup"><span data-stu-id="f4d32-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="f4d32-215">Entrez **STESample.ConsoleTest** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4d32-216">Ajoutez une référence à la **STESample.Entities** projet</span><span class="sxs-lookup"><span data-stu-id="f4d32-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="f4d32-217">Nous avons besoin d’une référence de service à notre service WCF</span><span class="sxs-lookup"><span data-stu-id="f4d32-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="f4d32-218">Avec le bouton droit le **STESample.ConsoleTest** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence de Service...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="f4d32-219">Cliquez sur **découvrir**</span><span class="sxs-lookup"><span data-stu-id="f4d32-219">Click **Discover**</span></span>
-   <span data-ttu-id="f4d32-220">Entrez **BloggingService** comme l’espace de noms et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="f4d32-221">Nous pouvons maintenant écrire du code pour consommer le service.</span><span class="sxs-lookup"><span data-stu-id="f4d32-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="f4d32-222">Ouvrez **Program.cs** et remplacez le contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="f4d32-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="f4d32-223">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="f4d32-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="f4d32-224">Avec le bouton droit le **STESample.ConsoleTest** projet **l’Explorateur de solutions** et sélectionnez **déboguer -&gt; démarrer une nouvelle instance**</span><span class="sxs-lookup"><span data-stu-id="f4d32-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="f4d32-225">Vous verrez la sortie suivante lorsque l’application s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f4d32-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="f4d32-226">Consommer le Service à partir d’une Application WPF</span><span class="sxs-lookup"><span data-stu-id="f4d32-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="f4d32-227">Nous allons créer une application WPF qui utilise notre service.</span><span class="sxs-lookup"><span data-stu-id="f4d32-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="f4d32-228">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="f4d32-229">Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application WPF**</span><span class="sxs-lookup"><span data-stu-id="f4d32-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="f4d32-230">Entrez **STESample.WPFTest** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="f4d32-231">Ajoutez une référence à la **STESample.Entities** projet</span><span class="sxs-lookup"><span data-stu-id="f4d32-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="f4d32-232">Nous avons besoin d’une référence de service à notre service WCF</span><span class="sxs-lookup"><span data-stu-id="f4d32-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="f4d32-233">Avec le bouton droit le **STESample.WPFTest** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence de Service...**</span><span class="sxs-lookup"><span data-stu-id="f4d32-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="f4d32-234">Cliquez sur **découvrir**</span><span class="sxs-lookup"><span data-stu-id="f4d32-234">Click **Discover**</span></span>
-   <span data-ttu-id="f4d32-235">Entrez **BloggingService** comme l’espace de noms et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f4d32-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="f4d32-236">Nous pouvons maintenant écrire du code pour consommer le service.</span><span class="sxs-lookup"><span data-stu-id="f4d32-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="f4d32-237">Ouvrez **MainWindow.xaml** et remplacez le contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="f4d32-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="f4d32-238">Ouvrez le code-behind de MainWindow (**MainWindow.xaml.cs**) et remplacez le contenu par le code suivant</span><span class="sxs-lookup"><span data-stu-id="f4d32-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="f4d32-239">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="f4d32-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="f4d32-240">Avec le bouton droit le **STESample.WPFTest** projet **l’Explorateur de solutions** et sélectionnez **déboguer -&gt; démarrer une nouvelle instance**</span><span class="sxs-lookup"><span data-stu-id="f4d32-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="f4d32-241">Vous pouvez manipuler les données à l’aide de l’écran et l’enregistrer via le service en utilisant le **enregistrer** bouton</span><span class="sxs-lookup"><span data-stu-id="f4d32-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Fenêtre principale de WPF](~/ef6/media/wpf.png)
