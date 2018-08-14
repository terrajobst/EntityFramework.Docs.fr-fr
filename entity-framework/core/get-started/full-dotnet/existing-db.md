---
title: Bien démarrer avec .NET Framework - Base de données existante - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614413"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="018a8-102">Bien démarrer avec EF Core sur .NET Framework avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="018a8-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="018a8-103">Dans ce tutoriel, vous générez une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="018a8-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="018a8-104">Vous créez un modèle Entity Framework en rétroconcevant une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="018a8-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="018a8-105">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="018a8-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="018a8-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="018a8-106">Prerequisites</span></span>

* [<span data-ttu-id="018a8-107">Visual Studio 2017 version 15.7 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="018a8-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="018a8-108">Créer une base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="018a8-108">Create Blogging database</span></span>

<span data-ttu-id="018a8-109">Ce tutoriel utilise une base de données de **création de blogs** sur l’instance LocalDb comme base de données existante.</span><span class="sxs-lookup"><span data-stu-id="018a8-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="018a8-110">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.</span><span class="sxs-lookup"><span data-stu-id="018a8-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="018a8-111">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="018a8-111">Open Visual Studio</span></span>

* <span data-ttu-id="018a8-112">**Outils > Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="018a8-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="018a8-113">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="018a8-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="018a8-114">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="018a8-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="018a8-115">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="018a8-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="018a8-116">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="018a8-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="018a8-117">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="018a8-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="018a8-118">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="018a8-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="018a8-119">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="018a8-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="018a8-120">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="018a8-120">Create a new project</span></span>

* <span data-ttu-id="018a8-121">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="018a8-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="018a8-122">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="018a8-122">**File > New > Project...**</span></span>

* <span data-ttu-id="018a8-123">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows Desktop**.</span><span class="sxs-lookup"><span data-stu-id="018a8-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="018a8-124">Sélectionnez le modèle de projet **Application console (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="018a8-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="018a8-125">Vérifiez que le projet cible le **.NET Framework 4.6.1** ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="018a8-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="018a8-126">Nommez le projet *ConsoleApp.ExistingDb* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="018a8-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="018a8-127">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="018a8-127">Install Entity Framework</span></span>

<span data-ttu-id="018a8-128">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="018a8-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="018a8-129">Ce tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="018a8-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="018a8-130">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="018a8-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="018a8-131">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="018a8-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="018a8-132">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="018a8-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="018a8-133">Dans l’étape suivante, vous utilisez des outils Entity Framework pour rétroconcevoir la base de données.</span><span class="sxs-lookup"><span data-stu-id="018a8-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="018a8-134">Installez donc le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="018a8-134">So install the tools package as well.</span></span>

* <span data-ttu-id="018a8-135">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="018a8-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="018a8-136">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="018a8-136">Reverse engineer the model</span></span>

<span data-ttu-id="018a8-137">Créons à présent le modèle EF à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="018a8-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="018a8-138">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="018a8-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="018a8-139">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="018a8-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="018a8-140">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="018a8-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="018a8-141">Par exemple, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="018a8-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="018a8-142">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog` et `Post`) et un contexte dérivé (`BloggingContext`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="018a8-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="018a8-143">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="018a8-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="018a8-144">Voici les classes d’entité `Blog` et `Post` :</span><span class="sxs-lookup"><span data-stu-id="018a8-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="018a8-145">Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="018a8-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="018a8-146">Le contexte représente une session avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="018a8-146">The context represents a session with the database.</span></span> <span data-ttu-id="018a8-147">Il contient des méthodes que vous pouvez utiliser pour interroger et enregistrer des instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="018a8-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="018a8-148">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="018a8-148">Use the model</span></span>

<span data-ttu-id="018a8-149">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="018a8-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="018a8-150">Ouvrez *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="018a8-150">Open *Program.cs*</span></span>

* <span data-ttu-id="018a8-151">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="018a8-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="018a8-152">Déboguer > Exécuter sans débogage</span><span class="sxs-lookup"><span data-stu-id="018a8-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="018a8-153">Vous voyez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="018a8-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![image](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="018a8-155">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="018a8-155">Additional Resources</span></span>

* [<span data-ttu-id="018a8-156">EF Core sur le .NET Framework avec une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="018a8-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="018a8-157">[EF Core sur .NET Core avec une nouvelle base de données - SQLite](xref:core/get-started/netcore/new-db-sqlite) : tutoriel sur la console multiplateforme EF.</span><span class="sxs-lookup"><span data-stu-id="018a8-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
