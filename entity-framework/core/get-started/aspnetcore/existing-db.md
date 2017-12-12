---
title: "Bien démarrer avec ASP.NET Core - Base de données existante - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="01a6b-102">Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="01a6b-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="01a6b-103">Le [Kit SDK .NET Core](https://www.microsoft.com/net/download/core) ne prend plus en charge `project.json` ni Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="01a6b-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="01a6b-104">Tous les utilisateurs réalisant des tâches de développement .NET Core sont invités à [migrer de project.json vers csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) et [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="01a6b-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="01a6b-105">Dans cette procédure pas à pas, vous allez générer une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="01a6b-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="01a6b-106">Vous allez utiliser l’ingénierie à rebours pour créer un modèle Entity Framework à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="01a6b-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="01a6b-107">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="01a6b-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01a6b-108">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="01a6b-108">Prerequisites</span></span>

<span data-ttu-id="01a6b-109">Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :</span><span class="sxs-lookup"><span data-stu-id="01a6b-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="01a6b-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="01a6b-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="01a6b-111">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="01a6b-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="01a6b-112">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="01a6b-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="01a6b-113">[Kit SDK .NET Core 2.0](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="01a6b-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="01a6b-114">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="01a6b-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="01a6b-115">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="01a6b-115">Blogging database</span></span>

<span data-ttu-id="01a6b-116">Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.</span><span class="sxs-lookup"><span data-stu-id="01a6b-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="01a6b-117">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre didacticiel, vous pouvez ignorer ces étapes.</span><span class="sxs-lookup"><span data-stu-id="01a6b-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="01a6b-118">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01a6b-118">Open Visual Studio</span></span>
* <span data-ttu-id="01a6b-119">**Outils -> Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="01a6b-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="01a6b-120">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="01a6b-121">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="01a6b-122">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="01a6b-123">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="01a6b-124">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="01a6b-125">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="01a6b-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="01a6b-126">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="01a6b-127">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="01a6b-127">Create a new project</span></span>

* <span data-ttu-id="01a6b-128">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="01a6b-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="01a6b-129">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="01a6b-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="01a6b-130">Dans le menu de gauche, sélectionnez **Installé > Modèles > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="01a6b-131">Sélectionnez le modèle de projet **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="01a6b-132">Entrez le nom **EFGetStarted.AspNetCore.ExistingDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="01a6b-133">Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="01a6b-134">Dans la liste de **modèles ASP.NET Core 2.0**, sélectionnez le modèle de projet **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="01a6b-135">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="01a6b-136">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="01a6b-137">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="01a6b-137">Install Entity Framework</span></span>

<span data-ttu-id="01a6b-138">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="01a6b-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="01a6b-139">Cette procédure pas à pas utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01a6b-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="01a6b-140">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="01a6b-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="01a6b-141">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="01a6b-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="01a6b-142">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="01a6b-143">Nous utiliserons des outils Entity Framework pour créer un modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="01a6b-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="01a6b-144">Nous installerons donc également le package d’outils :</span><span class="sxs-lookup"><span data-stu-id="01a6b-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="01a6b-145">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="01a6b-146">Nous utiliserons des outils de génération de modèles automatique ASP.NET Core pour créer par la suite des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="01a6b-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="01a6b-147">Nous installerons donc également ce package de conception :</span><span class="sxs-lookup"><span data-stu-id="01a6b-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="01a6b-148">Exécutez `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="01a6b-149">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="01a6b-149">Reverse engineer your model</span></span>

<span data-ttu-id="01a6b-150">Créons à présent le modèle EF à partir de votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="01a6b-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="01a6b-151">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="01a6b-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="01a6b-152">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante :</span><span class="sxs-lookup"><span data-stu-id="01a6b-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="01a6b-153">Si vous recevez l’erreur `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01a6b-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="01a6b-154">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="01a6b-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="01a6b-155">Par ex.</span><span class="sxs-lookup"><span data-stu-id="01a6b-155">E.g.</span></span> <span data-ttu-id="01a6b-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="01a6b-157">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog.cs` & `Post.cs`) et un contexte dérivé (`BloggingContext.cs`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="01a6b-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="01a6b-158">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="01a6b-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="01a6b-159">Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="01a6b-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="01a6b-160">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="01a6b-160">Register your context with dependency injection</span></span>

<span data-ttu-id="01a6b-161">Le concept d’injection de dépendances est essentiel dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01a6b-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="01a6b-162">Des services (tels que `BloggingContext`) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="01a6b-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="01a6b-163">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="01a6b-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="01a6b-164">Pour plus d’informations sur l’injection de dépendances, consultez l’article [Injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) sur le site ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01a6b-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="01a6b-165">Supprimer la configuration contextuelle inline</span><span class="sxs-lookup"><span data-stu-id="01a6b-165">Remove inline context configuration</span></span>

<span data-ttu-id="01a6b-166">Dans ASP.NET Core, la configuration s’effectue généralement dans **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="01a6b-167">Pour respecter ce modèle, nous allons transférer la configuration du fournisseur de base de données dans **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="01a6b-168">Ouvrir `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="01a6b-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="01a6b-169">Supprimez la méthode `OnConfiguring(...)`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="01a6b-170">Ajoutez le constructeur suivant pour pouvoir transférer la configuration dans le contexte par injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="01a6b-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="01a6b-171">Inscrire et configurer le contexte dans Startup.cs</span><span class="sxs-lookup"><span data-stu-id="01a6b-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="01a6b-172">Afin de permettre à nos contrôleurs MVC d’utiliser `BloggingContext`, nous allons inscrire ce dernier en tant que service.</span><span class="sxs-lookup"><span data-stu-id="01a6b-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="01a6b-173">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="01a6b-174">Ajoutez les instructions `using` suivantes au début du fichier.</span><span class="sxs-lookup"><span data-stu-id="01a6b-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="01a6b-175">Nous pouvons à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.</span><span class="sxs-lookup"><span data-stu-id="01a6b-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="01a6b-176">Localisez la méthode `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="01a6b-177">Ajoutez le code suivant pour inscrire le contexte en tant que service.</span><span class="sxs-lookup"><span data-stu-id="01a6b-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="01a6b-178">Dans une application réelle, vous placeriez généralement la chaîne de connexion dans un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="01a6b-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="01a6b-179">Par souci de simplicité, nous allons la définir dans le code.</span><span class="sxs-lookup"><span data-stu-id="01a6b-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="01a6b-180">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="01a6b-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="01a6b-181">Créer un contrôleur</span><span class="sxs-lookup"><span data-stu-id="01a6b-181">Create a controller</span></span>

<span data-ttu-id="01a6b-182">Nous allons à présent activer la génération de modèles automatique dans notre projet.</span><span class="sxs-lookup"><span data-stu-id="01a6b-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="01a6b-183">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="01a6b-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="01a6b-184">Sélectionnez **Dépendances complètes** et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="01a6b-185">Vous pouvez ignorer les instructions du fichier `ScaffoldingReadMe.txt` qui s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="01a6b-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="01a6b-186">Une fois activée la génération de modèles automatique, nous pouvons générer automatiquement un modèle de contrôleur pour l’entité `Blog`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="01a6b-187">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="01a6b-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="01a6b-188">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="01a6b-189">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="01a6b-190">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="01a6b-191">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="01a6b-191">Run the application</span></span>

<span data-ttu-id="01a6b-192">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="01a6b-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="01a6b-193">**Déboguer -> Exécuter sans débogage**</span><span class="sxs-lookup"><span data-stu-id="01a6b-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="01a6b-194">L’application est générée et s’ouvre dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="01a6b-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="01a6b-195">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="01a6b-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="01a6b-196">Cliquez sur **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-196">Click **Create New**</span></span>
* <span data-ttu-id="01a6b-197">Entrez l’**URL** du nouveau blog et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="01a6b-197">Enter a **Url** for the new blog and click **Create**</span></span>

![image](_static/create.png)

![image](_static/index-existing-db.png)
