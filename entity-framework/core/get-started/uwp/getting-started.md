---
title: Bien démarrer avec UWP - Nouvelle base de données - EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022374"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="93c78-102">Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="93c78-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="93c78-103">Dans ce tutoriel, vous allez générer une application UWP (Universal Windows Platform) qui effectue un accès aux données élémentaire sur une base de données SQLite locale à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="93c78-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="93c78-104">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="93c78-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93c78-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="93c78-105">Prerequisites</span></span>

* <span data-ttu-id="93c78-106">[Windows 10 Fall Creators Update (10.0 ; Build 16299) ou version ultérieure](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="93c78-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="93c78-107">[Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **Développement pour la plateforme Windows universelle**.</span><span class="sxs-lookup"><span data-stu-id="93c78-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="93c78-108">[SDK .NET Core 2.1](https://www.microsoft.com/net/core) ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="93c78-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c78-109">Ce tutoriel utilise des commandes de [migration](xref:core/managing-schemas/migrations/index) Entity Framework Core pour créer et mettre à jour le schéma de la base de données.</span><span class="sxs-lookup"><span data-stu-id="93c78-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="93c78-110">Ces commandes ne fonctionnent pas directement avec les projets UWP.</span><span class="sxs-lookup"><span data-stu-id="93c78-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="93c78-111">Pour cette raison, le modèle de données de l’application est placé dans un projet de bibliothèque partagé, et une application console .NET Core distincte est utilisée pour exécuter les commandes.</span><span class="sxs-lookup"><span data-stu-id="93c78-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="93c78-112">Créer un projet de bibliothèque pour stocker le modèle de données</span><span class="sxs-lookup"><span data-stu-id="93c78-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="93c78-113">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93c78-113">Open Visual Studio</span></span>

* <span data-ttu-id="93c78-114">**Fichier > Nouveau > Projet**</span><span class="sxs-lookup"><span data-stu-id="93c78-114">**File > New > Project**</span></span>

* <span data-ttu-id="93c78-115">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="93c78-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="93c78-116">Sélectionnez le modèle **Bibliothèque de classes (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="93c78-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="93c78-117">Nommez le projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="93c78-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="93c78-118">Nommez la solution *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="93c78-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="93c78-119">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="93c78-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="93c78-120">Installer le runtime Entity Framework Core dans le projet de modèle de données</span><span class="sxs-lookup"><span data-stu-id="93c78-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="93c78-121">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="93c78-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="93c78-122">Ce tutoriel utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="93c78-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="93c78-123">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="93c78-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="93c78-124">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="93c78-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="93c78-125">Vérifiez que le projet de bibliothèque *Blogging.Model* est sélectionné comme **Projet par défaut** dans la Console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="93c78-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="93c78-126">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="93c78-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="93c78-127">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="93c78-127">Create the data model</span></span>

<span data-ttu-id="93c78-128">Définissons à présent le *DbContext* et les classes d’entité qui composent le modèle.</span><span class="sxs-lookup"><span data-stu-id="93c78-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="93c78-129">Supprimez *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="93c78-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="93c78-130">Créez *Model.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="93c78-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="93c78-131">Créer un projet de console pour exécuter des commandes de migration</span><span class="sxs-lookup"><span data-stu-id="93c78-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="93c78-132">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="93c78-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="93c78-133">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93c78-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="93c78-134">Sélectionnez le modèle de projet **Application console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="93c78-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="93c78-135">Nommez le projet *Blogging.Migrations.Startup*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="93c78-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="93c78-136">Ajoutez une référence de projet du projet *Blogging.Migrations.Startup* au projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="93c78-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="93c78-137">Installer les outils Entity Framework Core dans le projet de démarrage de migrations</span><span class="sxs-lookup"><span data-stu-id="93c78-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="93c78-138">Pour activer les commandes de migration EF Core dans la Console du Gestionnaire de package, installez le package d’outils EF Core dans l’application console.</span><span class="sxs-lookup"><span data-stu-id="93c78-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="93c78-139">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="93c78-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="93c78-140">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`.</span><span class="sxs-lookup"><span data-stu-id="93c78-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="93c78-141">Créer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="93c78-141">Create the initial migration</span></span>

 <span data-ttu-id="93c78-142">Créez la migration initiale, en spécifiant l’application console comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="93c78-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="93c78-143">Exécutez `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`.</span><span class="sxs-lookup"><span data-stu-id="93c78-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="93c78-144">Cette commande génère automatiquement un modèle de migration qui crée l’ensemble initial de tables de bases de données pour votre modèle de données.</span><span class="sxs-lookup"><span data-stu-id="93c78-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="93c78-145">Créer le projet UWP</span><span class="sxs-lookup"><span data-stu-id="93c78-145">Create the UWP project</span></span>

* <span data-ttu-id="93c78-146">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="93c78-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="93c78-147">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows universel**.</span><span class="sxs-lookup"><span data-stu-id="93c78-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="93c78-148">Sélectionnez le modèle de projet **Application vide (Windows universel)**.</span><span class="sxs-lookup"><span data-stu-id="93c78-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="93c78-149">Nommez le projet *Blogging.UWP*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="93c78-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c78-150">Définissez les versions minimale et cible sur **Windows 10 Fall Creators Update (10.0 ; build 16299.0)** au minimum.</span><span class="sxs-lookup"><span data-stu-id="93c78-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="93c78-151">Les versions précédentes de Windows 10 ne prennent pas en charge .NET Standard 2.0, qui est exigé par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="93c78-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="93c78-152">Ajouter du code pour créer la base de données au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="93c78-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="93c78-153">Étant donné que vous souhaitez créer la base de données sur l’appareil sur lequel l’application s’exécute, vous allez ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="93c78-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="93c78-154">Lors de la première exécution de l’application, ce code se chargera de créer la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="93c78-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="93c78-155">Ajoutez une référence de projet du projet *Blogging.UWP* au projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="93c78-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="93c78-156">Ouvrez *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="93c78-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="93c78-157">Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.</span><span class="sxs-lookup"><span data-stu-id="93c78-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="93c78-158">Si vous modifiez votre modèle, utilisez la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration afin d’appliquer les modifications correspondantes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="93c78-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="93c78-159">Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="93c78-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="93c78-160">EF Core utilise une table `__EFMigrationsHistory` dans la base de données pour effectuer le suivi des migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="93c78-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="93c78-161">Utiliser le modèle de données</span><span class="sxs-lookup"><span data-stu-id="93c78-161">Use the data model</span></span>

<span data-ttu-id="93c78-162">Vous pouvez à présent utiliser EF Core pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="93c78-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="93c78-163">Ouvrez *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="93c78-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="93c78-164">Ajoutez le gestionnaire de chargement de page et le contenu de l’interface utilisateur mis en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="93c78-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="93c78-165">Maintenant, ajoutez du code pour connecter l’interface utilisateur à la base de données.</span><span class="sxs-lookup"><span data-stu-id="93c78-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="93c78-166">Ouvrez *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="93c78-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="93c78-167">Ajoutez le code mis en surbrillance de la liste suivante :</span><span class="sxs-lookup"><span data-stu-id="93c78-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="93c78-168">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="93c78-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="93c78-169">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet *Blogging.UWP*, puis sélectionnez **Déployer**.</span><span class="sxs-lookup"><span data-stu-id="93c78-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="93c78-170">Définissez *Blogging.UWP* comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="93c78-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="93c78-171">**Déboguer > Démarrer sans débogage**</span><span class="sxs-lookup"><span data-stu-id="93c78-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="93c78-172">L’application est générée et s’exécute.</span><span class="sxs-lookup"><span data-stu-id="93c78-172">The app builds and runs.</span></span>

* <span data-ttu-id="93c78-173">Entrez une URL et cliquez sur le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="93c78-173">Enter a URL and click the **Add** button</span></span>

  ![image](_static/create.png)

  ![image](_static/list.png)

  <span data-ttu-id="93c78-176">Et le tour est joué !</span><span class="sxs-lookup"><span data-stu-id="93c78-176">Tada!</span></span> <span data-ttu-id="93c78-177">Vous avez désormais une application UWP simple exécutant Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="93c78-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93c78-178">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="93c78-178">Next steps</span></span>

<span data-ttu-id="93c78-179">Pour accéder à des informations sur la compatibilité et les performances que vous devez connaître quand vous utilisez EF Core avec UWP, consultez [Implémentations de .NET prises en charge par EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="93c78-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="93c78-180">Consultez les autres articles de cette documentation pour en savoir plus sur les fonctionnalités d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="93c78-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
