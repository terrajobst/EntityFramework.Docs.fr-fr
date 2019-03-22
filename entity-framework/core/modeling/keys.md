---
title: Clés (principal) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319164"
---
# <a name="keys-primary"></a><span data-ttu-id="f31f5-102">Clés (primaire)</span><span class="sxs-lookup"><span data-stu-id="f31f5-102">Keys (primary)</span></span>

<span data-ttu-id="f31f5-103">Une clé sert d’identificateur principal unique pour chaque instance d’entité.</span><span class="sxs-lookup"><span data-stu-id="f31f5-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="f31f5-104">Lorsque vous utilisez une base de données relationnelle correspond à la notion d’un *clé primaire*.</span><span class="sxs-lookup"><span data-stu-id="f31f5-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="f31f5-105">Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (consultez [clés secondaires](alternate-keys.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="f31f5-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="f31f5-106">Une des méthodes suivantes peut être utilisée pour le programme d’installation/créer une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="f31f5-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="f31f5-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="f31f5-107">Conventions</span></span>

<span data-ttu-id="f31f5-108">Par convention, une propriété nommée `Id` ou `<type name>Id` sera configuré en tant que la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="f31f5-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="f31f5-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="f31f5-109">Data Annotations</span></span>

<span data-ttu-id="f31f5-110">Vous pouvez utiliser des Annotations de données pour configurer une propriété unique pour être la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="f31f5-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="f31f5-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="f31f5-111">Fluent API</span></span>

<span data-ttu-id="f31f5-112">Vous pouvez utiliser l’API Fluent pour configurer une propriété unique pour être la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="f31f5-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="f31f5-113">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés de la clé d’une entité (appelée une clé composite).</span><span class="sxs-lookup"><span data-stu-id="f31f5-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="f31f5-114">Clés composites ne peuvent être configurés à l’aide de l’API Fluent : conventions d’installation ajoute jamais une clé composite, et vous ne pouvez pas utiliser des Annotations de données pour configurer une.</span><span class="sxs-lookup"><span data-stu-id="f31f5-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
