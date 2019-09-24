---
title: Autres clés-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197464"
---
# <a name="alternate-keys"></a><span data-ttu-id="d5438-102">Clés secondaires</span><span class="sxs-lookup"><span data-stu-id="d5438-102">Alternate Keys</span></span>

<span data-ttu-id="d5438-103">Une autre clé sert d’identificateur unique alternatif pour chaque instance d’entité en plus de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="d5438-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="d5438-104">D’autres clés peuvent être utilisées comme cible d’une relation.</span><span class="sxs-lookup"><span data-stu-id="d5438-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="d5438-105">Lors de l’utilisation d’une base de données relationnelle, cela correspond au concept d’index/de contrainte unique sur la ou les colonnes clés de remplacement et une ou plusieurs contraintes de clé étrangère qui référencent la ou les colonnes.</span><span class="sxs-lookup"><span data-stu-id="d5438-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="d5438-106">Si vous souhaitez simplement garantir l’unicité d’une colonne, vous devez disposer d’un index unique plutôt que d’une clé secondaire, consultez [index](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="d5438-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="d5438-107">Dans EF, les clés alternatives offrent des fonctionnalités supérieures à celles des index uniques, car elles peuvent être utilisées comme cible d’une clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="d5438-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="d5438-108">Des clés alternatives sont généralement introduites pour vous si nécessaire et vous n’avez pas besoin de les configurer manuellement.</span><span class="sxs-lookup"><span data-stu-id="d5438-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="d5438-109">Pour plus d’informations, consultez [conventions](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="d5438-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="d5438-110">Conventions</span><span class="sxs-lookup"><span data-stu-id="d5438-110">Conventions</span></span>

<span data-ttu-id="d5438-111">Par Convention, une clé secondaire est introduite pour vous lorsque vous identifiez une propriété, qui n’est pas la clé primaire, comme la cible d’une relation.</span><span class="sxs-lookup"><span data-stu-id="d5438-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="d5438-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="d5438-112">Data Annotations</span></span>

<span data-ttu-id="d5438-113">Les clés secondaires ne peuvent pas être configurées à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="d5438-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d5438-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d5438-114">Fluent API</span></span>

<span data-ttu-id="d5438-115">Vous pouvez utiliser l’API Fluent pour configurer une seule propriété pour qu’elle soit une clé secondaire.</span><span class="sxs-lookup"><span data-stu-id="d5438-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="d5438-116">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé secondaire (appelée clé de remplacement composite).</span><span class="sxs-lookup"><span data-stu-id="d5438-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
