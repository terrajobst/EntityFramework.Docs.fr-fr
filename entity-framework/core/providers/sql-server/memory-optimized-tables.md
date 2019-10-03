---
title: Fournisseur de base de données Microsoft SQL Server-tables mémoire optimisées-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813497"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="43c0c-102">Prise en charge des tables optimisées en mémoire dans SQL Server fournisseur de base de données EF Core</span><span class="sxs-lookup"><span data-stu-id="43c0c-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="43c0c-103">Les [tables mémoire optimisées](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire.</span><span class="sxs-lookup"><span data-stu-id="43c0c-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="43c0c-104">Une deuxième copie des données de la table est conservée sur le disque, mais uniquement à des fins de durabilité.</span><span class="sxs-lookup"><span data-stu-id="43c0c-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="43c0c-105">Les données des tables mémoire optimisées sont uniquement lues à partir du disque pendant la récupération de la base de données.</span><span class="sxs-lookup"><span data-stu-id="43c0c-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="43c0c-106">Par exemple, après un redémarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="43c0c-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="43c0c-107">Configuration d’une table optimisée en mémoire</span><span class="sxs-lookup"><span data-stu-id="43c0c-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="43c0c-108">Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée.</span><span class="sxs-lookup"><span data-stu-id="43c0c-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="43c0c-109">Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="43c0c-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
