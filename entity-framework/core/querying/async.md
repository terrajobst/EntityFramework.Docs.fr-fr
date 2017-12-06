---
title: "Requêtes asynchrones - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Requêtes asynchrones

Requêtes asynchrones éviter de bloquer un thread pendant que la requête est exécutée dans la base de données. Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourd. Opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré à d’autres demandes de service lors de la fin de l’opération de base de données. Pour plus d’informations, consultez [de programmation asynchrone en c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core ne prend pas en charge plusieurs opérations parallèles en cours d’exécution sur la même instance de contexte. Vous devez attendre systématiquement pour une opération se termine avant de commencer l’opération suivante. Cela est généralement effectué à l’aide de la `await` (mot clé) de chaque opération asynchrone.

Entity Framework Core fournit un ensemble de méthodes d’extension asynchrone qui peut être utilisé comme alternative pour les méthodes LINQ qui provoquent une requête à exécuter et les résultats retournés. Exemples `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. Il n’existe pas les versions asynchrones des opérateurs LINQ comme `Where(...)`, `OrderBy(...)`, etc., car ces méthodes uniquement construire l’arborescence d’expression LINQ et n’entraînent pas la requête doit être exécutée dans la base de données.

> [!IMPORTANT]  
> Les méthodes d’extension EF Core async sont définies dans le `Microsoft.EntityFrameworkCore` espace de noms. Cet espace de noms doit être importé pour les méthodes soient disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
