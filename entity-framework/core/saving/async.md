---
title: 'Enregistrement asynchrone : EF Core'
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997280"
---
# <a name="asynchronous-saving"></a>Enregistrement asynchrone

L’enregistrement asynchrone évite de bloquer un thread lorsque les modifications sont écrites dans la base de données. Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourde. Les opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré pour d’autres demandes de service lors de la fin de l’opération de base de données. Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core ne prend pas en charge les opérations parallèles multiples en cours d’exécution sur la même instance de contexte. Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante. Cela est généralement effectué à l’aide du mot-clé `await` sur chaque opération asynchrone.

Entity Framework Core fournit `DbContext.SaveChangesAsync()` comme alternative asynchrone à `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
