---
title: Y compris les & exclusion des propriétés - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052489"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="10e8c-102">Y compris les & exclusion des propriétés</span><span class="sxs-lookup"><span data-stu-id="10e8c-102">Including & Excluding Properties</span></span>

<span data-ttu-id="10e8c-103">Y compris une propriété dans le modèle signifie qu’EF comporte les métadonnées sur cette propriété et tente de lire et écrire des valeurs de/à la base de données.</span><span class="sxs-lookup"><span data-stu-id="10e8c-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="10e8c-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="10e8c-104">Conventions</span></span>

<span data-ttu-id="10e8c-105">Par convention, les propriétés publiques avec un accesseur Get et un accesseur Set seront incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="10e8c-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="10e8c-106">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="10e8c-106">Data Annotations</span></span>

<span data-ttu-id="10e8c-107">Vous pouvez utiliser des Annotations de données pour exclure une propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="10e8c-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="10e8c-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="10e8c-108">Fluent API</span></span>

<span data-ttu-id="10e8c-109">Vous pouvez utiliser l’API Fluent pour exclure une propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="10e8c-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
