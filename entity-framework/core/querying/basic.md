---
title: 'Requêtes de base : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 6a381f419cb0958ea0835070e22fe7a3212457d7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993703"
---
# <a name="basic-queries"></a>Requêtes de base

Découvrez comment charger des entités à partir de la base de données avec Language Integrated Query (LINQ).

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="101-linq-samples"></a>101 exemples LINQ

Cette page présente quelques exemples pour accomplir des tâches courantes avec Entity Framework Core. Pour un ensemble d’exemples illustrant ce qui est possible de faire avec LINQ, consultez [101 exemples LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Chargement de toutes les données

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Chargement d’une seule entité

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Filtrage

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
