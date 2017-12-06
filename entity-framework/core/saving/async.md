---
title: Enregistrement asynchrone - EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a>Enregistrement asynchrone

L’enregistrement asynchrone évite de bloquer un thread tandis que les modifications sont écrites dans la base de données. Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourd. Opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré à d’autres demandes de service lors de la fin de l’opération de base de données. Pour plus d’informations, consultez [de programmation asynchrone en c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core ne prend pas en charge plusieurs opérations parallèles en cours d’exécution sur la même instance de contexte. Vous devez attendre systématiquement pour une opération se termine avant de commencer l’opération suivante. Cela est généralement effectué à l’aide de la `await` (mot clé) de chaque opération asynchrone.

Entity Framework Core fournit `DbContext.SaveChangesAsync()` comme alternative asynchrone à `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
