---
title: Bien démarrer avec ASP.NET Core - Base de données existante - EF Core
author: rowanmiller
description: Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497513"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="0c81c-103">Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="0c81c-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="0c81c-104">Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0c81c-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="0c81c-105">Vous rétroconcevez une base de données existante pour créer un modèle Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0c81c-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="0c81c-106">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="0c81c-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c81c-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="0c81c-107">Prerequisites</span></span>

<span data-ttu-id="0c81c-108">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="0c81c-108">Install the following software:</span></span>

* <span data-ttu-id="0c81c-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="0c81c-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="0c81c-110">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="0c81c-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="0c81c-111">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="0c81c-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="0c81c-112">[SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="0c81c-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="0c81c-113">Créer une base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="0c81c-113">Create Blogging database</span></span>

<span data-ttu-id="0c81c-114">Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.</span><span class="sxs-lookup"><span data-stu-id="0c81c-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="0c81c-115">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.</span><span class="sxs-lookup"><span data-stu-id="0c81c-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="0c81c-116">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c81c-116">Open Visual Studio</span></span>
* <span data-ttu-id="0c81c-117">**Outils -> Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="0c81c-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="0c81c-118">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="0c81c-119">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="0c81c-120">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="0c81c-121">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="0c81c-122">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="0c81c-123">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="0c81c-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="0c81c-124">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="0c81c-125">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="0c81c-125">Create a new project</span></span>

* <span data-ttu-id="0c81c-126">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0c81c-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="0c81c-127">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="0c81c-127">**File > New > Project...**</span></span>
* <span data-ttu-id="0c81c-128">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="0c81c-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="0c81c-129">Sélectionnez le modèle de projet **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="0c81c-130">Entrez **EFGetStarted.AspNetCore.ExistingDb** comme nom (il doit correspondre exactement à l’espace de noms utilisé ultérieurement dans le code) et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="0c81c-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="0c81c-131">Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="0c81c-132">Vérifiez que la liste déroulante du framework cible a la valeur **.NET Core**, et que la liste déroulante de version a la valeur **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="0c81c-133">Sélectionnez le modèle **Application web (Model-View-Controller)** .</span><span class="sxs-lookup"><span data-stu-id="0c81c-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="0c81c-134">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="0c81c-135">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="0c81c-136">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0c81c-136">Install Entity Framework Core</span></span>

<span data-ttu-id="0c81c-137">Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler.</span><span class="sxs-lookup"><span data-stu-id="0c81c-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="0c81c-138">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0c81c-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="0c81c-139">Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0c81c-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="0c81c-140">Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="0c81c-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="0c81c-141">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="0c81c-141">Reverse engineer your model</span></span>

<span data-ttu-id="0c81c-142">Créons à présent le modèle EF à partir de votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="0c81c-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="0c81c-143">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="0c81c-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="0c81c-144">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante :</span><span class="sxs-lookup"><span data-stu-id="0c81c-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="0c81c-145">Si vous recevez l’erreur `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c81c-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="0c81c-146">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="0c81c-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="0c81c-147">Par exemple, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="0c81c-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="0c81c-148">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog.cs` & `Post.cs`) et un contexte dérivé (`BloggingContext.cs`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="0c81c-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="0c81c-149">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="0c81c-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="0c81c-150">Voici les classes d’entité `Blog` et `Post` :</span><span class="sxs-lookup"><span data-stu-id="0c81c-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="0c81c-151">Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="0c81c-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="0c81c-152">Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="0c81c-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="0c81c-153">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="0c81c-153">Register your context with dependency injection</span></span>

<span data-ttu-id="0c81c-154">Le concept d’injection de dépendances est essentiel dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c81c-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="0c81c-155">Des services (tels que `BloggingContext`) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="0c81c-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="0c81c-156">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="0c81c-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="0c81c-157">Pour plus d’informations sur l’injection de dépendances, consultez l’article [Injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) sur le site ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0c81c-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="0c81c-158">Inscrire et configurer le contexte dans Startup.cs</span><span class="sxs-lookup"><span data-stu-id="0c81c-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="0c81c-159">Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.</span><span class="sxs-lookup"><span data-stu-id="0c81c-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="0c81c-160">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="0c81c-161">Ajoutez les instructions `using` suivantes au début du fichier.</span><span class="sxs-lookup"><span data-stu-id="0c81c-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="0c81c-162">Vous pouvez à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.</span><span class="sxs-lookup"><span data-stu-id="0c81c-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="0c81c-163">Localisez la méthode `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="0c81c-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="0c81c-164">Ajoutez le code en surbrillance suivant pour inscrire le contexte en tant que service.</span><span class="sxs-lookup"><span data-stu-id="0c81c-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="0c81c-165">Dans une application réelle, vous placez généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0c81c-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="0c81c-166">Par souci de simplicité, ce tutoriel la définit dans le code.</span><span class="sxs-lookup"><span data-stu-id="0c81c-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="0c81c-167">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="0c81c-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="0c81c-168">Créer un contrôleur et des vues</span><span class="sxs-lookup"><span data-stu-id="0c81c-168">Create a controller and views</span></span>

* <span data-ttu-id="0c81c-169">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**</span><span class="sxs-lookup"><span data-stu-id="0c81c-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="0c81c-170">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="0c81c-171">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="0c81c-172">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="0c81c-173">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="0c81c-173">Run the application</span></span>

<span data-ttu-id="0c81c-174">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="0c81c-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="0c81c-175">**Déboguer -> Exécuter sans débogage**</span><span class="sxs-lookup"><span data-stu-id="0c81c-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="0c81c-176">L’application est générée et s’ouvre dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="0c81c-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="0c81c-177">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="0c81c-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="0c81c-178">Cliquez sur **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-178">Click **Create New**</span></span>
* <span data-ttu-id="0c81c-179">Entrez l’**URL** du nouveau blog et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="0c81c-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Créer une page](_static/create.png)

  ![Page d’index](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="0c81c-182">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="0c81c-182">Next steps</span></span>

<span data-ttu-id="0c81c-183">Pour plus d’informations sur la façon de structurer un contexte et des classes d’entité, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="0c81c-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="0c81c-184">Reconstitution de la logique des produits</span><span class="sxs-lookup"><span data-stu-id="0c81c-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="0c81c-185">Référence des outils Entity Framework Core - .NET CLI</span><span class="sxs-lookup"><span data-stu-id="0c81c-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="0c81c-186">Référence des outils Entity Framework Core - Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="0c81c-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
