---
title: Fournisseur de base de données Microsoft SQL Server-tables mémoire optimisées-EF Core
description: Utilisation de tables optimisées en mémoire avec le fournisseur de base de données SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417790"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="53e25-103">Prise en charge des tables optimisées en mémoire dans SQL Server fournisseur de base de données EF Core</span><span class="sxs-lookup"><span data-stu-id="53e25-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="53e25-104">Les [tables mémoire optimisées](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire.</span><span class="sxs-lookup"><span data-stu-id="53e25-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="53e25-105">Une deuxième copie des données de la table est conservée sur le disque, mais uniquement pour la durabilité.</span><span class="sxs-lookup"><span data-stu-id="53e25-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="53e25-106">Les données des tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération d'une base de données.</span><span class="sxs-lookup"><span data-stu-id="53e25-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="53e25-107">Par exemple, après le redémarrage d'un serveur.</span><span class="sxs-lookup"><span data-stu-id="53e25-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="53e25-108">Configuration d’une table optimisée en mémoire</span><span class="sxs-lookup"><span data-stu-id="53e25-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="53e25-109">Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée.</span><span class="sxs-lookup"><span data-stu-id="53e25-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="53e25-110">Lorsque vous utilisez EF Core pour créer et gérer une base de données basée sur votre modèle (avec [migrations](xref:core/managing-schemas/migrations/index) ou [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), une table optimisée en mémoire est créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="53e25-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
