---
title: Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 878478099878e4a0bc65c44fef0609d28f39f2b8
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834771"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3dbdc-102">Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="3dbdc-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3dbdc-103">Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3dbdc-104">Le tutoriel utilise des migrations pour créer la base de données à partir du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="3dbdc-105">Vous pouvez suivre le tutoriel à l’aide de Visual Studio 2017 sur Windows, ou à l’aide de l’interface CLI .NET Core sur Windows, macOS ou Linux.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="3dbdc-106">Affichez l’exemple proposé dans cet article sur GitHub :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="3dbdc-107">Visual Studio 2017 avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="3dbdc-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="3dbdc-108">[CLI .NET Core avec SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="3dbdc-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3dbdc-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="3dbdc-109">Prerequisites</span></span>

<span data-ttu-id="3dbdc-110">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-112">[Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec ces charges de travail :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3dbdc-113">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3dbdc-114">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3dbdc-115">[SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-116">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-117">[SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="3dbdc-118">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="3dbdc-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-120">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3dbdc-121">**Fichier > Nouveau > Projet**</span><span class="sxs-lookup"><span data-stu-id="3dbdc-121">**File > New > Project**</span></span>
* <span data-ttu-id="3dbdc-122">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3dbdc-123">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3dbdc-124">Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3dbdc-125">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3dbdc-126">Vérifiez que les options **.NET Core** et **ASP.NET Core 2.1** sont sélectionnées dans les listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="3dbdc-127">Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3dbdc-128">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3dbdc-129">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-129">Click **OK**</span></span>

<span data-ttu-id="3dbdc-130">Avertissement : Si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté au projet dans `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3dbdc-131">À l’aide des techniques présentées dans ce tutoriel, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-132">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-133">Exécutez la commande suivante pour créer un projet MVC :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="3dbdc-134">Accédez au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-134">Change to the project directory.</span></span> <span data-ttu-id="3dbdc-135">Les commandes suivantes que vous entrez doivent s’exécuter sur le nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="3dbdc-136">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-136">Install Entity Framework Core</span></span>

<span data-ttu-id="3dbdc-137">Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3dbdc-138">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3dbdc-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3dbdc-140">Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="3dbdc-141">Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="3dbdc-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-142">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3dbdc-143">Ce tutoriel utilise SQLite, car il s’exécute sur toutes les plateformes prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="3dbdc-144">Exécutez la commande suivante pour installer le fournisseur SQLite :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="3dbdc-145">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="3dbdc-145">Create the model</span></span>

<span data-ttu-id="3dbdc-146">Définissez une classe de contexte et des classes d’entité qui composent le modèle.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-148">Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3dbdc-149">Entrez le nom **Model.cs** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3dbdc-150">Remplacez le contenu du fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-151">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-152">Dans le dossier **Models**, créez un fichier **Model.cs** avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="3dbdc-153">Une application de production placerait généralement chaque classe dans un fichier distinct.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="3dbdc-154">Par souci de simplicité, ce tutoriel place ces classes dans un même fichier.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="3dbdc-155">Inscrire le contexte avec l’injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="3dbdc-155">Register the context with dependency injection</span></span>

<span data-ttu-id="3dbdc-156">Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3dbdc-157">Ces services sont affectés aux composants qui en ont besoin (tels que les contrôleurs MVC) par le biais de propriétés ou de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3dbdc-158">Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-160">Dans **Startup.cs** ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="3dbdc-161">Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-162">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-163">Dans **Startup.cs** ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="3dbdc-164">Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="3dbdc-165">Une application de production placerait généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="3dbdc-166">Par souci de simplicité, ce tutoriel la définit dans le code.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="3dbdc-167">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="3dbdc-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="3dbdc-168">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="3dbdc-168">Create the database</span></span>

<span data-ttu-id="3dbdc-169">Les étapes suivantes utilisent des [migrations](xref:core/managing-schemas/migrations/index) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-169">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-171">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="3dbdc-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="3dbdc-172">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="3dbdc-173">Si vous obtenez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="3dbdc-174">La commande `Add-Migration` structure une migration afin de créer le jeu initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="3dbdc-175">La commande `Update-Database` crée la base de données et lui applique la nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-176">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-177">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="3dbdc-178">La commande `migrations` structure une migration afin de créer le jeu initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="3dbdc-179">La commande `database update` crée la base de données et lui applique la nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="3dbdc-180">Créer un contrôleur</span><span class="sxs-lookup"><span data-stu-id="3dbdc-180">Create a controller</span></span>

<span data-ttu-id="3dbdc-181">Générez automatiquement un contrôleur et des vues pour l’entité `Blog`.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-183">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3dbdc-184">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="3dbdc-185">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3dbdc-186">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-187">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3dbdc-188">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="3dbdc-189">Les commandes `tool install` et `add package` installent les outils qui peuvent structurer les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="3dbdc-190">La commande `restore` garantit que tous les packages du projet sont téléchargés, et la commande `aspnet-codegenerator` effectue la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="3dbdc-191">Le moteur de génération de modèles automatique crée les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="3dbdc-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="3dbdc-192">Un contrôleur (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="3dbdc-193">Des vues Razor pour les pages Créer, Supprimer, Détails, Modifier et Index (_Views/Movies/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="3dbdc-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Movies/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3dbdc-194">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="3dbdc-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3dbdc-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dbdc-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3dbdc-196">**Déboguer** > **Exécuter sans débogage**</span><span class="sxs-lookup"><span data-stu-id="3dbdc-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3dbdc-197">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="3dbdc-198">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="3dbdc-199">Utilisez le lien **Créer** pour créer des entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Créer une page](_static/create.png)

* <span data-ttu-id="3dbdc-201">Testez les liens **Détails**, **Modifier** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="3dbdc-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Page d’index](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3dbdc-203">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3dbdc-203">Additional Resources</span></span>

* [<span data-ttu-id="3dbdc-204">Tutoriel : Bien démarrer avec EF Core sur .NET Core avec une nouvelle base de données à l’aide de SQLite</span><span class="sxs-lookup"><span data-stu-id="3dbdc-204">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="3dbdc-205">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-205">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3dbdc-206">Tutoriel : Pages Razor avec Entity Framework Core dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3dbdc-206">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
