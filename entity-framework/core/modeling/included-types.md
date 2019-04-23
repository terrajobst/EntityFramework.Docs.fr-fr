---
title: Inclusion et exclusion de Types - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929795"
---
# <a name="including--excluding-types"></a><span data-ttu-id="7e2bb-102">Inclusion et exclusion de Types</span><span class="sxs-lookup"><span data-stu-id="7e2bb-102">Including & Excluding Types</span></span>

<span data-ttu-id="7e2bb-103">Dans les moyens de modèle EF a des métadonnées sur ce type et tentent de lire et écrire des instances à partir / vers la base de données, y compris un type.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7e2bb-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="7e2bb-104">Conventions</span></span>

<span data-ttu-id="7e2bb-105">Par convention, les types qui sont exposés dans `DbSet` propriétés de votre contexte sont incluses dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="7e2bb-106">En outre, les types qui sont mentionnées dans le `OnModelCreating` méthode sont également incluses.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="7e2bb-107">Enfin, tous les types qui sont trouvent en explorant les propriétés de navigation de types découverts de manière récursive sont également inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="7e2bb-108">**Par exemple, les trois types sont détectés dans le code suivant :**</span><span class="sxs-lookup"><span data-stu-id="7e2bb-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="7e2bb-109">`Blog` car elle est exposée dans un `DbSet` propriété dans le contexte</span><span class="sxs-lookup"><span data-stu-id="7e2bb-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="7e2bb-110">`Post` car il est découvert par le biais de la `Blog.Posts` propriété de navigation</span><span class="sxs-lookup"><span data-stu-id="7e2bb-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="7e2bb-111">`AuditEntry` car il sera mentionné dans `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="7e2bb-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="7e2bb-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="7e2bb-112">Data Annotations</span></span>

<span data-ttu-id="7e2bb-113">Vous pouvez utiliser des Annotations de données pour exclure un type à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="7e2bb-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7e2bb-114">Fluent API</span></span>

<span data-ttu-id="7e2bb-115">Vous pouvez utiliser l’API Fluent pour exclure un type à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="7e2bb-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
