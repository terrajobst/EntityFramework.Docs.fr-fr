---
title: Bien démarrer avec .NET Framework - Base de données existante - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="95d4b-102">Bien démarrer avec EF Core sur .NET Framework avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="95d4b-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="95d4b-103">Dans cette procédure pas à pas, vous allez générer une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="95d4b-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="95d4b-104">Vous allez utiliser l’ingénierie à rebours pour créer un modèle Entity Framework à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="95d4b-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="95d4b-105">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="95d4b-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95d4b-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="95d4b-106">Prerequisites</span></span>

<span data-ttu-id="95d4b-107">Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :</span><span class="sxs-lookup"><span data-stu-id="95d4b-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="95d4b-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="95d4b-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="95d4b-109">Dernière version du Gestionnaire de package NuGet</span><span class="sxs-lookup"><span data-stu-id="95d4b-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="95d4b-110">Dernière version de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="95d4b-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="95d4b-111">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="95d4b-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="95d4b-112">Base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="95d4b-112">Blogging database</span></span>

<span data-ttu-id="95d4b-113">Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.</span><span class="sxs-lookup"><span data-stu-id="95d4b-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="95d4b-114">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre didacticiel, vous pouvez ignorer ces étapes.</span><span class="sxs-lookup"><span data-stu-id="95d4b-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="95d4b-115">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95d4b-115">Open Visual Studio</span></span>

* <span data-ttu-id="95d4b-116">Outils > Connexion à une base de données...</span><span class="sxs-lookup"><span data-stu-id="95d4b-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="95d4b-117">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="95d4b-118">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="95d4b-119">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="95d4b-120">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="95d4b-121">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="95d4b-122">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="95d4b-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="95d4b-123">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="95d4b-124">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="95d4b-124">Create a new project</span></span>

* <span data-ttu-id="95d4b-125">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95d4b-125">Open Visual Studio</span></span>

* <span data-ttu-id="95d4b-126">Fichier > Nouveau > Projet...</span><span class="sxs-lookup"><span data-stu-id="95d4b-126">File > New > Project...</span></span>

* <span data-ttu-id="95d4b-127">Dans le menu de gauche, sélectionnez Modèles > Visual C# > Windows.</span><span class="sxs-lookup"><span data-stu-id="95d4b-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="95d4b-128">Choisissez le modèle de projet **Application console**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="95d4b-129">Assurez-vous de bien cibler **.NET Framework 4.5.1** ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="95d4b-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="95d4b-130">Nommez le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="95d4b-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="95d4b-131">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="95d4b-131">Install Entity Framework</span></span>

<span data-ttu-id="95d4b-132">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="95d4b-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="95d4b-133">Cette procédure pas à pas utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="95d4b-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="95d4b-134">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="95d4b-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="95d4b-135">Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="95d4b-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="95d4b-136">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="95d4b-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="95d4b-137">Pour activer l’ingénierie à rebours à partir d’une base de données existante, vous devez également installer deux autres packages.</span><span class="sxs-lookup"><span data-stu-id="95d4b-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="95d4b-138">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="95d4b-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="95d4b-139">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="95d4b-139">Reverse engineer your model</span></span>

<span data-ttu-id="95d4b-140">Créons à présent le modèle EF à partir de votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="95d4b-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="95d4b-141">Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="95d4b-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="95d4b-142">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="95d4b-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="95d4b-143">Le processus d’ingénierie à rebours créé des classes d’entité et un contexte dérivé en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="95d4b-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="95d4b-144">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="95d4b-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="95d4b-145">Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="95d4b-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="95d4b-146">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="95d4b-146">Use your model</span></span>

<span data-ttu-id="95d4b-147">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="95d4b-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="95d4b-148">Ouvrez *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="95d4b-148">Open *Program.cs*</span></span>

* <span data-ttu-id="95d4b-149">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="95d4b-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="95d4b-150">Déboguer > Exécuter sans débogage</span><span class="sxs-lookup"><span data-stu-id="95d4b-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="95d4b-151">Vous verrez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="95d4b-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![image](_static/output-existing-db.png)
