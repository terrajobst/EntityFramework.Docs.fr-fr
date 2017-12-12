---
title: "Bien démarrer avec .NET Framework - Nouvelle base de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="aa615-102">Bien démarrer avec EF Core sur .NET Framework avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="aa615-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="aa615-103">Dans cette procédure pas à pas, vous allez générer une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa615-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="aa615-104">Vous utiliserez des migrations pour créer la base de données à partir de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="aa615-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="aa615-105">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="aa615-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa615-106">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="aa615-106">Prerequisites</span></span>

<span data-ttu-id="aa615-107">Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :</span><span class="sxs-lookup"><span data-stu-id="aa615-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="aa615-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="aa615-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="aa615-109">Dernière version du Gestionnaire de package NuGet</span><span class="sxs-lookup"><span data-stu-id="aa615-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="aa615-110">Dernière version de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa615-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="aa615-111">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="aa615-111">Create a new project</span></span>

* <span data-ttu-id="aa615-112">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa615-112">Open Visual Studio</span></span>

* <span data-ttu-id="aa615-113">Fichier > Nouveau > Projet...</span><span class="sxs-lookup"><span data-stu-id="aa615-113">File > New > Project...</span></span>

* <span data-ttu-id="aa615-114">Dans le menu de gauche, sélectionnez Modèles > Visual C# > Bureau classique Windows.</span><span class="sxs-lookup"><span data-stu-id="aa615-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="aa615-115">Sélectionnez le modèle de projet **Application console (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="aa615-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="aa615-116">Assurez-vous de bien cibler **.NET Framework 4.5.1** ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="aa615-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="aa615-117">Nommez le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa615-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="aa615-118">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="aa615-118">Install Entity Framework</span></span>

<span data-ttu-id="aa615-119">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="aa615-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="aa615-120">Cette procédure pas à pas utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aa615-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="aa615-121">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="aa615-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="aa615-122">Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="aa615-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="aa615-123">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="aa615-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="aa615-124">Un peu plus loin dans cette procédure pas à pas, nous utiliserons également des outils Entity Framework pour gérer la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="aa615-125">Nous installerons donc également le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="aa615-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="aa615-126">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="aa615-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="aa615-127">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="aa615-127">Create your model</span></span>

<span data-ttu-id="aa615-128">Définissons à présent le contexte et les classes d’entité qui composeront le modèle.</span><span class="sxs-lookup"><span data-stu-id="aa615-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="aa615-129">Projet > Ajouter une classe...</span><span class="sxs-lookup"><span data-stu-id="aa615-129">Project > Add Class...</span></span>

* <span data-ttu-id="aa615-130">Entrez le nom *Model.cs* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa615-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="aa615-131">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="aa615-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="aa615-132">Dans une application réelle, vous placeriez chaque classe dans un fichier distinct et la chaîne de connexion dans le fichier `App.Config` que vous liriez à l’aide de `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="aa615-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="aa615-133">Dans le cadre de ce didacticiel, par souci de simplicité, nous plaçons tout dans un même fichier de code.</span><span class="sxs-lookup"><span data-stu-id="aa615-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="aa615-134">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="aa615-134">Create your database</span></span>

<span data-ttu-id="aa615-135">Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="aa615-136">Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="aa615-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="aa615-137">Exécutez `Add-Migration MyFirstMigration` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="aa615-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="aa615-138">Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="aa615-139">Étant donné que votre base de données n’existe pas encore, elle sera créée pour vous avant l’application de la migration.</span><span class="sxs-lookup"><span data-stu-id="aa615-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="aa615-140">Si vous apportez des modifications ultérieures au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les modifications du schéma correspondantes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="aa615-141">Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `Update-Database` pour appliquer les modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="aa615-142">EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa615-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="aa615-143">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="aa615-143">Use your model</span></span>

<span data-ttu-id="aa615-144">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="aa615-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="aa615-145">Ouvrez *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa615-145">Open *Program.cs*</span></span>

* <span data-ttu-id="aa615-146">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="aa615-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="aa615-147">Déboguer > Exécuter sans débogage</span><span class="sxs-lookup"><span data-stu-id="aa615-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="aa615-148">Vous verrez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="aa615-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![image](_static/output-new-db.png)
