---
title: 'Requêtes asynchrones : EF Core'
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052679"
---
# <a name="asynchronous-queries"></a>Requêtes asynchrones

Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données. Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourde. Les opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré pour d’autres demandes de service lors de la fin de l’opération de base de données. Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core ne prend pas en charge les opérations parallèles multiples en cours d’exécution sur la même instance de contexte. Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante. Cela est généralement effectué à l’aide du mot-clé `await` sur chaque opération asynchrone.

Entity Framework Core fournit un ensemble de méthodes d’extension asynchrone qui peuvent être utilisées comme alternative pour les méthodes LINQ qui provoquent l’exécution d’une requête et le retour de résultats. Par exemple, `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. Il n’existe pas de versions asynchrones des opérateurs LINQ comme `Where(...)`, `OrderBy(...)`, etc., car ces méthodes construisent uniquement l’arborescence de l’expression LINQ et n’entraînent pas l’exécution de la requête dans la base de données.

> [!IMPORTANT]  
> Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`. Cet espace de noms doit être importé pour que les méthodes soient disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
