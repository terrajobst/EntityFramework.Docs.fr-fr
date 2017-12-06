---
title: "Clés (principal) - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="keys-primary"></a><span data-ttu-id="4a04f-102">Clés (principal)</span><span class="sxs-lookup"><span data-stu-id="4a04f-102">Keys (primary)</span></span>

<span data-ttu-id="4a04f-103">Une clé sert d’identificateur principal unique pour chaque instance d’entité.</span><span class="sxs-lookup"><span data-stu-id="4a04f-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="4a04f-104">Lorsque vous utilisez une base de données relationnelle correspond au concept d’un *clé primaire*.</span><span class="sxs-lookup"><span data-stu-id="4a04f-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="4a04f-105">Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (voir [clés secondaires](alternate-keys.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="4a04f-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="4a04f-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="4a04f-106">Conventions</span></span>

<span data-ttu-id="4a04f-107">Par convention, une propriété nommée `Id` ou `<type name>Id` sera configuré en tant que la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="4a04f-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="4a04f-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="4a04f-108">Data Annotations</span></span>

<span data-ttu-id="4a04f-109">Vous pouvez utiliser des Annotations de données pour configurer une seule propriété qui sera la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="4a04f-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="4a04f-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="4a04f-110">Fluent API</span></span>

<span data-ttu-id="4a04f-111">Vous pouvez utiliser l’API Fluent pour configurer une seule propriété qui sera la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="4a04f-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="4a04f-112">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés de la clé d’une entité (appelée une clé composite).</span><span class="sxs-lookup"><span data-stu-id="4a04f-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="4a04f-113">Clés composites ne peuvent être configurées à l’aide de l’API Fluent - conventions va installer jamais une clé composite, et vous ne pouvez pas utiliser des Annotations de données pour configurer une.</span><span class="sxs-lookup"><span data-stu-id="4a04f-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
