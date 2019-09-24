---
title: Mappage de table-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196959"
---
# <a name="table-mapping"></a><span data-ttu-id="1c678-102">Mappage de table</span><span class="sxs-lookup"><span data-stu-id="1c678-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="1c678-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="1c678-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1c678-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1c678-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1c678-105">Le mappage de table identifie les données de table qui doivent être interrogées et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1c678-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1c678-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="1c678-106">Conventions</span></span>

<span data-ttu-id="1c678-107">Par convention, chaque entité est configurée pour être mappée à une table avec le même nom que la propriété `DbSet<TEntity>` qui expose l’entité sur le contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="1c678-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="1c678-108">Si aucun `DbSet<TEntity>` n’est inclus pour l’entité donnée, le nom de la classe est utilisé.</span><span class="sxs-lookup"><span data-stu-id="1c678-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1c678-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="1c678-109">Data Annotations</span></span>

<span data-ttu-id="1c678-110">Vous pouvez utiliser des annotations de données pour configurer la table à laquelle un type est mappé.</span><span class="sxs-lookup"><span data-stu-id="1c678-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="1c678-111">Vous pouvez également spécifier un schéma auquel appartient la table.</span><span class="sxs-lookup"><span data-stu-id="1c678-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="1c678-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="1c678-112">Fluent API</span></span>

<span data-ttu-id="1c678-113">Vous pouvez utiliser l’API Fluent pour configurer la table à laquelle un type est mappé.</span><span class="sxs-lookup"><span data-stu-id="1c678-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="1c678-114">Vous pouvez également spécifier un schéma auquel appartient la table.</span><span class="sxs-lookup"><span data-stu-id="1c678-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
