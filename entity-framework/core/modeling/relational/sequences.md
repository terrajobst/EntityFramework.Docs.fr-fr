---
title: "Séquences - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="sequences"></a><span data-ttu-id="7c4c3-102">Séquences</span><span class="sxs-lookup"><span data-stu-id="7c4c3-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="7c4c3-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7c4c3-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="7c4c3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7c4c3-105">Une séquence génère des valeurs numériques séquentielles dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="7c4c3-106">Les séquences ne sont pas associés à une table spécifique.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="7c4c3-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="7c4c3-107">Conventions</span></span>

<span data-ttu-id="7c4c3-108">Par convention, les séquences ne sont pas introduits dans pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7c4c3-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="7c4c3-109">Data Annotations</span></span>

<span data-ttu-id="7c4c3-110">Vous ne pouvez pas configurer une séquence à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7c4c3-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7c4c3-111">Fluent API</span></span>

<span data-ttu-id="7c4c3-112">Vous pouvez utiliser l’API Fluent pour créer une séquence dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="7c4c3-113">Vous pouvez également configurer des aspects supplémentaires de la séquence, telles que son schéma, une valeur de départ et un incrément.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="7c4c3-114">Lorsqu’une séquence est présentée, vous pouvez l’utiliser pour générer des valeurs pour les propriétés dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="7c4c3-115">Par exemple, vous pouvez utiliser [les valeurs par défaut](default-values.md) pour insérer la valeur suivante à partir de la séquence.</span><span class="sxs-lookup"><span data-stu-id="7c4c3-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
