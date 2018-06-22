---
title: Microsoft SQL Server de base de données fournisseur - Tables optimisées en mémoire - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052639"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="652c5-102">Prend en charge les Tables optimisées en mémoire dans le fournisseur de base de données SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="652c5-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="652c5-103">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="652c5-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="652c5-104">[Tables optimisées en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire.</span><span class="sxs-lookup"><span data-stu-id="652c5-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="652c5-105">Une deuxième copie des données de la table est conservée sur le disque, mais uniquement pour la durabilité.</span><span class="sxs-lookup"><span data-stu-id="652c5-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="652c5-106">Les données des tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération d'une base de données.</span><span class="sxs-lookup"><span data-stu-id="652c5-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="652c5-107">Par exemple, après le redémarrage d'un serveur.</span><span class="sxs-lookup"><span data-stu-id="652c5-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="652c5-108">Configuration d’une table optimisée en mémoire</span><span class="sxs-lookup"><span data-stu-id="652c5-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="652c5-109">Vous pouvez spécifier que la table d’à qu'une entité est mappée est optimisée en mémoire.</span><span class="sxs-lookup"><span data-stu-id="652c5-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="652c5-110">Pour créer et gérer une base de données à l’aide de EF Core lorsque selon votre modèle (avec migrations ou `Database.EnsureCreated()`), une table mémoire optimisée sera créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="652c5-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
