---
title: Mappage de table - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994135"
---
# <a name="table-mapping"></a>Mappage de table

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Mappage de table identifie les données de table doivent être interrogées à partir d’et enregistrées dans la base de données.

## <a name="conventions"></a>Conventions

Par convention, chaque entité sera être configurée pour mapper à une table portant le même nom que le `DbSet<TEntity>` propriété qui expose l’entité sur le contexte dérivé. Si aucun `DbSet<TEntity>` est inclus pour l’entité donnée, le nom de classe est utilisé.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer la table qui a un type est mappé au.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Vous pouvez également spécifier un schéma auquel appartient la table.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer la table qui a un type est mappé au.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Vous pouvez également spécifier un schéma auquel appartient la table.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
