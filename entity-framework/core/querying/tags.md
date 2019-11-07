---
title: Balises de requête - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654829"
---
# <a name="query-tags"></a><span data-ttu-id="830bb-102">Balises de requête</span><span class="sxs-lookup"><span data-stu-id="830bb-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="830bb-103">Cette fonctionnalité est une nouveauté d’EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="830bb-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="830bb-104">Cette fonctionnalité simplifie la corrélation des requêtes LINQ dans le code avec des requêtes SQL générées et capturées dans des journaux.</span><span class="sxs-lookup"><span data-stu-id="830bb-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="830bb-105">Vous annotez une requête LINQ à l’aide de la nouvelle méthode `TagWith()` :</span><span class="sxs-lookup"><span data-stu-id="830bb-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="830bb-106">Cette requête LINQ est traduite dans l’instruction SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="830bb-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="830bb-107">Il est possible d’appeler `TagWith()` plusieurs fois sur la même requête.</span><span class="sxs-lookup"><span data-stu-id="830bb-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="830bb-108">Les balises de requête sont cumulatives.</span><span class="sxs-lookup"><span data-stu-id="830bb-108">Query tags are cumulative.</span></span>
<span data-ttu-id="830bb-109">Par exemple, avec les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="830bb-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="830bb-110">Cette requête :</span><span class="sxs-lookup"><span data-stu-id="830bb-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="830bb-111">Se traduit par :</span><span class="sxs-lookup"><span data-stu-id="830bb-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="830bb-112">Il est également possible d’utiliser des chaînes multilignes comme balises de requête.</span><span class="sxs-lookup"><span data-stu-id="830bb-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="830bb-113">Exemple :</span><span class="sxs-lookup"><span data-stu-id="830bb-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="830bb-114">Génère l’instruction SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="830bb-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="830bb-115">Limitations connues</span><span class="sxs-lookup"><span data-stu-id="830bb-115">Known limitations</span></span>

<span data-ttu-id="830bb-116">**Les balises de requête ne sont pas paramétrables :** EF Core traite toujours les balises de requête dans la requête LINQ comme des opérateurs sur chaîne inclus dans l’instruction SQL générée.</span><span class="sxs-lookup"><span data-stu-id="830bb-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="830bb-117">Les requêtes compilées qui utilisent des balises de requête comme paramètres ne sont pas autorisées.</span><span class="sxs-lookup"><span data-stu-id="830bb-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
