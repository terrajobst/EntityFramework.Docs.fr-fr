---
title: Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996062"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="561b1-102">Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="561b1-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="561b1-103">Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="561b1-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="561b1-104">Vous utilisez des migrations pour créer la base de données à partir de votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="561b1-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="561b1-105">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="561b1-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="561b1-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="561b1-106">Prerequisites</span></span>

<span data-ttu-id="561b1-107">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="561b1-107">Install the following software:</span></span>

* <span data-ttu-id="561b1-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="561b1-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="561b1-109">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="561b1-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="561b1-110">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="561b1-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="561b1-111">[SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="561b1-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="561b1-112">Créer un projet dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="561b1-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="561b1-113">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="561b1-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="561b1-114">**Fichier > Nouveau > Projet**</span><span class="sxs-lookup"><span data-stu-id="561b1-114">**File > New > Project**</span></span>
* <span data-ttu-id="561b1-115">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="561b1-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="561b1-116">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="561b1-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="561b1-117">Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="561b1-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="561b1-118">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="561b1-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="561b1-119">Vérifiez que les options **.NET Core** et **ASP.NET Core 2.1** sont sélectionnées dans les listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="561b1-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="561b1-120">Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="561b1-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="561b1-121">Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="561b1-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="561b1-122">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="561b1-122">Click **OK**</span></span>

<span data-ttu-id="561b1-123">Avertissement: si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté à votre projet dans `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="561b1-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="561b1-124">À l’aide des techniques présentées dans ce tutoriel, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="561b1-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="561b1-125">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="561b1-125">Install Entity Framework Core</span></span>

<span data-ttu-id="561b1-126">Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler.</span><span class="sxs-lookup"><span data-stu-id="561b1-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="561b1-127">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="561b1-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="561b1-128">Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="561b1-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="561b1-129">Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="561b1-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="561b1-130">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="561b1-130">Create the model</span></span>

<span data-ttu-id="561b1-131">Définissez une classe de contexte et les classes d’entité qui composent le modèle :</span><span class="sxs-lookup"><span data-stu-id="561b1-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="561b1-132">Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.</span><span class="sxs-lookup"><span data-stu-id="561b1-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="561b1-133">Entrez le nom **Model.cs** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="561b1-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="561b1-134">Remplacez le contenu du fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="561b1-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="561b1-135">Dans une application réelle, vous placez généralement chaque classe de votre modèle dans un fichier distinct.</span><span class="sxs-lookup"><span data-stu-id="561b1-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="561b1-136">Par souci de simplicité, ce tutoriel place toutes les classes dans un même fichier.</span><span class="sxs-lookup"><span data-stu-id="561b1-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="561b1-137">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="561b1-137">Register your context with dependency injection</span></span>

<span data-ttu-id="561b1-138">Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="561b1-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="561b1-139">Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="561b1-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="561b1-140">Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.</span><span class="sxs-lookup"><span data-stu-id="561b1-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="561b1-141">Ouvrez **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="561b1-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="561b1-142">Ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="561b1-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="561b1-143">Appelez la méthode `AddDbContext` pour inscrire le contexte en tant que service.</span><span class="sxs-lookup"><span data-stu-id="561b1-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="561b1-144">Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="561b1-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="561b1-145">Remarque : Une application réelle place généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="561b1-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="561b1-146">Par souci de simplicité, ce tutoriel la définit dans le code.</span><span class="sxs-lookup"><span data-stu-id="561b1-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="561b1-147">Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="561b1-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="561b1-148">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="561b1-148">Create the database</span></span>

<span data-ttu-id="561b1-149">Une fois que vous avez un modèle, vous pouvez utiliser des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="561b1-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="561b1-150">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="561b1-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="561b1-151">Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="561b1-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="561b1-152">Si vous recevez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="561b1-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="561b1-153">Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="561b1-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="561b1-154">Cette commande crée la base de données avant d’appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="561b1-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="561b1-155">Créer un contrôleur</span><span class="sxs-lookup"><span data-stu-id="561b1-155">Create a controller</span></span>

<span data-ttu-id="561b1-156">Générez automatiquement un contrôleur et des vues pour l’entité `Blog`.</span><span class="sxs-lookup"><span data-stu-id="561b1-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="561b1-157">Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="561b1-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="561b1-158">Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="561b1-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="561b1-159">Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="561b1-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="561b1-160">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="561b1-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="561b1-161">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="561b1-161">Run the application</span></span>

<span data-ttu-id="561b1-162">Appuyez sur la touche F5 pour exécuter et tester l’application.</span><span class="sxs-lookup"><span data-stu-id="561b1-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="561b1-163">Accédez à `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="561b1-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="561b1-164">Utilisez le lien de création pour créer des entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="561b1-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="561b1-165">Testez les liens de détails et de suppression.</span><span class="sxs-lookup"><span data-stu-id="561b1-165">Test the details and delete links.</span></span>

![image](_static/create.png)

![image](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="561b1-168">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="561b1-168">Additional Resources</span></span>

* <span data-ttu-id="561b1-169">[EF - Nouvelle base de données avec SQLite](xref:core/get-started/netcore/new-db-sqlite) : didacticiel sur la console multiplateforme EF</span><span class="sxs-lookup"><span data-stu-id="561b1-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="561b1-170">Introduction à ASP.NET Core MVC sur Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="561b1-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="561b1-171">Introduction à ASP.NET Core MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="561b1-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="561b1-172">Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="561b1-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
