---
title: Bien démarrer avec UWP - Nouvelle base de données - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996908"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="90160-102">Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="90160-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="90160-103">Dans ce tutoriel, vous allez générer une application UWP (Universal Windows Platform) qui effectue un accès aux données élémentaire sur une base de données SQLite locale à l’aide d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="90160-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="90160-104">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="90160-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90160-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="90160-105">Prerequisites</span></span>

* <span data-ttu-id="90160-106">[Windows 10 Fall Creators Update (10.0 ; Build 16299) ou version ultérieure](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="90160-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="90160-107">[Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **Développement pour la plateforme Windows universelle**.</span><span class="sxs-lookup"><span data-stu-id="90160-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="90160-108">[SDK .NET Core 2.1](https://www.microsoft.com/net/core) ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="90160-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="90160-109">Créer un projet de modèle</span><span class="sxs-lookup"><span data-stu-id="90160-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90160-110">En raison des limitations au niveau de l’interaction des outils .NET Core avec les projets UWP, le modèle doit être placé dans un projet autre que UWP pour que vous puissiez exécuter des commandes de migration dans la **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="90160-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="90160-111">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90160-111">Open Visual Studio</span></span>

* <span data-ttu-id="90160-112">**Fichier > Nouveau > Projet**</span><span class="sxs-lookup"><span data-stu-id="90160-112">**File > New > Project**</span></span>

* <span data-ttu-id="90160-113">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="90160-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="90160-114">Sélectionnez le modèle **Bibliothèque de classes (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="90160-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="90160-115">Nommez le projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="90160-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="90160-116">Nommez la solution *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="90160-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="90160-117">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="90160-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="90160-118">Installer Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="90160-118">Install Entity Framework Core</span></span>

<span data-ttu-id="90160-119">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="90160-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="90160-120">Ce tutoriel utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="90160-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="90160-121">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="90160-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="90160-122">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="90160-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="90160-123">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="90160-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="90160-124">Un peu plus loin dans ce tutoriel, vous utiliserez certains outils Entity Framework Core pour tenir à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="90160-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="90160-125">Installez donc le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="90160-125">So install the tools package as well.</span></span>

* <span data-ttu-id="90160-126">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="90160-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="90160-127">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="90160-127">Create the model</span></span>

<span data-ttu-id="90160-128">Définissons à présent le contexte et les classes d’entité qui composent le modèle.</span><span class="sxs-lookup"><span data-stu-id="90160-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="90160-129">Supprimez *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="90160-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="90160-130">Créez *Model.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="90160-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="90160-131">Créer un projet UWP</span><span class="sxs-lookup"><span data-stu-id="90160-131">Create a new UWP project</span></span>

* <span data-ttu-id="90160-132">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="90160-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="90160-133">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows universel**.</span><span class="sxs-lookup"><span data-stu-id="90160-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="90160-134">Sélectionnez le modèle de projet **Application vide (Windows universel)**.</span><span class="sxs-lookup"><span data-stu-id="90160-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="90160-135">Nommez le projet *Blogging.UWP*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="90160-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="90160-136">Définissez les versions minimale et cible sur **Windows 10 Fall Creators Update (10.0 ; build 16299.0)** au minimum.</span><span class="sxs-lookup"><span data-stu-id="90160-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="90160-137">Créer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="90160-137">Create the initial migration</span></span>

<span data-ttu-id="90160-138">Maintenant que vous avez un modèle, configurez l’application pour créer une base de données lors de sa première exécution.</span><span class="sxs-lookup"><span data-stu-id="90160-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="90160-139">Dans cette section, vous allez créer la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="90160-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="90160-140">Dans la section suivante, vous ajouterez du code qui applique cette migration au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="90160-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="90160-141">Les outils de migration nécessitent un projet de démarrage autre que UWP. Vous allez donc le créer en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="90160-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="90160-142">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="90160-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="90160-143">Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="90160-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="90160-144">Sélectionnez le modèle de projet **Application console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="90160-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="90160-145">Nommez le projet *Blogging.Migrations.Startup*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="90160-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="90160-146">Ajoutez une référence de projet du projet *Blogging.Migrations.Startup* au projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="90160-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="90160-147">Vous pouvez maintenant créer votre migration initiale.</span><span class="sxs-lookup"><span data-stu-id="90160-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="90160-148">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="90160-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="90160-149">Sélectionnez le projet *Blogging.Model* comme **Projet par défaut**.</span><span class="sxs-lookup"><span data-stu-id="90160-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="90160-150">Dans l’**Explorateur de solutions**, définissez le projet *Blogging.Migrations.Startup* comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="90160-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="90160-151">Exécutez `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="90160-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="90160-152">Cette commande génère automatiquement un modèle de migration qui crée l’ensemble initial de tables de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="90160-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="90160-153">Créer la base de données au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="90160-153">Create the database on app startup</span></span>

<span data-ttu-id="90160-154">Étant donné que vous souhaitez créer la base de données sur l’appareil sur lequel l’application s’exécute, vous allez ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="90160-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="90160-155">Lors de la première exécution de l’application, ce code se chargera de créer la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="90160-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="90160-156">Ajoutez une référence de projet du projet *Blogging.UWP* au projet *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="90160-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="90160-157">Ouvrez *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="90160-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="90160-158">Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.</span><span class="sxs-lookup"><span data-stu-id="90160-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="90160-159">Si vous modifiez votre modèle, utilisez la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration afin d’appliquer les modifications correspondantes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="90160-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="90160-160">Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="90160-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="90160-161">EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="90160-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="90160-162">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="90160-162">Use the model</span></span>

<span data-ttu-id="90160-163">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="90160-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="90160-164">Ouvrez *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="90160-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="90160-165">Ajoutez le gestionnaire de chargement de page et le contenu de l’interface utilisateur mis en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="90160-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="90160-166">Maintenant, ajoutez du code pour connecter l’interface utilisateur à la base de données.</span><span class="sxs-lookup"><span data-stu-id="90160-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="90160-167">Ouvrez *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="90160-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="90160-168">Ajoutez le code mis en surbrillance de la liste suivante :</span><span class="sxs-lookup"><span data-stu-id="90160-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="90160-169">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="90160-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="90160-170">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet *Blogging.UWP*, puis sélectionnez **Déployer**.</span><span class="sxs-lookup"><span data-stu-id="90160-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="90160-171">Définissez *Blogging.UWP* comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="90160-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="90160-172">**Déboguer > Démarrer sans débogage**</span><span class="sxs-lookup"><span data-stu-id="90160-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="90160-173">L’application est générée et s’exécute.</span><span class="sxs-lookup"><span data-stu-id="90160-173">The app builds and runs.</span></span>

* <span data-ttu-id="90160-174">Entrez une URL et cliquez sur le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="90160-174">Enter a URL and click the **Add** button</span></span>

  ![image](_static/create.png)

  ![image](_static/list.png)

  <span data-ttu-id="90160-177">Et le tour est joué !</span><span class="sxs-lookup"><span data-stu-id="90160-177">Tada!</span></span> <span data-ttu-id="90160-178">Vous avez désormais une application UWP simple exécutant Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="90160-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90160-179">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="90160-179">Next steps</span></span>

<span data-ttu-id="90160-180">Pour accéder à des informations sur la compatibilité et les performances que vous devez connaître quand vous utilisez EF Core avec UWP, consultez [Implémentations de .NET prises en charge par EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="90160-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="90160-181">Consultez les autres articles de cette documentation pour en savoir plus sur les fonctionnalités d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="90160-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
