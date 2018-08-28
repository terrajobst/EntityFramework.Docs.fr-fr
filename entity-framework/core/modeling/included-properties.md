---
title: Inclusion et exclusion de propriétés - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 07b70e4517b67490e04a9ec9fa22b9b5d5217681
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998253"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="c6b7b-102">Inclusion et exclusion de propriétés</span><span class="sxs-lookup"><span data-stu-id="c6b7b-102">Including & Excluding Properties</span></span>

<span data-ttu-id="c6b7b-103">Y compris une propriété dans le modèle signifie qu’EF comporte des métadonnées sur cette propriété et va tenter de lire et écrire les valeurs à partir / vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6b7b-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c6b7b-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="c6b7b-104">Conventions</span></span>

<span data-ttu-id="c6b7b-105">Par convention, les propriétés publiques avec un accesseur Get et un accesseur Set figureront dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="c6b7b-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c6b7b-106">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="c6b7b-106">Data Annotations</span></span>

<span data-ttu-id="c6b7b-107">Vous pouvez utiliser des Annotations de données pour exclure une propriété à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="c6b7b-107">You can use Data Annotations to exclude a property from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="c6b7b-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c6b7b-108">Fluent API</span></span>

<span data-ttu-id="c6b7b-109">Vous pouvez utiliser l’API Fluent pour exclure une propriété à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="c6b7b-109">You can use the Fluent API to exclude a property from the model.</span></span>

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
