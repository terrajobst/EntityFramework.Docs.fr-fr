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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Prise en charge des tables optimisées en mémoire dans SQL Server fournisseur de base de données EF Core

Les [tables mémoire optimisées](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire. Une deuxième copie des données de la table est conservée sur le disque, mais uniquement pour la durabilité. Les données des tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération d'une base de données. Par exemple, après le redémarrage d'un serveur.

## <a name="configuring-a-memory-optimized-table"></a>Configuration d’une table optimisée en mémoire

Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée. Lorsque vous utilisez EF Core pour créer et gérer une base de données basée sur votre modèle (avec [migrations](xref:core/managing-schemas/migrations/index) ou [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), une table optimisée en mémoire est créée pour ces entités.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
