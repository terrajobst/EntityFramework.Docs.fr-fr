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
# <a name="asynchronous-queries"></a><span data-ttu-id="4092e-102">Requêtes asynchrones</span><span class="sxs-lookup"><span data-stu-id="4092e-102">Asynchronous Queries</span></span>

<span data-ttu-id="4092e-103">Les requêtes asynchrones évitent de bloquer un thread pendant que la requête est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4092e-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="4092e-104">Cela peut être utile pour éviter le gel de l’interface utilisateur d’une application client lourde.</span><span class="sxs-lookup"><span data-stu-id="4092e-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="4092e-105">Les opérations asynchrones peuvent également augmenter le débit dans une application web, où le thread peut être libéré pour d’autres demandes de service lors de la fin de l’opération de base de données.</span><span class="sxs-lookup"><span data-stu-id="4092e-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="4092e-106">Pour plus d’informations sur la programmation asynchrone, consultez [Programmation asynchrone en C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="4092e-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="4092e-107">EF Core ne prend pas en charge les opérations parallèles multiples en cours d’exécution sur la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="4092e-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="4092e-108">Vous devez toujours attendre qu’opération se termine avant de commencer l’opération suivante.</span><span class="sxs-lookup"><span data-stu-id="4092e-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="4092e-109">Cela est généralement effectué à l’aide du mot-clé `await` sur chaque opération asynchrone.</span><span class="sxs-lookup"><span data-stu-id="4092e-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="4092e-110">Entity Framework Core fournit un ensemble de méthodes d’extension asynchrone qui peuvent être utilisées comme alternative pour les méthodes LINQ qui provoquent l’exécution d’une requête et le retour de résultats.</span><span class="sxs-lookup"><span data-stu-id="4092e-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="4092e-111">Par exemple, `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. Il n’existe pas de versions asynchrones des opérateurs LINQ comme `Where(...)`, `OrderBy(...)`, etc., car ces méthodes construisent uniquement l’arborescence de l’expression LINQ et n’entraînent pas l’exécution de la requête dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4092e-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="4092e-112">Les méthodes d’extension EF Core asynchrones sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4092e-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="4092e-113">Cet espace de noms doit être importé pour que les méthodes soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="4092e-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
