---
title: Bien démarrer avec ASP.NET Core - Base de données existante - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949151"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="08c7e-102">Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="08c7e-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="08c7e-103">Dans cette procédure pas à pas, vous allez générer une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="08c7e-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="08c7e-104">Vous allez utiliser l’ingénierie à rebours pour créer un modèle Entity Framework à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="08c7e-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="08c7e-105">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="08c7e-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08c7e-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="08c7e-106">Prerequisites</span></span>

<span data-ttu-id="08c7e-107">Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :</span><span class="sxs-lookup"><span data-stu-id="08c7e-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="08c7e-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="08c7e-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="08c7e-109">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="08c7e-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="08c7e-110">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="08c7e-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="08c7e-111">[Kit SDK .NET Core 2.0](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="08c7e-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="08c7e-112">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="08c7e-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="08c7e-113">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="08c7e-113">Blogging database</span></span>

<span data-ttu-id="08c7e-114">Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.</span><span class="sxs-lookup"><span data-stu-id="08c7e-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="08c7e-115">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre didacticiel, vous pouvez ignorer ces étapes.</span><span class="sxs-lookup"><span data-stu-id="08c7e-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="08c7e-116">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08c7e-116">Open Visual Studio</span></span>
* <span data-ttu-id="08c7e-117">**Outils -> Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="08c7e-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="08c7e-118">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="08c7e-119">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="08c7e-120">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="08c7e-121">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="08c7e-122">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="08c7e-123">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="08c7e-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="08c7e-124">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="08c7e-125">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="08c7e-125">Create a new project</span></span>

* <span data-ttu-id="08c7e-126">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="08c7e-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="08c7e-127">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="08c7e-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="08c7e-128">Dans le menu de gauche, sélectionnez **Installé > Modèles > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="08c7e-129">Sélectionnez le modèle de projet **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="08c7e-130">Entrez le nom **EFGetStarted.AspNetCore.ExistingDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="08c7e-131">Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="08c7e-132">Dans la liste de **modèles ASP.NET Core 2.0**, sélectionnez le modèle de projet **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="08c7e-133">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="08c7e-134">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="08c7e-135">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="08c7e-135">Install Entity Framework</span></span>

<span data-ttu-id="08c7e-136">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="08c7e-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="08c7e-137">Cette procédure pas à pas utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="08c7e-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="08c7e-138">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="08c7e-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="08c7e-139">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="08c7e-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="08c7e-140">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="08c7e-141">Nous utiliserons des outils Entity Framework pour créer un modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c7e-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="08c7e-142">Nous installerons donc également le package d’outils :</span><span class="sxs-lookup"><span data-stu-id="08c7e-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="08c7e-143">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="08c7e-144">Nous utiliserons des outils de génération de modèles automatique ASP.NET Core pour créer par la suite des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="08c7e-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="08c7e-145">Nous installerons donc également ce package de conception :</span><span class="sxs-lookup"><span data-stu-id="08c7e-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="08c7e-146">Exécutez `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="08c7e-147">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="08c7e-147">Reverse engineer your model</span></span>

<span data-ttu-id="08c7e-148">Créons à présent le modèle EF à partir de votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="08c7e-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="08c7e-149">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="08c7e-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="08c7e-150">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante :</span><span class="sxs-lookup"><span data-stu-id="08c7e-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="08c7e-151">Si vous recevez l’erreur `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08c7e-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="08c7e-152">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="08c7e-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="08c7e-153">Par exemple, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="08c7e-154">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog.cs` & `Post.cs`) et un contexte dérivé (`BloggingContext.cs`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="08c7e-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="08c7e-155">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="08c7e-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="08c7e-156">Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="08c7e-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="08c7e-157">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="08c7e-157">Register your context with dependency injection</span></span>

<span data-ttu-id="08c7e-158">Le concept d’injection de dépendances est essentiel dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08c7e-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="08c7e-159">Des services (tels que `BloggingContext`) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="08c7e-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="08c7e-160">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="08c7e-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="08c7e-161">Pour plus d’informations sur l’injection de dépendances, consultez l’article [Injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) sur le site ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08c7e-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="08c7e-162">Supprimer la configuration contextuelle inline</span><span class="sxs-lookup"><span data-stu-id="08c7e-162">Remove inline context configuration</span></span>

<span data-ttu-id="08c7e-163">Dans ASP.NET Core, la configuration s’effectue généralement dans **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="08c7e-164">Pour respecter ce modèle, nous allons transférer la configuration du fournisseur de base de données dans **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="08c7e-165">Ouvrez `Models\BloggingContext.cs`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="08c7e-166">Supprimez la méthode `OnConfiguring(...)`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="08c7e-167">Ajoutez le constructeur suivant pour pouvoir transférer la configuration dans le contexte par injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="08c7e-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="08c7e-168">Inscrire et configurer le contexte dans Startup.cs</span><span class="sxs-lookup"><span data-stu-id="08c7e-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="08c7e-169">Afin de permettre à nos contrôleurs MVC d’utiliser `BloggingContext`, nous allons inscrire ce dernier en tant que service.</span><span class="sxs-lookup"><span data-stu-id="08c7e-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="08c7e-170">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="08c7e-171">Ajoutez les instructions `using` suivantes au début du fichier.</span><span class="sxs-lookup"><span data-stu-id="08c7e-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="08c7e-172">Nous pouvons à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.</span><span class="sxs-lookup"><span data-stu-id="08c7e-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="08c7e-173">Localisez la méthode `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="08c7e-174">Ajoutez le code suivant pour inscrire le contexte en tant que service.</span><span class="sxs-lookup"><span data-stu-id="08c7e-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="08c7e-175">Dans une application réelle, vous placeriez généralement la chaîne de connexion dans un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="08c7e-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="08c7e-176">Par souci de simplicité, nous allons la définir dans le code.</span><span class="sxs-lookup"><span data-stu-id="08c7e-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="08c7e-177">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="08c7e-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="08c7e-178">Créer un contrôleur</span><span class="sxs-lookup"><span data-stu-id="08c7e-178">Create a controller</span></span>

<span data-ttu-id="08c7e-179">Nous allons à présent activer la génération de modèles automatique dans notre projet.</span><span class="sxs-lookup"><span data-stu-id="08c7e-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="08c7e-180">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="08c7e-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="08c7e-181">Sélectionnez **Dépendances complètes** et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="08c7e-182">Vous pouvez ignorer les instructions du fichier `ScaffoldingReadMe.txt` qui s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="08c7e-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="08c7e-183">Une fois activée la génération de modèles automatique, nous pouvons générer automatiquement un modèle de contrôleur pour l’entité `Blog`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="08c7e-184">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="08c7e-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="08c7e-185">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="08c7e-186">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="08c7e-187">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="08c7e-188">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="08c7e-188">Run the application</span></span>

<span data-ttu-id="08c7e-189">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="08c7e-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="08c7e-190">**Déboguer -> Exécuter sans débogage**</span><span class="sxs-lookup"><span data-stu-id="08c7e-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="08c7e-191">L’application est générée et s’ouvre dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="08c7e-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="08c7e-192">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="08c7e-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="08c7e-193">Cliquez sur **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-193">Click **Create New**</span></span>
* <span data-ttu-id="08c7e-194">Entrez l’**URL** du nouveau blog et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="08c7e-194">Enter a **Url** for the new blog and click **Create**</span></span>

![image](_static/create.png)

![image](_static/index-existing-db.png)
