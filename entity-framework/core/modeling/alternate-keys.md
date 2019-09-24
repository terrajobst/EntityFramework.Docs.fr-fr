---
title: Autres clés-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197464"
---
# <a name="alternate-keys"></a>Clés secondaires

Une autre clé sert d’identificateur unique alternatif pour chaque instance d’entité en plus de la clé primaire. D’autres clés peuvent être utilisées comme cible d’une relation. Lors de l’utilisation d’une base de données relationnelle, cela correspond au concept d’index/de contrainte unique sur la ou les colonnes clés de remplacement et une ou plusieurs contraintes de clé étrangère qui référencent la ou les colonnes.

> [!TIP]  
> Si vous souhaitez simplement garantir l’unicité d’une colonne, vous devez disposer d’un index unique plutôt que d’une clé secondaire, consultez [index](indexes.md). Dans EF, les clés alternatives offrent des fonctionnalités supérieures à celles des index uniques, car elles peuvent être utilisées comme cible d’une clé étrangère.

Des clés alternatives sont généralement introduites pour vous si nécessaire et vous n’avez pas besoin de les configurer manuellement. Pour plus d’informations, consultez [conventions](#conventions) .

## <a name="conventions"></a>Conventions

Par Convention, une clé secondaire est introduite pour vous lorsque vous identifiez une propriété, qui n’est pas la clé primaire, comme la cible d’une relation.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

Les clés secondaires ne peuvent pas être configurées à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une seule propriété pour qu’elle soit une clé secondaire.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé secondaire (appelée clé de remplacement composite).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
