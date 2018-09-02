---
title: Bien démarrer avec .NET Framework - Nouvelle base de données - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997585"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="b3215-102">Bien démarrer avec EF Core sur .NET Framework avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="b3215-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="b3215-103">Dans ce tutoriel, vous générez une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3215-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="b3215-104">Vous utilisez des migrations pour créer la base de données à partir d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="b3215-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="b3215-105">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="b3215-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3215-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b3215-106">Prerequisites</span></span>

* [<span data-ttu-id="b3215-107">Visual Studio 2017 version 15.7 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="b3215-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="b3215-108">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="b3215-108">Create a new project</span></span>

* <span data-ttu-id="b3215-109">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b3215-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="b3215-110">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="b3215-110">**File > New > Project...**</span></span>

* <span data-ttu-id="b3215-111">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows Desktop**.</span><span class="sxs-lookup"><span data-stu-id="b3215-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="b3215-112">Sélectionnez le modèle de projet **Application console (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b3215-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="b3215-113">Vérifiez que le projet cible le **.NET Framework 4.6.1** ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="b3215-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="b3215-114">Nommez le projet *ConsoleApp.NewDb* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3215-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="b3215-115">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b3215-115">Install Entity Framework</span></span>

<span data-ttu-id="b3215-116">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="b3215-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="b3215-117">Ce tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3215-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="b3215-118">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b3215-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="b3215-119">Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="b3215-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="b3215-120">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="b3215-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="b3215-121">Un peu plus loin dans ce tutoriel, vous utiliserez certains outils Entity Framework pour gérer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="b3215-122">Installez donc le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="b3215-122">So install the tools package as well.</span></span>

* <span data-ttu-id="b3215-123">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="b3215-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="b3215-124">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="b3215-124">Create the model</span></span>

<span data-ttu-id="b3215-125">Définissons à présent le contexte et les classes d’entité qui composent le modèle.</span><span class="sxs-lookup"><span data-stu-id="b3215-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="b3215-126">**Projet > Ajouter une classe...**</span><span class="sxs-lookup"><span data-stu-id="b3215-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="b3215-127">Entrez le nom *Model.cs* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3215-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="b3215-128">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b3215-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="b3215-129">Dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration ou une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="b3215-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="b3215-130">Par souci de simplicité, ce tutoriel place le tout dans un même fichier de code.</span><span class="sxs-lookup"><span data-stu-id="b3215-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="b3215-131">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="b3215-131">Create the database</span></span>

<span data-ttu-id="b3215-132">Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="b3215-133">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="b3215-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="b3215-134">Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration afin de créer le jeu initial de tables du modèle.</span><span class="sxs-lookup"><span data-stu-id="b3215-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="b3215-135">Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="b3215-136">Étant donné que votre base de données n’existe pas encore, elle sera créée avant l’application de la migration.</span><span class="sxs-lookup"><span data-stu-id="b3215-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="b3215-137">Si vous apportez des changements au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les changements de schéma correspondants à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="b3215-138">Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `Update-Database` pour appliquer les modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="b3215-139">EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3215-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="b3215-140">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="b3215-140">Use the model</span></span>

<span data-ttu-id="b3215-141">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="b3215-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="b3215-142">Ouvrez *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="b3215-142">Open *Program.cs*</span></span>

* <span data-ttu-id="b3215-143">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b3215-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="b3215-144">**Déboguer > Démarrer sans débogage**</span><span class="sxs-lookup"><span data-stu-id="b3215-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="b3215-145">Vous voyez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="b3215-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![image](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="b3215-147">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b3215-147">Additional Resources</span></span>

* [<span data-ttu-id="b3215-148">EF Core sur .NET Framework avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="b3215-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="b3215-149">[EF Core sur .NET Core avec une nouvelle base de données - SQLite](xref:core/get-started/netcore/new-db-sqlite) : tutoriel sur la console multiplateforme EF.</span><span class="sxs-lookup"><span data-stu-id="b3215-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
