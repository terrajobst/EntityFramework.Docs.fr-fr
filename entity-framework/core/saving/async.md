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
# <a name="asynchronous-saving"></a><span data-ttu-id="9fea3-102">Enregistrement asynchrone</span><span class="sxs-lookup"><span data-stu-id="9fea3-102">Asynchronous Saving</span></span>

<span data-ttu-id="9fea3-103">L’enregistrement asynchrone évite de bloquer un thread tandis que les modifications sont écrites dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="9fea3-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="9fea3-104">Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourd.</span><span class="sxs-lookup"><span data-stu-id="9fea3-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="9fea3-105">Opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré à d’autres demandes de service lors de la fin de l’opération de base de données.</span><span class="sxs-lookup"><span data-stu-id="9fea3-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="9fea3-106">Pour plus d’informations, consultez [de programmation asynchrone en c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="9fea3-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="9fea3-107">EF Core ne prend pas en charge plusieurs opérations parallèles en cours d’exécution sur la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="9fea3-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="9fea3-108">Vous devez attendre systématiquement pour une opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="9fea3-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="9fea3-109">Cela est généralement effectué à l’aide de la `await` (mot clé) de chaque opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9fea3-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="9fea3-110">Entity Framework Core fournit `DbContext.SaveChangesAsync()` comme alternative asynchrone à `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9fea3-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
