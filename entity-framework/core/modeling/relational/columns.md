---
title: Mappage de colonnes - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: bca9ca22d211aa58a3bba00f6e4d54b8fe4a0df8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996202"
---
# <a name="column-mapping"></a><span data-ttu-id="84efc-102">Mappage de colonnes</span><span class="sxs-lookup"><span data-stu-id="84efc-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="84efc-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="84efc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="84efc-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="84efc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="84efc-105">Mappage de colonne identifie les données de la colonne doivent être interrogées à partir d’et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="84efc-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="84efc-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="84efc-106">Conventions</span></span>

<span data-ttu-id="84efc-107">Par convention, chaque propriété sera être configurée pour mapper à une colonne portant le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="84efc-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="84efc-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="84efc-108">Data Annotations</span></span>

<span data-ttu-id="84efc-109">Vous pouvez utiliser des Annotations de données pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="84efc-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="84efc-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="84efc-110">Fluent API</span></span>

<span data-ttu-id="84efc-111">Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="84efc-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

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
