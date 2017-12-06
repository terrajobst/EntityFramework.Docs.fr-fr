---
title: Longueur maximale - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a>Longueur maximale

Configuration d’une longueur maximale fournit une indication pour le magasin de données sur le type de données approprié à utiliser pour une propriété donnée. La longueur maximale s’applique uniquement aux types de données de tableau, tel que `string` et `byte[]`.

> [!NOTE]  
> Entity Framework n’effectue aucune validation de longueur maximale avant de passer des données au fournisseur. C’est à la banque de données ou le fournisseur pour valider le cas échéant. Par exemple, SQL Server, qui dépasse la longueur maximale de ciblage entraîne une exception en tant que type de données de la colonne sous-jacente n’autorise pas les données excédentaires à stocker.

## <a name="conventions"></a>Conventions

Par convention, il incombe au fournisseur de base de données de choisir un type de données approprié pour les propriétés. Pour les propriétés qui ont une longueur, le fournisseur de base de données sera choisir généralement d’un type de données qui permet la longueur de la plus longue des données. Par exemple, Microsoft SQL Server utilisera `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` si la colonne est utilisée en tant que clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les Annotations de données pour configurer une longueur maximale pour une propriété. Dans cet exemple, ciblant SQL Server, cela entraînerait le `nvarchar(500)` type de données qui est utilisé.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une longueur maximale pour une propriété. Dans cet exemple, ciblant SQL Server, cela entraînerait le `nvarchar(500)` type de données qui est utilisé.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
