---
title: 'Requêtes asynchrones : EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181819"
---
# <a name="asynchronous-queries"></a>Requêtes asynchrones

Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données. Les requêtes Async sont importantes pour conserver une interface utilisateur réactive dans des applications clientes lourdes. Ils peuvent également augmenter le débit dans les applications Web où ils libèrent le thread pour traiter d’autres requêtes dans des applications Web. Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même instance de contexte. Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante. Cela s’effectue généralement en utilisant le mot clé `await` sur chaque opération asynchrone.

Entity Framework Core fournit un ensemble de méthodes d’extension Async similaires aux méthodes LINQ, qui exécutent une requête et retournent des résultats. Exemples : `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`. Il n’existe aucune version asynchrone de certains opérateurs LINQ, tels que `Where(...)` ou `OrderBy(...)`, car ces méthodes génèrent uniquement l’arborescence de l’expression LINQ et n’entraînent pas l’exécution de la requête dans la base de données.

> [!IMPORTANT]  
> Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`. Cet espace de noms doit être importé pour que les méthodes soient disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
