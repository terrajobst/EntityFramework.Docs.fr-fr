---
title: Bien démarrer avec .NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
description: Bien démarrer avec .NET Core à l’aide d’Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993690"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="d1df0-103">Bien démarrer avec EF Core sur une application console .NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="d1df0-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="d1df0-104">Dans ce tutoriel, vous allez créer une application console .NET Core qui effectue un accès aux données d’une base de données SQLite à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d1df0-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="d1df0-105">Vous utilisez des migrations pour créer la base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="d1df0-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="d1df0-106">Consultez [ASP.NET Core - Nouvelle base de données](xref:core/get-started/aspnetcore/new-db) pour une version de Visual Studio utilisant ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d1df0-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="d1df0-107">Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="d1df0-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1df0-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d1df0-108">Prerequisites</span></span>

* [<span data-ttu-id="d1df0-109">SDK .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="d1df0-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="d1df0-110">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="d1df0-110">Create a new project</span></span>

* <span data-ttu-id="d1df0-111">Créez un projet de console :</span><span class="sxs-lookup"><span data-stu-id="d1df0-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="d1df0-112">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d1df0-112">Install Entity Framework Core</span></span>

<span data-ttu-id="d1df0-113">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="d1df0-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="d1df0-114">Cette procédure pas à pas utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="d1df0-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="d1df0-115">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d1df0-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="d1df0-116">Installez Microsoft.EntityFrameworkCore.Sqlite et Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="d1df0-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="d1df0-117">Exécutez `dotnet restore` pour installer les nouveaux packages.</span><span class="sxs-lookup"><span data-stu-id="d1df0-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="d1df0-118">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="d1df0-118">Create the model</span></span>

<span data-ttu-id="d1df0-119">Définissez le contexte et les classes d’entité qui composeront le modèle.</span><span class="sxs-lookup"><span data-stu-id="d1df0-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="d1df0-120">Créez un nouveau fichier *Model.cs* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="d1df0-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="d1df0-121">Conseil : Dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d1df0-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="d1df0-122">Pour simplifier le tutoriel, tout est placé dans un seul et même fichier.</span><span class="sxs-lookup"><span data-stu-id="d1df0-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d1df0-123">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="d1df0-123">Create the database</span></span>

<span data-ttu-id="d1df0-124">Une fois que vous avez un modèle, vous utilisez des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="d1df0-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="d1df0-125">Exécutez `dotnet ef migrations add InitialCreate` pour générer automatiquement un modèle de migration et créer l’ensemble initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="d1df0-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="d1df0-126">Exécutez `dotnet ef database update` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d1df0-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="d1df0-127">Cette commande crée la base de données avant d’appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="d1df0-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="d1df0-128">La base de données SQLite *blogging.db*\* se trouve dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="d1df0-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="d1df0-129">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="d1df0-129">Use the model</span></span>

* <span data-ttu-id="d1df0-130">Ouvrez le fichier *Program.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d1df0-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="d1df0-131">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="d1df0-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="d1df0-132">Un blog est enregistré dans la base de données et les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="d1df0-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="d1df0-133">Modification du modèle :</span><span class="sxs-lookup"><span data-stu-id="d1df0-133">Changing the model:</span></span>

- <span data-ttu-id="d1df0-134">Si vous apportez des modifications au modèle, vous pouvez utiliser la commande `dotnet ef migrations add` pour générer automatiquement une nouvelle [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="d1df0-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="d1df0-135">Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `dotnet ef database update` pour appliquer les modifications de schéma à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d1df0-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="d1df0-136">EF Core utilise une table `__EFMigrationsHistory` dans la base de données pour effectuer le suivi des migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d1df0-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="d1df0-137">Le moteur de base de données SQLite ne prend pas en charge certaines modifications de schéma qui sont prises en charge par la plupart des autres bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="d1df0-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="d1df0-138">Par exemple, l’opération `DropColumn` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="d1df0-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="d1df0-139">Les migrations EF Core génèrent du code pour ces opérations,</span><span class="sxs-lookup"><span data-stu-id="d1df0-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="d1df0-140">mais si vous tentez de les appliquer à une base de données ou de générer un script, EF Core lève des exceptions.</span><span class="sxs-lookup"><span data-stu-id="d1df0-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="d1df0-141">Consultez [Limitations de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d1df0-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="d1df0-142">Pour tout nouveau développement, il est préférable de supprimer la base de données et d’en créer une nouvelle plutôt que d’utiliser des migrations quand le modèle change.</span><span class="sxs-lookup"><span data-stu-id="d1df0-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1df0-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d1df0-143">Additional Resources</span></span>

* [<span data-ttu-id="d1df0-144">Introduction à ASP.NET Core MVC sur Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="d1df0-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="d1df0-145">Introduction à ASP.NET Core MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1df0-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="d1df0-146">Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1df0-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
