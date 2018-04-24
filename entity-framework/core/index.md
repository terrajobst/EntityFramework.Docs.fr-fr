---
title: Vue d’ensemble - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: f9aac91545b97e56686e3a8d2eb9e83c849587d9
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="e39c1-102">Vue d’ensemble d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e39c1-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="e39c1-103">Entity Framework (EF) Core est une version légère, extensible et multiplateforme de la technologie d’accès aux données Entity Framework populaire.</span><span class="sxs-lookup"><span data-stu-id="e39c1-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="e39c1-104">EF Core peut servir de mappeur relationnel/objet (O/RM), permettant aux développeurs .NET de travailler avec une base de données à l’aide d’objets .NET, et éliminant la nécessité de la plupart du code d’accès aux données qu’ils doivent généralement écrire.</span><span class="sxs-lookup"><span data-stu-id="e39c1-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span> 

<span data-ttu-id="e39c1-105">EF Core prend en charge de nombreux moteurs de base de données ; consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e39c1-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="e39c1-106">Si vous voulez approfondir vos connaissances en écrivant du code, nous vous recommandons un de nos guides de [démarrage](get-started/index.md) pour commencer avec EF Core.</span><span class="sxs-lookup"><span data-stu-id="e39c1-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="e39c1-107">Nouveautés d’EF Core</span><span class="sxs-lookup"><span data-stu-id="e39c1-107">What is new in EF Core</span></span>

<span data-ttu-id="e39c1-108">Si vous connaissez déjà EF Core et souhaitez vous plonger directement dans les détails des dernières versions :</span><span class="sxs-lookup"><span data-stu-id="e39c1-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="e39c1-109">**[Nouveautés d’EF Core 2.1 (actuellement en préversion)](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="e39c1-109">**[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="e39c1-110">**[Nouveautés d’EF Core 2.0 (dernière version publiée)](xref:core/what-is-new/ef-core-2.0)**</span><span class="sxs-lookup"><span data-stu-id="e39c1-110">**[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**</span></span>
- <span data-ttu-id="e39c1-111">**[Mise à niveau d’applications existantes vers EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="e39c1-111">**[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="e39c1-112">Obtenir Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e39c1-112">Get Entity Framework Core</span></span>

<span data-ttu-id="e39c1-113">[Installez le package NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) pour le fournisseur de base de données que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="e39c1-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="e39c1-114">Par exemple,</span><span class="sxs-lookup"><span data-stu-id="e39c1-114">E.g.</span></span> <span data-ttu-id="e39c1-115">pour installer le fournisseur SQL Server dans un développement multiplateforme à l’aide de l’outil `dotnet` depuis la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="e39c1-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="e39c1-116">Ou, dans Visual Studio, à l’aide de la console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="e39c1-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="e39c1-117">Consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations sur les fournisseurs disponibles et [Installation d’EF Core](get-started/install/index.md) pour découvrir la procédure d’installation détaillée.</span><span class="sxs-lookup"><span data-stu-id="e39c1-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="e39c1-118">Modèle</span><span class="sxs-lookup"><span data-stu-id="e39c1-118">The Model</span></span>

<span data-ttu-id="e39c1-119">Avec EF Core, l’accès aux données est effectué à l’aide d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="e39c1-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="e39c1-120">Un modèle est composé de classes d’entité et d’un contexte dérivé qui représente une session avec la base de données, ce qui vous permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="e39c1-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="e39c1-121">Pour en savoir plus, consultez [Création d’un modèle](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="e39c1-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="e39c1-122">Vous pouvez générer un modèle à partir d’une base de données existante, coder manuellement un modèle en fonction de votre base de données ou utiliser EF Migrations pour créer une base de données à partir de votre modèle (et la faire évoluer au même rythme que celui-ci).</span><span class="sxs-lookup"><span data-stu-id="e39c1-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a><span data-ttu-id="e39c1-123">Interrogation</span><span class="sxs-lookup"><span data-stu-id="e39c1-123">Querying</span></span>

<span data-ttu-id="e39c1-124">Les instances de vos classes d’entité sont récupérées de la base de données à l’aide de LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="e39c1-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="e39c1-125">Pour en savoir plus, consultez [Interrogation des données](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="e39c1-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="e39c1-126">Enregistrement de données</span><span class="sxs-lookup"><span data-stu-id="e39c1-126">Saving Data</span></span>

<span data-ttu-id="e39c1-127">Les données sont créées, supprimées et modifiées dans la base de données à l’aide d’instances de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="e39c1-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="e39c1-128">Pour en savoir plus, consultez [Enregistrement de données](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="e39c1-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
