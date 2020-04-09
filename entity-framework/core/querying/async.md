---
title: 'Requêtes asynchrones : EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417752"
---
# <a name="asynchronous-queries"></a>Requêtes asynchrones

Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données. Les requêtes Async sont importantes pour maintenir une interface utilisateur réactive dans les applications à client épais. Ils peuvent également augmenter le débit dans les applications Web où ils libèrent le thread pour servir d’autres demandes dans les applications Web. Pour plus d’informations, voir [Asynchrone Programmation en C .](/dotnet/csharp/async)

> [!WARNING]  
> EF Core ne prend pas en charge l’exécution de plusieurs opérations parallèles dans le même contexte. Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante. Ceci est généralement fait `await` en utilisant le mot clé sur chaque opération async.

Entity Framework Core fournit un ensemble de méthodes d’extension async similaires aux méthodes LINQ, qui exécutent une requête et des résultats de retour. Exemples `ToListAsync()` `ToArrayAsync()`incluent `SingleAsync()`, , . Il n’existe pas de versions async de certains exploitants de LINQ, comme `Where(...)` ou `OrderBy(...)` parce que ces méthodes ne font qu’accumuler l’arbre d’expression de la LINQ et ne font pas exécuter la requête dans la base de données.

> [!IMPORTANT]  
> Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`. Cet espace de noms doit être importé pour que les méthodes soient disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
