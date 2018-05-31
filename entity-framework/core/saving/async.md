---
title: 'Enregistrement asynchrone : EF Core'
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052619"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="97d55-102">Enregistrement asynchrone</span><span class="sxs-lookup"><span data-stu-id="97d55-102">Asynchronous Saving</span></span>

<span data-ttu-id="97d55-103">L’enregistrement asynchrone évite de bloquer un thread lorsque les modifications sont écrites dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="97d55-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="97d55-104">Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourde.</span><span class="sxs-lookup"><span data-stu-id="97d55-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="97d55-105">Les opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré pour d’autres demandes de service lors de la fin de l’opération de base de données.</span><span class="sxs-lookup"><span data-stu-id="97d55-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="97d55-106">Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="97d55-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="97d55-107">EF Core ne prend pas en charge les opérations parallèles multiples en cours d’exécution sur la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="97d55-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="97d55-108">Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="97d55-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="97d55-109">Cela est généralement effectué à l’aide du mot-clé `await` sur chaque opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="97d55-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="97d55-110">Entity Framework Core fournit `DbContext.SaveChangesAsync()` comme alternative asynchrone à `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="97d55-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
