---
title: 'Requêtes asynchrones : EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417752"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="769f3-102">Requêtes asynchrones</span><span class="sxs-lookup"><span data-stu-id="769f3-102">Asynchronous Queries</span></span>

<span data-ttu-id="769f3-103">Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="769f3-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="769f3-104">Les requêtes Async sont importantes pour conserver une interface utilisateur réactive dans des applications clientes lourdes.</span><span class="sxs-lookup"><span data-stu-id="769f3-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="769f3-105">Ils peuvent également augmenter le débit dans les applications Web où ils libèrent le thread pour traiter d’autres requêtes dans des applications Web.</span><span class="sxs-lookup"><span data-stu-id="769f3-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="769f3-106">Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="769f3-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="769f3-107">EF Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="769f3-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="769f3-108">Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="769f3-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="769f3-109">Pour ce faire, utilisez le mot clé `await` sur chaque opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="769f3-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="769f3-110">Entity Framework Core fournit un ensemble de méthodes d’extension Async similaires aux méthodes LINQ, qui exécutent une requête et retournent des résultats.</span><span class="sxs-lookup"><span data-stu-id="769f3-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="769f3-111">Exemples : `ToListAsync()`, `ToArrayAsync()``SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="769f3-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="769f3-112">Il n’existe aucune version asynchrone de certains opérateurs LINQ, tels que `Where(...)` ou `OrderBy(...)`, car ces méthodes génèrent uniquement l’arborescence de l’expression LINQ et n’entraînent pas l’exécution de la requête dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="769f3-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="769f3-113">Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="769f3-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="769f3-114">Cet espace de noms doit être importé pour que les méthodes soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="769f3-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
