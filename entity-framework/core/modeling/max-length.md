---
title: Longueur maximale - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996189"
---
# <a name="maximum-length"></a>Longueur maximale

Configuration d’une longueur maximale de fournit une indication au magasin de données sur le type de données approprié à utiliser pour une propriété donnée. Longueur maximale s’applique uniquement aux types de données de tableau, tel que `string` et `byte[]`.

> [!NOTE]  
> Entity Framework n’effectue aucune validation de longueur maximale avant de transmettre des données au fournisseur. C’est à la banque de fournisseur ou de données pour valider le cas échéant. Par exemple, lorsque SQL Server, qui dépasse la longueur maximale de ciblage entraîne une exception comme type de données de la colonne sous-jacente n’autorise pas les données excédentaires à stocker.

## <a name="conventions"></a>Conventions

Par convention, il est de laisser le fournisseur de base de données peut choisir un type de données approprié pour les propriétés. Pour les propriétés qui ont une longueur, le fournisseur de base de données choisit généralement un type de données qui autorise la longueur la plus longue des données. Par exemple, Microsoft SQL Server utilisera `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` si la colonne est utilisée en tant que clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les Annotations de données pour configurer une longueur maximale pour une propriété. Dans cet exemple, ciblant SQL Server cela entraînerait la `nvarchar(500)` type de données utilisé.

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

Vous pouvez utiliser l’API Fluent pour configurer une longueur maximale pour une propriété. Dans cet exemple, ciblant SQL Server cela entraînerait la `nvarchar(500)` type de données utilisé.

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
