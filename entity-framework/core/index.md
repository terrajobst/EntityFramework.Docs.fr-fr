---
title: Vue d'ensemble d'Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 0107a520e5a698eaf76426b63c6f784392559167
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196973"
---
# <a name="entity-framework-core"></a><span data-ttu-id="fb9a1-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fb9a1-102">Entity Framework Core</span></span>

<span data-ttu-id="fb9a1-103">Entity Framework (EF) Core est une version légère, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) et multiplateforme de la très connue technologie d’accès aux données Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="fb9a1-104">EF Core peut servir de mappeur relationnel/objet (O/RM), permettant aux développeurs .NET de travailler avec une base de données à l’aide d’objets .NET, et éliminant la nécessité de la plupart du code d’accès aux données qu’ils doivent généralement écrire.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="fb9a1-105">EF Core prend en charge de nombreux moteurs de base de données ; consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="fb9a1-106">Modèle</span><span class="sxs-lookup"><span data-stu-id="fb9a1-106">The Model</span></span>

<span data-ttu-id="fb9a1-107">Avec EF Core, l’accès aux données est effectué à l’aide d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="fb9a1-108">Un modèle est composé de classes d’entités et d’un objet de contexte représentant une session avec la base de données, ce qui permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="fb9a1-109">Pour en savoir plus, consultez [Création d’un modèle](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="fb9a1-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="fb9a1-110">Vous pouvez générer un modèle à partir d’une base de données existante, coder manuellement un modèle en fonction de votre base de données ou utiliser [EF Migrations](managing-schemas/migrations/index.md) pour créer une base de données à partir de votre modèle et la faire évoluer au même rythme que celui-ci.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="querying"></a><span data-ttu-id="fb9a1-111">Interrogation</span><span class="sxs-lookup"><span data-stu-id="fb9a1-111">Querying</span></span>

<span data-ttu-id="fb9a1-112">Les instances de vos classes d’entité sont récupérées de la base de données à l’aide de LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="fb9a1-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="fb9a1-113">Pour en savoir plus, consultez [Interrogation des données](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="fb9a1-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="fb9a1-114">Enregistrement de données</span><span class="sxs-lookup"><span data-stu-id="fb9a1-114">Saving Data</span></span>

<span data-ttu-id="fb9a1-115">Les données sont créées, supprimées et modifiées dans la base de données à l’aide d’instances de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="fb9a1-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="fb9a1-116">Pour en savoir plus, consultez [Enregistrement de données](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="fb9a1-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="fb9a1-117">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fb9a1-117">Next steps</span></span>

<span data-ttu-id="fb9a1-118">Pour des tutoriels d’introduction, consultez [Bien démarrer avec Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="fb9a1-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

