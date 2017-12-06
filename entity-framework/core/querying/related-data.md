---
title: "Chargement associées données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="6d1f8-102">Chargement de données connexes</span><span class="sxs-lookup"><span data-stu-id="6d1f8-102">Loading Related Data</span></span>

<span data-ttu-id="6d1f8-103">Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="6d1f8-104">Il existe trois modèles d’O/RM communs utilisés pour charger les données associées.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="6d1f8-105">**Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="6d1f8-106">**Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à une date ultérieure.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="6d1f8-107">**Chargement différé** signifie que les données associées en toute transparence sont chargées à partir de la base de données que lorsque la propriété de navigation est accessible.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="6d1f8-108">Chargement différé n’est pas encore possible avec EF de base.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="6d1f8-109">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="6d1f8-110">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="6d1f8-110">Eager loading</span></span>

<span data-ttu-id="6d1f8-111">Vous pouvez utiliser la `Include` méthode pour spécifier les données connexes à inclure dans les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="6d1f8-112">Dans l’exemple suivant, les blogs sont retournés dans les résultats auront leurs `Posts` propriété remplie avec les messages liés.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="6d1f8-113">Entity Framework Core va corriger automatiquement des propriétés de navigation à toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="6d1f8-114">Par conséquent, même si vous n’incluez pas explicitement les données pour une propriété de navigation, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="6d1f8-115">Vous pouvez inclure des données liées provenant de plusieurs relations dans une requête unique.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="6d1f8-116">Y compris plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="6d1f8-116">Including multiple levels</span></span>

<span data-ttu-id="6d1f8-117">Vous pouvez descendre, par le biais des relations à inclure plusieurs niveaux de données connexes à l’aide du `ThenInclude` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6d1f8-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="6d1f8-118">L’exemple suivant charge tous les blogs, leurs messages liés et l’auteur de chaque publication.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="6d1f8-119">Vous pouvez chaîner plusieurs appels à `ThenInclude` pour continuer, notamment les prochaines niveaux de données associées.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="6d1f8-120">Vous pouvez combiner tous de cette option pour inclure les données associées à partir de plusieurs niveaux et plusieurs racines dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="6d1f8-121">Voulez-vous inclure plusieurs entités associées pour l’une des entités qui est incluse.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="6d1f8-122">Par exemple, lors de l’interrogation `Blog`s, vous incluez `Posts` , puis à inclure à la fois le `Author` et `Tags` de la `Posts`.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="6d1f8-123">Pour ce faire, vous devez spécifier chaque inclure le chemin d’accès à partir de la racine.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="6d1f8-124">Par exemple, `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="6d1f8-125">Cela ne signifie pas que vous obtiendrez des jointures redondantes, dans la plupart des cas QU'EF consolide les jointures lors de la génération SQL.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="6d1f8-126">Ignoré inclut</span><span class="sxs-lookup"><span data-stu-id="6d1f8-126">Ignored includes</span></span>

<span data-ttu-id="6d1f8-127">Si vous modifiez la requête afin qu’elle retourne ne sont plus des instances de la requête a commencé avec le type d’entité, les opérateurs include sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="6d1f8-128">Dans l’exemple suivant, les opérateurs include sont basées sur les `Blog`, puis le `Select` opérateur est utilisé pour modifier la requête pour retourner un type anonyme.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="6d1f8-129">Dans ce cas, les opérateurs include n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="6d1f8-130">Par défaut, EF Core consigne un avertissement lorsque incluent les opérateurs sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="6d1f8-131">Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="6d1f8-132">Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré lever ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="6d1f8-133">Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="6d1f8-134">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="6d1f8-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="6d1f8-135">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="6d1f8-136">Vous pouvez charger explicitement une propriété de navigation via la `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="6d1f8-137">Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="6d1f8-138">Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core automatiquement définit les propriétés de navigation de l’entité qui vient d’être chargé pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargé pour faire référence à la entité qui vient d’être chargé.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="6d1f8-139">Interrogation des entités connexes</span><span class="sxs-lookup"><span data-stu-id="6d1f8-139">Querying related entities</span></span>

<span data-ttu-id="6d1f8-140">Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="6d1f8-141">Cela vous permet de vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="6d1f8-142">Vous pouvez également filtrer les entités associées sont chargées en mémoire.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="6d1f8-143">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="6d1f8-143">Lazy loading</span></span>

<span data-ttu-id="6d1f8-144">Chargement différé n’est pas encore pris en charge par EF Core.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="6d1f8-145">Vous pouvez afficher le [élément chargement différé notre backlog](https://github.com/aspnet/EntityFramework/issues/3797) pour effectuer le suivi de cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="6d1f8-146">Sérialisation et les données associées</span><span class="sxs-lookup"><span data-stu-id="6d1f8-146">Related data and serialization</span></span>

<span data-ttu-id="6d1f8-147">Étant donné que les propriétés de EF Core sera automatiquement correctives navigation, vous pouvez retrouver avec des cycles dans votre graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="6d1f8-148">Par exemple, le chargement d’un blog et il est lié billets aboutit à un objet de blog qui fait référence à une collection de publications.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="6d1f8-149">Chacune de ces publications aura une référence vers le blog.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="6d1f8-150">Certaines infrastructures de sérialisation n’autorisent pas ces cycles.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="6d1f8-151">Par exemple, Json.NET lève l’exception suivante si un cycle est prématurément.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="6d1f8-152">Newtonsoft.Json.JsonSerializationException : Self faisant référence à la boucle détectée pour la propriété « Blog » avec le type 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="6d1f8-153">Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il se trouve dans le graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="6d1f8-154">Cette opération est effectuée le `ConfigureServices(...)` méthode dans `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="6d1f8-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
