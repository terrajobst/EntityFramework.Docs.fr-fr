---
title: Nouveautés d’EF Core 2.2 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417446"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="4f6aa-102">Nouvelles fonctionnalités d’EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="4f6aa-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="4f6aa-103">Prise en charge des données spatiales</span><span class="sxs-lookup"><span data-stu-id="4f6aa-103">Spatial data support</span></span>

<span data-ttu-id="4f6aa-104">Les données spatiales peuvent être utilisées pour représenter l’emplacement physique et la forme d’objets.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="4f6aa-105">Plusieurs bases de données peuvent nativement stocker, indexer et interroger des données spatiales.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-105">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="4f6aa-106">Les scénarios courants incluent l’interrogation d’objets dans un rayon donné et le test pour vérifier si un polygone contient un emplacement donné.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="4f6aa-107">EF Core 2.2 prend désormais en charge l’utilisation de données spatiales provenant de diverses bases de données à l’aide de types issus de la bibliothèque [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).</span><span class="sxs-lookup"><span data-stu-id="4f6aa-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="4f6aa-108">La prise en charge des données spatiales est implémenté sous forme d’une série de packages d’extension spécifiques à un fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="4f6aa-109">Chacun de ces packages contribue aux mappages des types NTS et des méthodes ainsi que des types spatiaux et fonctions correspondants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="4f6aa-110">Ces extensions de fournisseur sont désormais disponibles pour [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), et [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (à partir du [projet Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="4f6aa-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="4f6aa-111">Les types spatiaux peuvent être utilisés directement avec le [fournisseur en mémoire EF Core](xref:core/providers/in-memory/index), sans extensions supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-111">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="4f6aa-112">Une fois l’extension du fournisseur installée, vous pouvez ajouter à vos entités des propriétés de types pris en charge.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="4f6aa-113">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

<span data-ttu-id="4f6aa-114">Vous pouvez alors conserver les entités avec des données spatiales :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

<span data-ttu-id="4f6aa-115">Et vous pouvez exécuter des requêtes de base de données basées sur des données spatiales et des opérations :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="4f6aa-116">Pour plus d’informations sur cette fonctionnalité, consultez la [documentation sur les types spatiaux](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="4f6aa-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="4f6aa-117">Collections d’entités détenues</span><span class="sxs-lookup"><span data-stu-id="4f6aa-117">Collections of owned entities</span></span>

<span data-ttu-id="4f6aa-118">EF Core 2.0 a ajouté la possibilité de modéliser la propriété dans les associations un-à-un.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="4f6aa-119">EF Core 2.2 étend la capacité d’exprimer la propriété aux associations un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="4f6aa-120">La propriété permet de limiter la façon dont les entités sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="4f6aa-121">Par exemple, les entités détenues :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-121">For example, owned entities:</span></span>

- <span data-ttu-id="4f6aa-122">Peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-122">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="4f6aa-123">Sont automatiquement chargées et ne peut être uniquement suivies par un objet DbContext en même temps que leur propriétaire.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="4f6aa-124">Dans les bases de données relationnelles, les collections détenues sont mappées à des tables distinctes du propriétaire, comme des associations un-à-plusieurs standard.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="4f6aa-125">Mais dans les bases de données orientées document, nous prévoyons d’imbriquer les entités détenues (dans des collections ou références détenues) dans le même document que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="4f6aa-126">Vous pouvez utiliser cette fonctionnalité en appelant la nouvelle API OwnsMany() :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="4f6aa-127">Pour plus d’informations, consultez la [documentation mise à jour sur les entités détenues](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="4f6aa-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="4f6aa-128">Balises de requête</span><span class="sxs-lookup"><span data-stu-id="4f6aa-128">Query tags</span></span>

<span data-ttu-id="4f6aa-129">Cette fonctionnalité simplifie la corrélation des requêtes LINQ dans le code avec des requêtes SQL générées et capturées dans des journaux.</span><span class="sxs-lookup"><span data-stu-id="4f6aa-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="4f6aa-130">Pour tirer parti des balises de requête, vous annotez une requête LINQ à l’aide de la nouvelle méthode TagWith().</span><span class="sxs-lookup"><span data-stu-id="4f6aa-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="4f6aa-131">En utilisant la requête spatiale d’un exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="4f6aa-132">Cette requête LINQ générera la sortie SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="4f6aa-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="4f6aa-133">Pour plus d’informations, consultez la [documentation sur les balises de requête](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="4f6aa-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
