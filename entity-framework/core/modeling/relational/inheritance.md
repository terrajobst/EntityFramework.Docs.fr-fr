---
title: Héritage (base de données relationnelle) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996320"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="fd306-102">Héritage (base de données relationnelle)</span><span class="sxs-lookup"><span data-stu-id="fd306-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="fd306-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="fd306-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="fd306-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="fd306-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="fd306-105">L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="fd306-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="fd306-106">Actuellement, uniquement au modèle de table par hiérarchie (TPH) est implémenté dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="fd306-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="fd306-107">Autres modèles courants comme table par type (TPT) et table-par-type concret (TPC) ne sont pas encore disponibles.</span><span class="sxs-lookup"><span data-stu-id="fd306-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="fd306-108">Conventions</span><span class="sxs-lookup"><span data-stu-id="fd306-108">Conventions</span></span>

<span data-ttu-id="fd306-109">Par convention, l’héritage sera mappé à l’aide du modèle de table par hiérarchie (TPH).</span><span class="sxs-lookup"><span data-stu-id="fd306-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="fd306-110">TPH utilise une seule table pour stocker les données pour tous les types dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="fd306-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="fd306-111">Une colonne de discriminateur est utilisée pour identifier le type de chaque ligne représente.</span><span class="sxs-lookup"><span data-stu-id="fd306-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="fd306-112">EF Core sera uniquement installer héritage si deux ou plusieurs des types hérités sont explicitement inclus dans le modèle (voir [héritage](../inheritance.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="fd306-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="fd306-113">Voici un exemple montrant un scénario d’héritage simple et les données stockées dans une table de base de données relationnelle en utilisant le modèle TPH.</span><span class="sxs-lookup"><span data-stu-id="fd306-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="fd306-114">Le *discriminateur* colonne identifie le type de *Blog* sont stockées dans chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="fd306-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![image](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="fd306-116">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="fd306-116">Data Annotations</span></span>

<span data-ttu-id="fd306-117">Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="fd306-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fd306-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="fd306-118">Fluent API</span></span>

<span data-ttu-id="fd306-119">Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs qui sont utilisées pour identifier chaque type dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="fd306-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="fd306-120">Configuration de la propriété de discriminateur</span><span class="sxs-lookup"><span data-stu-id="fd306-120">Configuring the discriminator property</span></span>

<span data-ttu-id="fd306-121">Dans les exemples ci-dessus, le discriminateur est créé comme un [occulter la propriété](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="fd306-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="fd306-122">Dans la mesure où il s’agit d’une propriété dans le modèle, il peut être configuré comme d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="fd306-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="fd306-123">Par exemple, pour définir la longueur maximale lorsque la valeur par défaut, les discriminateur par convention est utilisée :</span><span class="sxs-lookup"><span data-stu-id="fd306-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="fd306-124">Le discriminateur peut également être mappé à une propriété CLR réelle de votre entité.</span><span class="sxs-lookup"><span data-stu-id="fd306-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="fd306-125">Exemple :</span><span class="sxs-lookup"><span data-stu-id="fd306-125">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="fd306-126">Combinant ces deux opérations, il est possible de mapper le discriminateur pour une véritable propriété et le configurer :</span><span class="sxs-lookup"><span data-stu-id="fd306-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
