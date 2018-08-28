---
title: Index - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995478"
---
# <a name="indexes"></a>Index

Les index sont un concept commun entre plusieurs banques de données. Bien que leur implémentation dans le magasin de données peut varier, ils sont utilisés pour faire des recherches basées sur une colonne (ou un ensemble de colonnes) plus efficace.

## <a name="conventions"></a>Conventions

Par convention, un index est créé dans chaque propriété (ou un ensemble de propriétés) qui sont utilisés comme une clé étrangère.

## <a name="data-annotations"></a>Annotations de données

Index ne peuvent pas être créés à l’aide des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier un index sur une seule propriété. Par défaut, les index ne sont pas uniques.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Vous pouvez également spécifier qu’un index doit être unique, ce qui signifie qu’aucun deux entités ne peuvent avoir la même valeur (s) pour les propriétés données.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Vous pouvez également spécifier un index sur plusieurs colonnes.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Il existe un seul index par un ensemble distinct de propriétés. Si vous utilisez l’API Fluent pour configurer un index sur un ensemble de propriétés qui possède déjà un index défini, soit par convention ou la configuration précédente, puis vous allez modifier la définition de cet index. Cela est utile si vous souhaitez continuer à configurer un index qui a été créé par convention.
