---
title: Mappage de table-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656103"
---
# <a name="table-mapping"></a>Mappage de tables

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le mappage de table identifie les données de table qui doivent être interrogées et enregistrées dans la base de données.

## <a name="conventions"></a>Conventions

Par convention, chaque entité est configurée pour être mappée à une table avec le même nom que la propriété `DbSet<TEntity>` qui expose l’entité sur le contexte dérivé. Si aucun `DbSet<TEntity>` n’est inclus pour l’entité donnée, le nom de la classe est utilisé.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour configurer la table à laquelle un type est mappé.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

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

Vous pouvez utiliser l’API Fluent pour configurer la table à laquelle un type est mappé.

``` csharp
using Microsoft.EntityFrameworkCore;

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

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
