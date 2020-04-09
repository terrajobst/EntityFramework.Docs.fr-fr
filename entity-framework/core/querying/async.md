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
# <a name="asynchronous-queries"></a><span data-ttu-id="1d9b1-102">Requêtes asynchrones</span><span class="sxs-lookup"><span data-stu-id="1d9b1-102">Asynchronous Queries</span></span>

<span data-ttu-id="1d9b1-103">Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="1d9b1-104">Les requêtes Async sont importantes pour maintenir une interface utilisateur réactive dans les applications à client épais.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="1d9b1-105">Ils peuvent également augmenter le débit dans les applications Web où ils libèrent le thread pour servir d’autres demandes dans les applications Web.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="1d9b1-106">Pour plus d’informations, voir [Asynchrone Programmation en C .](/dotnet/csharp/async)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="1d9b1-107">EF Core ne prend pas en charge l’exécution de plusieurs opérations parallèles dans le même contexte.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="1d9b1-108">Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="1d9b1-109">Ceci est généralement fait `await` en utilisant le mot clé sur chaque opération async.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="1d9b1-110">Entity Framework Core fournit un ensemble de méthodes d’extension async similaires aux méthodes LINQ, qui exécutent une requête et des résultats de retour.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="1d9b1-111">Exemples `ToListAsync()` `ToArrayAsync()`incluent `SingleAsync()`, , .</span><span class="sxs-lookup"><span data-stu-id="1d9b1-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="1d9b1-112">Il n’existe pas de versions async de certains exploitants de LINQ, comme `Where(...)` ou `OrderBy(...)` parce que ces méthodes ne font qu’accumuler l’arbre d’expression de la LINQ et ne font pas exécuter la requête dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="1d9b1-113">Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="1d9b1-114">Cet espace de noms doit être importé pour que les méthodes soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
