---
title: "Suivi de Visual Studio. Aucun suivi des requêtes - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="9ae54-102">Suivi de Visual Studio. Aucun suivi des requêtes</span><span class="sxs-lookup"><span data-stu-id="9ae54-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="9ae54-103">Contrôles de comportement de suivi ou non de Entity Framework Core conserve les informations relatives à une instance d’entité dans sa traceur de modifications.</span><span class="sxs-lookup"><span data-stu-id="9ae54-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="9ae54-104">Si une entité est suivie, toutes les modifications détectées dans l’entité sont rendues persistantes dans la base de données au cours de `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9ae54-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="9ae54-105">Entity Framework Core sera correctif également des propriétés de navigation entre les entités qui sont obtenues à partir d’une requête de suivi et des entités qui ont été précédemment chargées dans l’instance de DbContext.</span><span class="sxs-lookup"><span data-stu-id="9ae54-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="9ae54-106">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="9ae54-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="9ae54-107">Requêtes de suivi</span><span class="sxs-lookup"><span data-stu-id="9ae54-107">Tracking queries</span></span>

<span data-ttu-id="9ae54-108">Par défaut, suivez les requêtes qui retournent des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="9ae54-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="9ae54-109">Cela signifie que vous pouvez apporter des modifications à des instances d’entité et ont ces modifications rendues persistantes par `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9ae54-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="9ae54-110">Dans l’exemple suivant, la modification de l’évaluation de blogs sera détectée et rendues persistantes dans la base de données au cours de `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9ae54-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="9ae54-111">Aucun suivi des requêtes</span><span class="sxs-lookup"><span data-stu-id="9ae54-111">No-tracking queries</span></span>

<span data-ttu-id="9ae54-112">Aucune requête de suivi n’est utiles lorsque les résultats sont utilisés dans un scénario en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="9ae54-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="9ae54-113">Ils sont plus rapides à exécuter, car il est inutile de modifier le programme d’installation des informations de suivi.</span><span class="sxs-lookup"><span data-stu-id="9ae54-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="9ae54-114">Vous pouvez permuter une requête individuelle pour être non suivi :</span><span class="sxs-lookup"><span data-stu-id="9ae54-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="9ae54-115">Vous pouvez également modifier le comportement au niveau de l’instance de contexte de suivi par défaut :</span><span class="sxs-lookup"><span data-stu-id="9ae54-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="9ae54-116">Aucune requête de suivi toujours n’effectuer la résolution d’identité dans la requête de la cause.</span><span class="sxs-lookup"><span data-stu-id="9ae54-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="9ae54-117">Si le jeu de résultats contient plusieurs fois à la même entité, la même instance de la classe d’entité s’affichera pour chaque occurrence du jeu de résultats.</span><span class="sxs-lookup"><span data-stu-id="9ae54-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="9ae54-118">Toutefois, les références faibles sont utilisés pour effectuer le suivi des entités qui ont déjà été retournées.</span><span class="sxs-lookup"><span data-stu-id="9ae54-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="9ae54-119">Si un résultat antérieur avec la même identité est hors de portée, et le garbage collection s’exécute, vous risquez d’obtenir une nouvelle instance de l’entité.</span><span class="sxs-lookup"><span data-stu-id="9ae54-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="9ae54-120">Pour plus d’informations, consultez [requête fonctionnement](overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ae54-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="9ae54-121">Le suivi et les projections</span><span class="sxs-lookup"><span data-stu-id="9ae54-121">Tracking and projections</span></span>

<span data-ttu-id="9ae54-122">Même si le type de résultat de la requête n’est pas un type d’entité, si le résultat contient des types d’entités qu’ils sont toujours suivies par défaut.</span><span class="sxs-lookup"><span data-stu-id="9ae54-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="9ae54-123">Dans la requête suivante, qui retourne un type anonyme, les instances de `Blog` dans le résultat de suivi ensemble.</span><span class="sxs-lookup"><span data-stu-id="9ae54-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="9ae54-124">Si le jeu de résultats ne contient pas tous les types d’entité, aucun suivi n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="9ae54-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="9ae54-125">Dans la requête suivante, qui retourne un type anonyme avec certaines des valeurs de l’entité (mais aucune instance du type d’entité réel), il n’existe aucun suivi effectuées.</span><span class="sxs-lookup"><span data-stu-id="9ae54-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
