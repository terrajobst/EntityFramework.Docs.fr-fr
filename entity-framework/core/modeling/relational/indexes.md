---
title: Index - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="5f055-102">Index</span><span class="sxs-lookup"><span data-stu-id="5f055-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="5f055-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="5f055-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5f055-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="5f055-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5f055-105">Un index dans une base de données relationnelle est mappé au même concept en tant qu’index dans le cœur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f055-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="5f055-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="5f055-106">Conventions</span></span>

<span data-ttu-id="5f055-107">Par convention, les index sont nommés `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="5f055-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="5f055-108">Index composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.</span><span class="sxs-lookup"><span data-stu-id="5f055-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5f055-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="5f055-109">Data Annotations</span></span>

<span data-ttu-id="5f055-110">Les index ne peuvent pas être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="5f055-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5f055-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="5f055-111">Fluent API</span></span>

<span data-ttu-id="5f055-112">Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.</span><span class="sxs-lookup"><span data-stu-id="5f055-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
