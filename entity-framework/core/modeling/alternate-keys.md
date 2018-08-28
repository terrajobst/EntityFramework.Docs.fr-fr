---
title: Clés secondaires - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996969"
---
# <a name="alternate-keys"></a>Clés secondaires

Une autre clé sert d’un autre identificateur unique pour chaque instance d’entité en plus de la clé primaire. Clés secondaires peuvent être utilisés comme cible d’une relation. Lorsque vous utilisez une base de données relationnelle correspond au concept d’une contrainte/index unique sur les colonnes de clé secondaires et un ou plusieurs contraintes de clé étrangère qui font référence à l’ou les colonnes.

> [!TIP]  
> Si vous souhaitez simplement l’unicité d’une colonne vous ensuite un index unique plutôt qu’une autre clé, consultez [index](indexes.md). Dans EF, les clés secondaires fournissent davantage de fonctionnalités que les index uniques car ils peuvent être utilisés comme cible d’une clé étrangère.

Clés secondaires sont généralement introduites pour vous si nécessaire et vous n’avez pas besoin de les configurer manuellement. Consultez [Conventions](#conventions) pour plus d’informations.

## <a name="conventions"></a>Conventions

Par convention, une autre clé est introduite pour vous lorsque vous identifiez une propriété qui n’est pas la clé primaire, comme la cible d’une relation.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Annotations de données

Clés secondaires ne peuvent pas être configurés à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété unique pour être une autre clé.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés d’une autre clé (comme une autre clé composite).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
