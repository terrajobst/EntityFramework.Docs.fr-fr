---
title: Index-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197246"
---
# <a name="indexes"></a><span data-ttu-id="a1d9f-102">Index</span><span class="sxs-lookup"><span data-stu-id="a1d9f-102">Indexes</span></span>

<span data-ttu-id="a1d9f-103">Les index sont un concept commun entre de nombreux magasins de données.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="a1d9f-104">Bien que leur implémentation dans le magasin de données puisse varier, elles sont utilisées pour rendre les recherches basées sur une colonne (ou un ensemble de colonnes) plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="a1d9f-105">Conventions</span><span class="sxs-lookup"><span data-stu-id="a1d9f-105">Conventions</span></span>

<span data-ttu-id="a1d9f-106">Par Convention, un index est créé dans chaque propriété (ou ensemble de propriétés) utilisée comme clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a1d9f-107">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a1d9f-107">Data Annotations</span></span>

<span data-ttu-id="a1d9f-108">Les index ne peuvent pas être créés à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a1d9f-109">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a1d9f-109">Fluent API</span></span>

<span data-ttu-id="a1d9f-110">Vous pouvez utiliser l’API Fluent pour spécifier un index sur une seule propriété.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="a1d9f-111">Par défaut, les index sont non uniques.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="a1d9f-112">Vous pouvez également spécifier qu’un index doit être unique, ce qui signifie que deux entités ne peuvent pas avoir la même valeur pour la ou les propriétés spécifiées.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="a1d9f-113">Vous pouvez également spécifier un index sur plusieurs colonnes.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="a1d9f-114">Il n’existe qu’un seul index par jeu de propriétés distinct.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="a1d9f-115">Si vous utilisez l’API Fluent pour configurer un index sur un ensemble de propriétés pour lequel un index est déjà défini, soit par Convention, soit par configuration précédente, vous allez modifier la définition de cet index.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="a1d9f-116">Cela est utile si vous souhaitez configurer davantage un index qui a été créé par Convention.</span><span class="sxs-lookup"><span data-stu-id="a1d9f-116">This is useful if you want to further configure an index that was created by convention.</span></span>
