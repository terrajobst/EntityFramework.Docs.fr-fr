---
title: Fournisseur de Azure Cosmos DB-limitations-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150808"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="d1a17-102">Limitations des fournisseurs de EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d1a17-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="d1a17-103">Le fournisseur Cosmos présente un certain nombre de limitations.</span><span class="sxs-lookup"><span data-stu-id="d1a17-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="d1a17-104">La plupart de ces limitations résultent de limitations dans le moteur de base de données Cosmos sous-jacent et ne sont pas spécifiques à EF.</span><span class="sxs-lookup"><span data-stu-id="d1a17-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="d1a17-105">Mais la plupart [n’ont pas encore été implémentées](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="d1a17-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="d1a17-106">Limitations temporaires</span><span class="sxs-lookup"><span data-stu-id="d1a17-106">Temporary limitations</span></span>

- <span data-ttu-id="d1a17-107">Même s’il existe un seul type d’entité sans héritage mappé à un conteneur, il possède toujours une propriété de discriminateur.</span><span class="sxs-lookup"><span data-stu-id="d1a17-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="d1a17-108">Les types d’entités avec des clés de partition ne fonctionnent pas correctement dans certains scénarios</span><span class="sxs-lookup"><span data-stu-id="d1a17-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="d1a17-109">`Include`les appels ne sont pas pris en charge</span><span class="sxs-lookup"><span data-stu-id="d1a17-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="d1a17-110">`Join`les appels ne sont pas pris en charge</span><span class="sxs-lookup"><span data-stu-id="d1a17-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="d1a17-111">Limitations du kit de développement logiciel Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d1a17-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="d1a17-112">Seules les méthodes Async sont fournies</span><span class="sxs-lookup"><span data-stu-id="d1a17-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="d1a17-113">Étant donné qu’il n’existe aucune version de synchronisation des méthodes de bas niveau EF Core s’appuie sur, la fonctionnalité correspondante est `.Wait()` actuellement implémentée `Task`en appelant sur le retourné.</span><span class="sxs-lookup"><span data-stu-id="d1a17-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="d1a17-114">Cela signifie que l’utilisation de `SaveChanges`méthodes telles `ToList` que, ou à la place de leurs équivalents Async, peut entraîner un blocage dans votre application</span><span class="sxs-lookup"><span data-stu-id="d1a17-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="d1a17-115">Limitations de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d1a17-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="d1a17-116">Vous pouvez voir la vue d’ensemble complète des [fonctionnalités prises en charge par Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data). il s’agit des différences les plus notables par rapport à une base de données relationnelle :</span><span class="sxs-lookup"><span data-stu-id="d1a17-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="d1a17-117">Les transactions initiées par le client ne sont pas prises en charge</span><span class="sxs-lookup"><span data-stu-id="d1a17-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="d1a17-118">Certaines requêtes de plusieurs partitions ne sont pas prises en charge ou sont beaucoup plus lentes en fonction des opérateurs impliqués</span><span class="sxs-lookup"><span data-stu-id="d1a17-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>