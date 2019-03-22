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
# <a name="keys-primary"></a>Clés (primaire)

Une clé sert d’identificateur principal unique pour chaque instance d’entité. Lorsque vous utilisez une base de données relationnelle correspond à la notion d’un *clé primaire*. Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (consultez [clés secondaires](alternate-keys.md) pour plus d’informations). 

Une des méthodes suivantes peut être utilisée pour le programme d’installation/créer une clé primaire.

## <a name="conventions"></a>Conventions

Par convention, une propriété nommée `Id` ou `<type name>Id` sera configuré en tant que la clé d’une entité.

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

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer une propriété unique pour être la clé d’une entité.

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

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété unique pour être la clé d’une entité.

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

Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés de la clé d’une entité (appelée une clé composite). Clés composites ne peuvent être configurés à l’aide de l’API Fluent : conventions d’installation ajoute jamais une clé composite, et vous ne pouvez pas utiliser des Annotations de données pour configurer une.

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
