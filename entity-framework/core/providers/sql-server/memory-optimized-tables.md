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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Prend en charge des Tables optimisées en mémoire dans le fournisseur de base de données SQL Server EF Core

> [!NOTE]  
>
> Cette fonctionnalité a été introduite dans EF Core 1.1.

[Tables optimisées en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire. Une deuxième copie de données de la table est conservée sur le disque, mais uniquement à des fins de durabilité. Données dans les tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération de base de données. Par exemple, après un redémarrage du serveur.

## <a name="configuring-a-memory-optimized-table"></a>Configuration d’une table mémoire optimisée

Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée. Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
