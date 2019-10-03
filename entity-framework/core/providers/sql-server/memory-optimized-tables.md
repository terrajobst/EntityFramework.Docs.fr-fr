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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Prise en charge des tables optimisées en mémoire dans SQL Server fournisseur de base de données EF Core

Les [tables mémoire optimisées](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire. Une deuxième copie des données de la table est conservée sur le disque, mais uniquement à des fins de durabilité. Les données des tables mémoire optimisées sont uniquement lues à partir du disque pendant la récupération de la base de données. Par exemple, après un redémarrage du serveur.

## <a name="configuring-a-memory-optimized-table"></a>Configuration d’une table optimisée en mémoire

Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée. Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
