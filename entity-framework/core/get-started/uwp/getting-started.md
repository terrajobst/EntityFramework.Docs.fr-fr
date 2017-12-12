---
title: "Bien démarrer avec UWP - Nouvelle base de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="1a65c-102">Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="1a65c-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="1a65c-103">Ce didacticiel utilise EF Core 2.0.1 (version publiée avec ASP.NET Core et le Kit SDK .NET Core 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="1a65c-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="1a65c-104">EF Core 2.0.0 ne dispose pas de certains correctifs de bogues essentiels pour une bonne expérience UWP.</span><span class="sxs-lookup"><span data-stu-id="1a65c-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="1a65c-105">Dans cette procédure pas à pas, vous allez générer une plateforme Windows universelle (UWP) qui exécute l’accès aux données de base d’une base de données SQLite locale à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1a65c-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a65c-106">**Évitez les types anonymes dans les requêtes LINQ sur UWP**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="1a65c-107">Pour déployer une application UWP dans le magasin d’applications, votre application doit être générée avec .NET Native.</span><span class="sxs-lookup"><span data-stu-id="1a65c-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="1a65c-108">Les requêtes avec des types anonymes ont des performances inférieures sur .NET Native.</span><span class="sxs-lookup"><span data-stu-id="1a65c-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="1a65c-109">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="1a65c-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a65c-110">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="1a65c-110">Prerequisites</span></span>

<span data-ttu-id="1a65c-111">Les éléments suivants sont nécessaires pour effectuer cette procédure pas à pas :</span><span class="sxs-lookup"><span data-stu-id="1a65c-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="1a65c-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="1a65c-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="1a65c-113">[SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="1a65c-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="1a65c-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 ou ultérieure avec la charge de travail **Développement pour la plateforme Windows universelle**</span><span class="sxs-lookup"><span data-stu-id="1a65c-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="1a65c-115">Créer un projet de modèle</span><span class="sxs-lookup"><span data-stu-id="1a65c-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="1a65c-116">En raison des limitations au niveau de l’interaction des outils .NET Core avec les projets UWP, le modèle doit être placé dans un projet autre que UWP pour pouvoir exécuter des commandes de migration dans la console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="1a65c-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="1a65c-117">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a65c-117">Open Visual Studio</span></span>

* <span data-ttu-id="1a65c-118">Fichier > Nouveau > Projet...</span><span class="sxs-lookup"><span data-stu-id="1a65c-118">File > New > Project...</span></span>

* <span data-ttu-id="1a65c-119">Dans le menu de gauche, sélectionnez Modèles > Visual C#.</span><span class="sxs-lookup"><span data-stu-id="1a65c-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="1a65c-120">Sélectionnez le modèle de projet **Bibliothèque de classes (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="1a65c-121">Nommez le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="1a65c-122">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1a65c-122">Install Entity Framework</span></span>

<span data-ttu-id="1a65c-123">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="1a65c-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="1a65c-124">Cette procédure pas à pas utilise SQLite.</span><span class="sxs-lookup"><span data-stu-id="1a65c-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="1a65c-125">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1a65c-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="1a65c-126">Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="1a65c-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="1a65c-127">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="1a65c-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="1a65c-128">Un peu plus loin dans cette procédure pas à pas, nous utiliserons également des outils Entity Framework pour gérer la base de données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="1a65c-129">Nous installerons donc également le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="1a65c-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="1a65c-130">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="1a65c-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="1a65c-131">Modifiez le fichier .csproj et remplacez `<TargetFramework>netstandard2.0</TargetFramework>` par `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.</span><span class="sxs-lookup"><span data-stu-id="1a65c-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="1a65c-132">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="1a65c-132">Create your model</span></span>

<span data-ttu-id="1a65c-133">Définissons à présent le contexte et les classes d’entité qui composeront le modèle.</span><span class="sxs-lookup"><span data-stu-id="1a65c-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="1a65c-134">Projet > Ajouter une classe...</span><span class="sxs-lookup"><span data-stu-id="1a65c-134">Project > Add Class...</span></span>

* <span data-ttu-id="1a65c-135">Entrez le nom *Model.cs* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="1a65c-136">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="1a65c-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="1a65c-137">Créer un projet UWP</span><span class="sxs-lookup"><span data-stu-id="1a65c-137">Create a new UWP project</span></span>

* <span data-ttu-id="1a65c-138">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a65c-138">Open Visual Studio</span></span>

* <span data-ttu-id="1a65c-139">Fichier > Nouveau > Projet...</span><span class="sxs-lookup"><span data-stu-id="1a65c-139">File > New > Project...</span></span>

* <span data-ttu-id="1a65c-140">Dans le menu de gauche, sélectionnez Modèles > Visual C# > Windows universel.</span><span class="sxs-lookup"><span data-stu-id="1a65c-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="1a65c-141">Sélectionnez le modèle de projet **Application vide (Windows universel)**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="1a65c-142">Nommez le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="1a65c-143">Définissez la cible et les versions minimales sur `Windows 10 Fall Creators Update (10.0; build 16299.0)` au minimum.</span><span class="sxs-lookup"><span data-stu-id="1a65c-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="1a65c-144">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="1a65c-144">Create your database</span></span>

<span data-ttu-id="1a65c-145">Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="1a65c-146">Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="1a65c-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="1a65c-147">Sélectionnez le projet de modèle en tant que projet par défaut et définissez-le comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="1a65c-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="1a65c-148">Exécutez `Add-Migration MyFirstMigration` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="1a65c-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="1a65c-149">Étant donné que nous voulons créer la base de données sur l’appareil sur lequel est exécutée l’application, nous allons ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="1a65c-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="1a65c-150">Au cours de la première exécution de l’application, ce code se chargera de créer la base de données locale pour nous.</span><span class="sxs-lookup"><span data-stu-id="1a65c-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="1a65c-151">Cliquez avec le bouton de droite sur **App.xaml** dans l’**Explorateur de solutions**, puis sélectionnez **Afficher le code**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="1a65c-152">Ajoutez la ligne using mise en surbrillance au début du fichier.</span><span class="sxs-lookup"><span data-stu-id="1a65c-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="1a65c-153">Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.</span><span class="sxs-lookup"><span data-stu-id="1a65c-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="1a65c-154">Si vous apportez des modifications ultérieures au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les modifications correspondantes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="1a65c-155">Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="1a65c-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="1a65c-156">EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="1a65c-157">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="1a65c-157">Use your model</span></span>

<span data-ttu-id="1a65c-158">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="1a65c-159">Ouvrez *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="1a65c-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="1a65c-160">Ajoutez le gestionnaire de chargement de page et le contenu de l’interface utilisateur mis en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1a65c-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="1a65c-161">Nous allons maintenant ajouter du code pour connecter l’interface utilisateur à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1a65c-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="1a65c-162">Cliquez avec le bouton de droite sur **MainPage.xaml** dans l’**Explorateur de solutions**, puis sélectionnez **Afficher le code**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="1a65c-163">Ajoutez le code mis en surbrillance de la liste suivante.</span><span class="sxs-lookup"><span data-stu-id="1a65c-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="1a65c-164">Vous pouvez à présent exécuter l’application pour la voir en action.</span><span class="sxs-lookup"><span data-stu-id="1a65c-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="1a65c-165">Déboguer > Exécuter sans débogage</span><span class="sxs-lookup"><span data-stu-id="1a65c-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="1a65c-166">L’application est générée et lancée.</span><span class="sxs-lookup"><span data-stu-id="1a65c-166">The application will build and launch</span></span>

* <span data-ttu-id="1a65c-167">Entrez une URL et cliquez sur le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="1a65c-167">Enter a URL and click the **Add** button</span></span>

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="1a65c-170">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="1a65c-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="1a65c-171">La performance `SaveChanges()` peut être améliorée en implémentant [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx) et [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) dans vos types d’entité et en utilisant `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="1a65c-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="1a65c-172">Et le tour est joué !</span><span class="sxs-lookup"><span data-stu-id="1a65c-172">Tada!</span></span> <span data-ttu-id="1a65c-173">Vous avez désormais une application UWP simple exécutant Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1a65c-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="1a65c-174">Consultez les autres articles de cette documentation pour en savoir plus sur les fonctionnalités d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1a65c-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
