---
title: Mappage de colonne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052899"
---
# <a name="column-mapping"></a><span data-ttu-id="e0177-102">Mappage de colonnes</span><span class="sxs-lookup"><span data-stu-id="e0177-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="e0177-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="e0177-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e0177-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="e0177-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e0177-105">Mappage de colonne identifie les données de la colonne doivent être interrogées à partir d’et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e0177-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e0177-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="e0177-106">Conventions</span></span>

<span data-ttu-id="e0177-107">Par convention, chaque propriété sera le programme d’installation pour mapper à une colonne portant le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="e0177-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e0177-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="e0177-108">Data Annotations</span></span>

<span data-ttu-id="e0177-109">Vous pouvez utiliser des Annotations de données pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="e0177-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="e0177-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e0177-110">Fluent API</span></span>

<span data-ttu-id="e0177-111">Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="e0177-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
