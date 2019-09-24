---
title: Inclusion de & à l’exception des types-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197375"
---
# <a name="including--excluding-types"></a><span data-ttu-id="a3b7e-102">Inclusion de & à l’exception des types</span><span class="sxs-lookup"><span data-stu-id="a3b7e-102">Including & Excluding Types</span></span>

<span data-ttu-id="a3b7e-103">L’inclusion d’un type dans le modèle signifie qu’EF a des métadonnées sur ce type et tente de lire et d’écrire des instances à partir de/vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="a3b7e-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="a3b7e-104">Conventions</span></span>

<span data-ttu-id="a3b7e-105">Par Convention, les types qui sont exposés `DbSet` dans les propriétés de votre contexte sont inclus dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="a3b7e-106">En outre, les types qui sont mentionnés dans `OnModelCreating` la méthode sont également inclus.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="a3b7e-107">Enfin, tous les types qui sont trouvés en explorant de manière récursive les propriétés de navigation des types découverts sont également inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="a3b7e-108">**Par exemple, dans le code suivant, les trois types sont découverts :**</span><span class="sxs-lookup"><span data-stu-id="a3b7e-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="a3b7e-109">`Blog`parce qu’il est exposé dans `DbSet` une propriété sur le contexte</span><span class="sxs-lookup"><span data-stu-id="a3b7e-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="a3b7e-110">`Post`parce qu’il est découvert via `Blog.Posts` la propriété de navigation</span><span class="sxs-lookup"><span data-stu-id="a3b7e-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="a3b7e-111">`AuditEntry`parce qu’il est mentionné dans`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="a3b7e-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="a3b7e-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a3b7e-112">Data Annotations</span></span>

<span data-ttu-id="a3b7e-113">Vous pouvez utiliser des annotations de données pour exclure un type du modèle.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="a3b7e-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a3b7e-114">Fluent API</span></span>

<span data-ttu-id="a3b7e-115">Vous pouvez utiliser l’API Fluent pour exclure un type du modèle.</span><span class="sxs-lookup"><span data-stu-id="a3b7e-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
