---
title: Microsoft SQL Server de base de données fournisseur - Tables optimisées en mémoire - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995800"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="4661f-102">Prend en charge des Tables optimisées en mémoire dans le fournisseur de base de données SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="4661f-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="4661f-103">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4661f-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="4661f-104">[Tables optimisées en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire.</span><span class="sxs-lookup"><span data-stu-id="4661f-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="4661f-105">Une deuxième copie de données de la table est conservée sur le disque, mais uniquement à des fins de durabilité.</span><span class="sxs-lookup"><span data-stu-id="4661f-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="4661f-106">Données dans les tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération de base de données.</span><span class="sxs-lookup"><span data-stu-id="4661f-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="4661f-107">Par exemple, après un redémarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="4661f-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="4661f-108">Configuration d’une table mémoire optimisée</span><span class="sxs-lookup"><span data-stu-id="4661f-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="4661f-109">Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée.</span><span class="sxs-lookup"><span data-stu-id="4661f-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="4661f-110">Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="4661f-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
