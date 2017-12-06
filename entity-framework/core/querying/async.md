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
# <a name="asynchronous-queries"></a><span data-ttu-id="2afe2-102">Requêtes asynchrones</span><span class="sxs-lookup"><span data-stu-id="2afe2-102">Asynchronous Queries</span></span>

<span data-ttu-id="2afe2-103">Requêtes asynchrones éviter de bloquer un thread pendant que la requête est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2afe2-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="2afe2-104">Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourd.</span><span class="sxs-lookup"><span data-stu-id="2afe2-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="2afe2-105">Opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré à d’autres demandes de service lors de la fin de l’opération de base de données.</span><span class="sxs-lookup"><span data-stu-id="2afe2-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="2afe2-106">Pour plus d’informations, consultez [de programmation asynchrone en c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="2afe2-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="2afe2-107">EF Core ne prend pas en charge plusieurs opérations parallèles en cours d’exécution sur la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="2afe2-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="2afe2-108">Vous devez attendre systématiquement pour une opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="2afe2-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="2afe2-109">Cela est généralement effectué à l’aide de la `await` (mot clé) de chaque opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="2afe2-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="2afe2-110">Entity Framework Core fournit un ensemble de méthodes d’extension asynchrone qui peut être utilisé comme alternative pour les méthodes LINQ qui provoquent une requête à exécuter et les résultats retournés.</span><span class="sxs-lookup"><span data-stu-id="2afe2-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="2afe2-111">Exemples `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. Il n’existe pas les versions asynchrones des opérateurs LINQ comme `Where(...)`, `OrderBy(...)`, etc., car ces méthodes uniquement construire l’arborescence d’expression LINQ et n’entraînent pas la requête doit être exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2afe2-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="2afe2-112">Les méthodes d’extension EF Core async sont définies dans le `Microsoft.EntityFrameworkCore` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="2afe2-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="2afe2-113">Cet espace de noms doit être importé pour les méthodes soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="2afe2-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
