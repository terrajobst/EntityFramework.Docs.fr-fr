---
title: Mappage de table - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949007"
---
# <a name="table-mapping"></a><span data-ttu-id="1e366-102">Mappage de table</span><span class="sxs-lookup"><span data-stu-id="1e366-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="1e366-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="1e366-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1e366-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1e366-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1e366-105">Mappage de table identifie les données de table doivent être interrogées à partir d’et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1e366-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1e366-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="1e366-106">Conventions</span></span>

<span data-ttu-id="1e366-107">Par convention, chaque entité sera être configurée pour mapper à une table portant le même nom que le `DbSet<TEntity>` propriété qui expose l’entité sur le contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="1e366-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="1e366-108">Si aucun `DbSet<TEntity>` est inclus pour l’entité donnée, le nom de classe est utilisé.</span><span class="sxs-lookup"><span data-stu-id="1e366-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1e366-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="1e366-109">Data Annotations</span></span>

<span data-ttu-id="1e366-110">Vous pouvez utiliser des Annotations de données pour configurer la table qui a un type est mappé au.</span><span class="sxs-lookup"><span data-stu-id="1e366-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="1e366-111">Vous pouvez également spécifier un schéma auquel appartient la table.</span><span class="sxs-lookup"><span data-stu-id="1e366-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="1e366-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="1e366-112">Fluent API</span></span>

<span data-ttu-id="1e366-113">Vous pouvez utiliser l’API Fluent pour configurer la table qui a un type est mappé au.</span><span class="sxs-lookup"><span data-stu-id="1e366-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="1e366-114">Vous pouvez également spécifier un schéma auquel appartient la table.</span><span class="sxs-lookup"><span data-stu-id="1e366-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
