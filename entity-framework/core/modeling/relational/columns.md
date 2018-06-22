---
title: Mappage de colonne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052899"
---
# <a name="column-mapping"></a>Mappage de colonnes

> [!NOTE]  
> La configuration de cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).

Mappage de colonne identifie les données de la colonne doivent être interrogées à partir d’et enregistrées dans la base de données.

## <a name="conventions"></a>Conventions

Par convention, chaque propriété sera le programme d’installation pour mapper à une colonne portant le même nom que la propriété.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer la colonne à laquelle une propriété est mappée.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
