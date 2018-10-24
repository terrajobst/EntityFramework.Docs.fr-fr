---
title: Bien démarrer avec .NET Framework - Base de données existante - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: b9e079f88dd35016407b19bb627f8bd46edb3d4c
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447155"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="ed3ce-102">Bien démarrer avec EF Core sur .NET Framework avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="ed3ce-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="ed3ce-103">Dans ce tutoriel, vous générez une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="ed3ce-104">Vous créez un modèle Entity Framework en rétroconcevant une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="ed3ce-105">[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="ed3ce-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed3ce-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ed3ce-106">Prerequisites</span></span>

* [<span data-ttu-id="ed3ce-107">Visual Studio 2017 version 15.7 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="ed3ce-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="ed3ce-108">Créer une base de données de création de blogs</span><span class="sxs-lookup"><span data-stu-id="ed3ce-108">Create Blogging database</span></span>

<span data-ttu-id="ed3ce-109">Ce tutoriel utilise une base de données de **création de blogs** sur l’instance LocalDb comme base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="ed3ce-110">Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="ed3ce-111">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed3ce-111">Open Visual Studio</span></span>

* <span data-ttu-id="ed3ce-112">**Outils > Connexion à une base de données...**</span><span class="sxs-lookup"><span data-stu-id="ed3ce-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="ed3ce-113">Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="ed3ce-114">Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="ed3ce-115">Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="ed3ce-116">La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="ed3ce-117">Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="ed3ce-118">Copiez le script ci-dessous dans l’éditeur de requêtes.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="ed3ce-119">Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="ed3ce-120">Créer un projet</span><span class="sxs-lookup"><span data-stu-id="ed3ce-120">Create a new project</span></span>

* <span data-ttu-id="ed3ce-121">Ouvrez Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="ed3ce-122">**Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="ed3ce-122">**File > New > Project...**</span></span>

* <span data-ttu-id="ed3ce-123">Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows Desktop**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="ed3ce-124">Sélectionnez le modèle de projet **Application console (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="ed3ce-125">Vérifiez que le projet cible le **.NET Framework 4.6.1** ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="ed3ce-126">Nommez le projet *ConsoleApp.ExistingDb* et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ed3ce-127">Installer Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ed3ce-127">Install Entity Framework</span></span>

<span data-ttu-id="ed3ce-128">Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ed3ce-129">Ce tutoriel utilise SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="ed3ce-130">Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ed3ce-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ed3ce-131">**Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="ed3ce-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ed3ce-132">Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="ed3ce-133">Dans l’étape suivante, vous utilisez des outils Entity Framework pour rétroconcevoir la base de données.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="ed3ce-134">Installez donc le package d’outils.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-134">So install the tools package as well.</span></span>

* <span data-ttu-id="ed3ce-135">Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="ed3ce-136">Rétroconcevoir le modèle</span><span class="sxs-lookup"><span data-stu-id="ed3ce-136">Reverse engineer the model</span></span>

<span data-ttu-id="ed3ce-137">Créons à présent le modèle EF à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="ed3ce-138">**Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="ed3ce-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="ed3ce-139">Exécutez la commande suivante pour créer un modèle à partir de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="ed3ce-140">Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="ed3ce-141">Par exemple, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="ed3ce-142">Le processus d’ingénierie à rebours créé des classes d’entité (`Blog` et `Post`) et un contexte dérivé (`BloggingContext`) en fonction du schéma de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="ed3ce-143">Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="ed3ce-144">Voici les classes d’entité `Blog` et `Post` :</span><span class="sxs-lookup"><span data-stu-id="ed3ce-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="ed3ce-145">Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="ed3ce-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="ed3ce-146">Le contexte représente une session avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-146">The context represents a session with the database.</span></span> <span data-ttu-id="ed3ce-147">Il contient des méthodes que vous pouvez utiliser pour interroger et enregistrer des instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="ed3ce-148">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="ed3ce-148">Use the model</span></span>

<span data-ttu-id="ed3ce-149">Vous pouvez à présent utiliser le modèle pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="ed3ce-150">Ouvrez *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-150">Open *Program.cs*</span></span>

* <span data-ttu-id="ed3ce-151">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="ed3ce-152">Déboguer > Exécuter sans débogage</span><span class="sxs-lookup"><span data-stu-id="ed3ce-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="ed3ce-153">Vous voyez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.</span><span class="sxs-lookup"><span data-stu-id="ed3ce-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![image](_static/output-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="ed3ce-155">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ed3ce-155">Next steps</span></span>

<span data-ttu-id="ed3ce-156">Pour plus d’informations sur la façon de structurer un contexte et des classes d’entité, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="ed3ce-156">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="ed3ce-157">Référence des outils Entity Framework Core - .NET CLI</span><span class="sxs-lookup"><span data-stu-id="ed3ce-157">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="ed3ce-158">Référence des outils Entity Framework Core - Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="ed3ce-158">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)