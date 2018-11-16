---
title: Créer et supprimer des API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688627"
---
# <a name="create-and-drop-apis"></a>Créer et supprimer des API

Les méthodes EnsureCreated et EnsureDeleted fournissent une alternative légère à [Migrations](migrations/index.md) pour gérer le schéma de base de données. Ces méthodes sont utiles dans les scénarios lorsque les données sont temporaires et peuvent être supprimées lorsque le schéma est modifié. Par exemple lors d’une création de prototypes, dans les tests, ou pour les caches locaux.

Certains fournisseurs (en particulier ceux non relationnelles) ne prennent pas en charge les Migrations. Pour ces fournisseurs, EnsureCreated est souvent le moyen le plus simple pour initialiser le schéma de base de données.

> [!WARNING]
> EnsureCreated et Migrations ne fonctionnent pas bien ensemble. Si vous utilisez des Migrations, n’utilisez pas EnsureCreated pour initialiser le schéma.

Transition à partir de EnsureCreated aux Migrations n’est pas une expérience transparente. Pour cela, le plus simple consiste à supprimer la base de données et de créer de nouveau à l’aide des Migrations. Si vous prévoyez d’utiliser des migrations à l’avenir, il est préférable de simplement commencer avec des Migrations au lieu d’utiliser EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

La méthode EnsureDeleted sera supprimée de la base de données si elle existe. Si vous n’avez pas les autorisations appropriées, une exception est levée.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated crée la base de données si elle n’existe pas et initialiser le schéma de base de données. Si des tables existent (y compris les tables pour une autre classe DbContext), le schéma ne sont pas initialisé.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Les versions asynchrones de ces méthodes sont également disponibles.

## <a name="sql-script"></a>Script SQL

Pour obtenir le SQL utilisé par EnsureCreated, vous pouvez utiliser la méthode GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Plusieurs classes de DbContext

EnsureCreated fonctionne uniquement lorsque aucune table n’est présent dans la base de données. Si nécessaire, vous pouvez écrire votre propre vérification pour voir si le schéma doit être initialisé et utiliser le service IRelationalDatabaseCreator sous-jacent pour initialiser le schéma.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
