---
title: Bien démarrer avec ASP.NET Core - Base de données existante - EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 84e2e4bc1bdc774fa059fa893e0f8ac128931feb
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447181"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="ef7c8-102">Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="ef7c8-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="ef7c8-103">Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="ef7c8-104">Vous rétroconcevez une base de données existante pour créer un modèle Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="ef7c8-105">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="ef7c8-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef7c8-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ef7c8-106">Prerequisites</span></span>

<span data-ttu-id="ef7c8-107">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="ef7c8-107">Install the following software:</span></span>

* <span data-ttu-id="ef7c8-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef7c8-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="ef7c8-109">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="ef7c8-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="ef7c8-110">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="ef7c8-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="ef7c8-111">[SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="ef7c8-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="ef7c8-112">Créer une base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="ef7c8-112">Create Blogging database</span></span>

<span data-ttu-id="ef7c8-113">Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="ef7c8-114">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="ef7c8-115">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef7c8-115">Open Visual Studio</span></span>
* <span data-ttu-id="ef7c8-116">**Outils -> Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="ef7c8-117">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="ef7c8-118">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="ef7c8-119">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="ef7c8-120">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="ef7c8-121">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="ef7c8-122">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="ef7c8-123">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="ef7c8-124">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="ef7c8-124">Create a new project</span></span>

* <span data-ttu-id="ef7c8-125">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="ef7c8-126">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-126">**File > New > Project...**</span></span>
* <span data-ttu-id="ef7c8-127">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="ef7c8-128">Sélectionnez le modèle de projet **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="ef7c8-129">Entrez le nom **EFGetStarted.AspNetCore.ExistingDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="ef7c8-130">Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="ef7c8-131">Vérifiez que la liste déroulante du framework cible a la valeur **.NET Core**, et que la liste déroulante de version a la valeur **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="ef7c8-132">Sélectionnez le modèle **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="ef7c8-133">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="ef7c8-134">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="ef7c8-135">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ef7c8-135">Install Entity Framework Core</span></span>

<span data-ttu-id="ef7c8-136">Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="ef7c8-137">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ef7c8-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="ef7c8-138">Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="ef7c8-139">Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="ef7c8-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="ef7c8-140">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="ef7c8-140">Reverse engineer your model</span></span>

<span data-ttu-id="ef7c8-141">Créons à présent le modèle EF à partir de votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="ef7c8-142">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="ef7c8-143">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante :</span><span class="sxs-lookup"><span data-stu-id="ef7c8-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="ef7c8-144">Si vous recevez l’erreur `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="ef7c8-145">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="ef7c8-146">Par exemple, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="ef7c8-147">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog.cs` & `Post.cs`) et un contexte dérivé (`BloggingContext.cs`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="ef7c8-148">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="ef7c8-149">Voici les classes d’entité `Blog` et `Post` :</span><span class="sxs-lookup"><span data-stu-id="ef7c8-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="ef7c8-150">Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="ef7c8-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="ef7c8-151">Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="ef7c8-152">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="ef7c8-152">Register your context with dependency injection</span></span>

<span data-ttu-id="ef7c8-153">Le concept d’injection de dépendances est essentiel dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="ef7c8-154">Des services (tels que `BloggingContext`) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ef7c8-155">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="ef7c8-156">Pour plus d’informations sur l’injection de dépendances, consultez l’article [Injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) sur le site ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="ef7c8-157">Inscrire et configurer le contexte dans Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ef7c8-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="ef7c8-158">Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="ef7c8-159">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="ef7c8-160">Ajoutez les instructions `using` suivantes au début du fichier.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="ef7c8-161">Vous pouvez à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="ef7c8-162">Localisez la méthode `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="ef7c8-163">Ajoutez le code en surbrillance suivant pour inscrire le contexte en tant que service.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="ef7c8-164">Dans une application réelle, vous placez généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="ef7c8-165">Par souci de simplicité, ce tutoriel la définit dans le code.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="ef7c8-166">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="ef7c8-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="ef7c8-167">Créer un contrôleur et des vues</span><span class="sxs-lookup"><span data-stu-id="ef7c8-167">Create a controller and views</span></span>

* <span data-ttu-id="ef7c8-168">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="ef7c8-169">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="ef7c8-170">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="ef7c8-171">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="ef7c8-172">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="ef7c8-172">Run the application</span></span>

<span data-ttu-id="ef7c8-173">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="ef7c8-174">**Déboguer -> Exécuter sans débogage**</span><span class="sxs-lookup"><span data-stu-id="ef7c8-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="ef7c8-175">L’application est générée et s’ouvre dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="ef7c8-176">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="ef7c8-177">Cliquez sur **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-177">Click **Create New**</span></span>
* <span data-ttu-id="ef7c8-178">Entrez l’**URL** du nouveau blog et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ef7c8-178">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Créer une page](_static/create.png)

  ![Page d’index](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="ef7c8-181">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ef7c8-181">Next steps</span></span>

<span data-ttu-id="ef7c8-182">Pour plus d’informations sur la façon de structurer un contexte et des classes d’entités, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="ef7c8-182">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="ef7c8-183">Référence des outils Entity Framework Core - .NET CLI</span><span class="sxs-lookup"><span data-stu-id="ef7c8-183">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="ef7c8-184">Référence des outils Entity Framework Core - Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="ef7c8-184">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)