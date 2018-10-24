---
title: Bien démarrer avec .NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
description: Bien démarrer avec .NET Core à l’aide d’Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: ec20040917a2bca8177924b6905b1cd79e5cd9da
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834732"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="fa5a0-103">Bien démarrer avec EF Core sur une application console .NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="fa5a0-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="fa5a0-104">Dans ce tutoriel, vous allez créer une application console .NET Core qui effectue un accès aux données d’une base de données SQLite à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="fa5a0-105">Vous utilisez des migrations pour créer la base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="fa5a0-106">Consultez [ASP.NET Core - Nouvelle base de données](xref:core/get-started/aspnetcore/new-db) pour une version de Visual Studio utilisant ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="fa5a0-107">Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="fa5a0-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa5a0-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fa5a0-108">Prerequisites</span></span>

* [<span data-ttu-id="fa5a0-109">SDK .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="fa5a0-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="fa5a0-110">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="fa5a0-110">Create a new project</span></span>

* <span data-ttu-id="fa5a0-111">Créez un projet de console :</span><span class="sxs-lookup"><span data-stu-id="fa5a0-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="fa5a0-112">Changer le répertoire actif</span><span class="sxs-lookup"><span data-stu-id="fa5a0-112">Change the current directory</span></span>

<span data-ttu-id="fa5a0-113">Lors des étapes suivantes, nous devons émettre des commandes `dotnet` sur l’application.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="fa5a0-114">Nous sélectionnons le répertoire de l’application comme répertoire actif en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="fa5a0-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="fa5a0-115">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fa5a0-115">Install Entity Framework Core</span></span>

<span data-ttu-id="fa5a0-116">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="fa5a0-117">Cette procédure pas à pas utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="fa5a0-118">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="fa5a0-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="fa5a0-119">Installez Microsoft.EntityFrameworkCore.Sqlite et Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="fa5a0-120">Exécutez `dotnet restore` pour installer les nouveaux packages.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="fa5a0-121">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="fa5a0-121">Create the model</span></span>

<span data-ttu-id="fa5a0-122">Définissez le contexte et les classes d’entité qui composeront le modèle.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="fa5a0-123">Créez un nouveau fichier *Model.cs* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="fa5a0-124">Conseil : Dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="fa5a0-125">Pour simplifier le tutoriel, tout est placé dans un seul et même fichier.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="fa5a0-126">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="fa5a0-126">Create the database</span></span>

<span data-ttu-id="fa5a0-127">Une fois que vous avez un modèle, vous utilisez des [migrations](xref:core/managing-schemas/migrations/index) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="fa5a0-128">Exécutez `dotnet ef migrations add InitialCreate` pour générer automatiquement un modèle de migration et créer l’ensemble initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="fa5a0-129">Exécutez `dotnet ef database update` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="fa5a0-130">Cette commande crée la base de données avant d’appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="fa5a0-131">La base de données SQLite *blogging.db*\* se trouve dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="fa5a0-132">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="fa5a0-132">Use the model</span></span>

* <span data-ttu-id="fa5a0-133">Ouvrez le fichier *Program.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="fa5a0-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="fa5a0-134">Testez l’application à partir de la console.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-134">Test the app from the console.</span></span> <span data-ttu-id="fa5a0-135">Consultez la [Remarque sur Visual Studio](#vs) pour exécuter l’application à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="fa5a0-136">Un blog est enregistré dans la base de données et les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="fa5a0-137">Modification du modèle :</span><span class="sxs-lookup"><span data-stu-id="fa5a0-137">Changing the model:</span></span>

- <span data-ttu-id="fa5a0-138">Si vous apportez des modifications au modèle, vous pouvez utiliser la commande `dotnet ef migrations add` pour générer automatiquement une nouvelle [migration](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="fa5a0-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="fa5a0-139">Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `dotnet ef database update` pour appliquer les modifications de schéma à la base de données.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="fa5a0-140">EF Core utilise une table `__EFMigrationsHistory` dans la base de données pour effectuer le suivi des migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="fa5a0-141">Le moteur de base de données SQLite ne prend pas en charge certaines modifications de schéma qui sont prises en charge par la plupart des autres bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="fa5a0-142">Par exemple, l’opération `DropColumn` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="fa5a0-143">Les migrations EF Core génèrent du code pour ces opérations,</span><span class="sxs-lookup"><span data-stu-id="fa5a0-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="fa5a0-144">mais si vous tentez de les appliquer à une base de données ou de générer un script, EF Core lève des exceptions.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="fa5a0-145">Consultez [Limitations de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="fa5a0-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="fa5a0-146">Pour tout nouveau développement, il est préférable de supprimer la base de données et d’en créer une nouvelle plutôt que d’utiliser des migrations quand le modèle change.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="fa5a0-147">Exécuter à partir de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa5a0-147">Run from Visual Studio</span></span>

<span data-ttu-id="fa5a0-148">Pour exécuter cet exemple à partir de Visual Studio, vous devez définir manuellement le répertoire de travail comme racine du projet.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="fa5a0-149">Si vous ne définissez pas le répertoire de travail, l’exception `Microsoft.Data.Sqlite.SqliteException` suivante est levée : `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="fa5a0-150">Pour définir le répertoire de travail :</span><span class="sxs-lookup"><span data-stu-id="fa5a0-150">To set the working directory:</span></span>

* <span data-ttu-id="fa5a0-151">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="fa5a0-152">Sélectionnez l’onglet **Déboguer** dans le volet gauche.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="fa5a0-153">Définissez le répertoire du projet comme **Répertoire de travail**.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="fa5a0-154">Enregistrez les modifications.</span><span class="sxs-lookup"><span data-stu-id="fa5a0-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa5a0-155">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fa5a0-155">Additional Resources</span></span>

* [<span data-ttu-id="fa5a0-156">Tutoriel : Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données à l’aide de SQLite</span><span class="sxs-lookup"><span data-stu-id="fa5a0-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="fa5a0-157">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa5a0-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="fa5a0-158">Tutoriel : Pages Razor avec Entity Framework Core dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa5a0-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
