---
title: "Héritage (base de données relationnelle) - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="eca2a-102">Héritage (base de données relationnelle)</span><span class="sxs-lookup"><span data-stu-id="eca2a-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="eca2a-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="eca2a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="eca2a-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="eca2a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="eca2a-105">L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="eca2a-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="eca2a-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="eca2a-106">Conventions</span></span>

<span data-ttu-id="eca2a-107">Par convention, l’héritage sera mappé à l’aide du modèle de table par hiérarchie (TPH).</span><span class="sxs-lookup"><span data-stu-id="eca2a-107">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="eca2a-108">TPH utilise une seule table pour stocker les données pour tous les types dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="eca2a-108">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="eca2a-109">Une colonne de discriminateur est utilisée pour identifier le type de chaque ligne représente.</span><span class="sxs-lookup"><span data-stu-id="eca2a-109">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="eca2a-110">EF va installer uniquement l’héritage si deux ou plusieurs types hérités sont explicitement inclus dans le modèle (voir [héritage](../inheritance.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="eca2a-110">EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="eca2a-111">Voici un exemple d’un scénario simple d’héritage et les données stockées dans une table de base de données relationnelle à l’aide du modèle TPH.</span><span class="sxs-lookup"><span data-stu-id="eca2a-111">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="eca2a-112">Le *discriminateur* colonne identifie le type de *Blog* est stocké dans chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="eca2a-112">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="eca2a-114">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="eca2a-114">Data Annotations</span></span>

<span data-ttu-id="eca2a-115">Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="eca2a-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="eca2a-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="eca2a-116">Fluent API</span></span>

<span data-ttu-id="eca2a-117">Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs qui sont utilisées pour identifier chaque type dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="eca2a-117">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
