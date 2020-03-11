---
title: Fournisseur de Azure Cosmos DB-limitations-EF Core
description: Limitations du fournisseur de Azure Cosmos DB Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417208"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Limitations des fournisseurs de EF Core Azure Cosmos DB

Le fournisseur Cosmos présente un certain nombre de limitations. La plupart de ces limitations résultent de limitations dans le moteur de base de données Cosmos sous-jacent et ne sont pas spécifiques à EF. Mais la plupart [n’ont pas encore été implémentées](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Limitations temporaires

- Même s’il existe un seul type d’entité sans héritage mappé à un conteneur, il possède toujours une propriété de discriminateur.
- Les types d’entités avec des clés de partition ne fonctionnent pas correctement dans certains scénarios
- les appels de `Include` ne sont pas pris en charge
- les appels de `Join` ne sont pas pris en charge

## <a name="azure-cosmos-db-sdk-limitations"></a>Limitations du kit de développement logiciel Azure Cosmos DB

- Seules les méthodes Async sont fournies

> [!WARNING]
> Étant donné qu’il n’existe aucune version de synchronisation des méthodes de bas niveau EF Core s’appuie sur, la fonctionnalité correspondante est actuellement implémentée en appelant `.Wait()` sur le `Task`retourné. Cela signifie que l’utilisation de méthodes telles que `SaveChanges`ou `ToList` au lieu de leurs équivalents asynchrones peut entraîner un interblocage dans votre application

## <a name="azure-cosmos-db-limitations"></a>Limitations de Azure Cosmos DB

Vous pouvez voir la vue d’ensemble complète des [fonctionnalités prises en charge par Azure Cosmos DB](/azure/cosmos-db/modeling-data). il s’agit des différences les plus notables par rapport à une base de données relationnelle :

- Les transactions initiées par le client ne sont pas prises en charge
- Certaines requêtes de plusieurs partitions ne sont pas prises en charge ou sont beaucoup plus lentes en fonction des opérateurs impliqués
