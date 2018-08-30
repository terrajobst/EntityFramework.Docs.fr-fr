---
title: 'Requêtes de base : EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: eceac81546b23157611edd530b8b71f71e970c1f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447747"
---
# <a name="basic-queries"></a><span data-ttu-id="a6cdd-102">Requêtes de base</span><span class="sxs-lookup"><span data-stu-id="a6cdd-102">Basic Queries</span></span>

<span data-ttu-id="a6cdd-103">Découvrez comment charger des entités à partir de la base de données avec Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="a6cdd-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="a6cdd-104">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6cdd-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="a6cdd-105">101 exemples LINQ</span><span class="sxs-lookup"><span data-stu-id="a6cdd-105">101 LINQ samples</span></span>

<span data-ttu-id="a6cdd-106">Cette page présente quelques exemples pour accomplir des tâches courantes avec Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a6cdd-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="a6cdd-107">Pour un ensemble d’exemples illustrant ce qui est possible de faire avec LINQ, consultez [101 exemples LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="a6cdd-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="a6cdd-108">Chargement de toutes les données</span><span class="sxs-lookup"><span data-stu-id="a6cdd-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="a6cdd-109">Chargement d’une seule entité</span><span class="sxs-lookup"><span data-stu-id="a6cdd-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="a6cdd-110">Filtrage</span><span class="sxs-lookup"><span data-stu-id="a6cdd-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
