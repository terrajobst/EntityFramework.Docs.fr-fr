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
# <a name="including--excluding-types"></a>Inclusion de & à l’exception des types

L’inclusion d’un type dans le modèle signifie qu’EF a des métadonnées sur ce type et tente de lire et d’écrire des instances à partir de/vers la base de données.

## <a name="conventions"></a>Conventions

Par Convention, les types qui sont exposés `DbSet` dans les propriétés de votre contexte sont inclus dans votre modèle. En outre, les types qui sont mentionnés dans `OnModelCreating` la méthode sont également inclus. Enfin, tous les types qui sont trouvés en explorant de manière récursive les propriétés de navigation des types découverts sont également inclus dans le modèle.

**Par exemple, dans le code suivant, les trois types sont découverts :**

* `Blog`parce qu’il est exposé dans `DbSet` une propriété sur le contexte

* `Post`parce qu’il est découvert via `Blog.Posts` la propriété de navigation

* `AuditEntry`parce qu’il est mentionné dans`OnModelCreating`

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

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour exclure un type du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour exclure un type du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
