---
title: "Microsoft SQL Server de base de données fournisseur - Tables optimisées en mémoire - EF Core"
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
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Prend en charge les Tables optimisées en mémoire dans le fournisseur de base de données SQL Server EF Core

> [!NOTE]  
>
> Cette fonctionnalité a été introduite dans EF Core 1.1.

[Tables optimisées en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sont une fonctionnalité de SQL Server où la table entière réside en mémoire. Une deuxième copie des données de la table est conservée sur le disque, mais uniquement pour la durabilité. Les données des tables mémoire optimisées sont uniquement lues à partir du disque lors de la récupération d'une base de données. Par exemple, après le redémarrage d'un serveur.

## <a name="configuring-a-memory-optimized-table"></a>Configuration d’une table optimisée en mémoire

Vous pouvez spécifier que la table d’à qu'une entité est mappée est optimisée en mémoire. Pour créer et gérer une base de données à l’aide de EF Core lorsque selon votre modèle (avec migrations ou `Database.EnsureCreated()`), une table mémoire optimisée sera créée pour ces entités.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
