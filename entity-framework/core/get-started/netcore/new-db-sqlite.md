---
title: Bien démarrer avec .NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Bien démarrer avec .NET Core à l’aide d’Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911500"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="e239f-104">Bien démarrer avec EF Core sur une application console .NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="e239f-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="e239f-105">Dans cette procédure pas à pas, vous créez une application console .NET Core qui effectue un accès aux données d’une base de données SQLite à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e239f-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="e239f-106">Vous utilisez des migrations pour créer la base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="e239f-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="e239f-107">Consultez [ASP.NET Core - Nouvelle base de données](xref:core/get-started/aspnetcore/new-db) pour une version de Visual Studio utilisant ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e239f-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="e239f-108">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="e239f-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e239f-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e239f-109">Prerequisites</span></span>

<span data-ttu-id="e239f-110">[Le SDK .NET Core](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="e239f-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="e239f-111">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="e239f-111">Create a new project</span></span>

* <span data-ttu-id="e239f-112">Créez un projet de console :</span><span class="sxs-lookup"><span data-stu-id="e239f-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="e239f-113">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e239f-113">Install Entity Framework Core</span></span>

<span data-ttu-id="e239f-114">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="e239f-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e239f-115">Cette procédure pas à pas utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="e239f-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="e239f-116">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e239f-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e239f-117">Installez Microsoft.EntityFrameworkCore.Sqlite et Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="e239f-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="e239f-118">Exécutez `dotnet restore` pour installer les nouveaux packages.</span><span class="sxs-lookup"><span data-stu-id="e239f-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e239f-119">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="e239f-119">Create the model</span></span>

<span data-ttu-id="e239f-120">Définissez le contexte et les classes d’entité qui composeront le modèle.</span><span class="sxs-lookup"><span data-stu-id="e239f-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="e239f-121">Créez un nouveau fichier *Model.cs* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="e239f-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="e239f-122">Conseil : dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="e239f-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="e239f-123">Pour simplifier le tutoriel, tout est placé dans un seul et même fichier.</span><span class="sxs-lookup"><span data-stu-id="e239f-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e239f-124">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="e239f-124">Create the database</span></span>

<span data-ttu-id="e239f-125">Une fois que vous avez un modèle, vous utilisez des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="e239f-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="e239f-126">Exécutez `dotnet ef migrations add InitialCreate` pour générer automatiquement un modèle de migration et créer l’ensemble initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="e239f-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="e239f-127">Exécutez `dotnet ef database update` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e239f-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="e239f-128">Cette commande crée la base de données avant d’appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="e239f-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="e239f-129">La base de données SQLite *blogging.db*\* se trouve dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e239f-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="e239f-130">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="e239f-130">Use your model</span></span>

* <span data-ttu-id="e239f-131">Ouvrez le fichier *Program.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e239f-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="e239f-132">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="e239f-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="e239f-133">Un blog est enregistré dans la base de données et les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="e239f-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="e239f-134">Modification du modèle :</span><span class="sxs-lookup"><span data-stu-id="e239f-134">Changing the model:</span></span>

- <span data-ttu-id="e239f-135">Si vous apportez des modifications au modèle, vous pouvez utiliser la commande `dotnet ef migrations add` pour générer automatiquement un nouveau modèle de [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour appliquer les modifications du schéma correspondantes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e239f-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="e239f-136">Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `dotnet ef database update` pour appliquer les modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e239f-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="e239f-137">EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e239f-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="e239f-138">SQLite ne prend pas en charge toutes les migrations (modifications du schéma) en raison des limitations de SQLite.</span><span class="sxs-lookup"><span data-stu-id="e239f-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="e239f-139">Consultez [Limitations de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="e239f-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="e239f-140">Pour tout nouveau développement, il est préférable de supprimer la base de données et d’en créer une nouvelle plutôt que d’utiliser des migrations quand votre modèle est modifié.</span><span class="sxs-lookup"><span data-stu-id="e239f-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e239f-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e239f-141">Additional Resources</span></span>

* <span data-ttu-id="e239f-142">[.NET Core - Nouvelle base de données avec SQLite](xref:core/get-started/netcore/new-db-sqlite) : didacticiel sur la console multiplateforme EF</span><span class="sxs-lookup"><span data-stu-id="e239f-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="e239f-143">Introduction à ASP.NET Core MVC sur Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="e239f-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="e239f-144">Introduction à ASP.NET Core MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e239f-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="e239f-145">Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e239f-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
