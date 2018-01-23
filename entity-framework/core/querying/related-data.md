---
title: "Chargement associées données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="4ce87-102">Chargement de données connexes</span><span class="sxs-lookup"><span data-stu-id="4ce87-102">Loading Related Data</span></span>

<span data-ttu-id="4ce87-103">Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées.</span><span class="sxs-lookup"><span data-stu-id="4ce87-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="4ce87-104">Il existe trois modèles d’O/RM communs utilisés pour charger les données associées.</span><span class="sxs-lookup"><span data-stu-id="4ce87-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="4ce87-105">**Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.</span><span class="sxs-lookup"><span data-stu-id="4ce87-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="4ce87-106">**Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à une date ultérieure.</span><span class="sxs-lookup"><span data-stu-id="4ce87-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="4ce87-107">**Chargement différé** signifie que les données associées en toute transparence sont chargées à partir de la base de données que lorsque la propriété de navigation est accessible.</span><span class="sxs-lookup"><span data-stu-id="4ce87-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="4ce87-108">Chargement différé n’est pas encore possible avec EF de base.</span><span class="sxs-lookup"><span data-stu-id="4ce87-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="4ce87-109">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="4ce87-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="4ce87-110">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="4ce87-110">Eager loading</span></span>

<span data-ttu-id="4ce87-111">Vous pouvez utiliser la `Include` méthode pour spécifier les données connexes à inclure dans les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="4ce87-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="4ce87-112">Dans l’exemple suivant, les blogs sont retournés dans les résultats auront leurs `Posts` propriété remplie avec les messages liés.</span><span class="sxs-lookup"><span data-stu-id="4ce87-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="4ce87-113">Entity Framework Core va corriger automatiquement des propriétés de navigation à toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="4ce87-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="4ce87-114">Par conséquent, même si vous n’incluez pas explicitement les données pour une propriété de navigation, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="4ce87-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="4ce87-115">Vous pouvez inclure des données liées provenant de plusieurs relations dans une requête unique.</span><span class="sxs-lookup"><span data-stu-id="4ce87-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="4ce87-116">Y compris plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="4ce87-116">Including multiple levels</span></span>

<span data-ttu-id="4ce87-117">Vous pouvez descendre, par le biais des relations à inclure plusieurs niveaux de données connexes à l’aide du `ThenInclude` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4ce87-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="4ce87-118">L’exemple suivant charge tous les blogs, leurs messages liés et l’auteur de chaque publication.</span><span class="sxs-lookup"><span data-stu-id="4ce87-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="4ce87-119">Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrect et peut provoquer des expressions correctes marquage avec des erreurs de syntaxe lorsque vous utilisez la `ThenInclude` méthode après une propriété de navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="4ce87-119">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="4ce87-120">Ceci est le symptôme d’un bogue IntelliSense suivi à https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="4ce87-120">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="4ce87-121">Il est possible d’ignorer ces erreurs de syntaxe fausses tant que le code est correct et qu’il peut être compilé avec succès.</span><span class="sxs-lookup"><span data-stu-id="4ce87-121">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="4ce87-122">Vous pouvez chaîner plusieurs appels à `ThenInclude` pour continuer, notamment les prochaines niveaux de données associées.</span><span class="sxs-lookup"><span data-stu-id="4ce87-122">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="4ce87-123">Vous pouvez combiner tous de cette option pour inclure les données associées à partir de plusieurs niveaux et plusieurs racines dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="4ce87-123">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="4ce87-124">Voulez-vous inclure plusieurs entités associées pour l’une des entités qui est incluse.</span><span class="sxs-lookup"><span data-stu-id="4ce87-124">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="4ce87-125">Par exemple, lors de l’interrogation `Blog`s, vous incluez `Posts` , puis à inclure à la fois le `Author` et `Tags` de la `Posts`.</span><span class="sxs-lookup"><span data-stu-id="4ce87-125">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="4ce87-126">Pour ce faire, vous devez spécifier chaque inclure le chemin d’accès à partir de la racine.</span><span class="sxs-lookup"><span data-stu-id="4ce87-126">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="4ce87-127">Par exemple, `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="4ce87-127">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="4ce87-128">Cela ne signifie pas que vous obtiendrez des jointures redondantes, dans la plupart des cas QU'EF consolide les jointures lors de la génération SQL.</span><span class="sxs-lookup"><span data-stu-id="4ce87-128">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="4ce87-129">Ignoré inclut</span><span class="sxs-lookup"><span data-stu-id="4ce87-129">Ignored includes</span></span>

<span data-ttu-id="4ce87-130">Si vous modifiez la requête afin qu’elle retourne ne sont plus des instances de la requête a commencé avec le type d’entité, les opérateurs include sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="4ce87-130">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="4ce87-131">Dans l’exemple suivant, les opérateurs include sont basées sur les `Blog`, puis le `Select` opérateur est utilisé pour modifier la requête pour retourner un type anonyme.</span><span class="sxs-lookup"><span data-stu-id="4ce87-131">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="4ce87-132">Dans ce cas, les opérateurs include n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="4ce87-132">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="4ce87-133">Par défaut, EF Core consigne un avertissement lorsque incluent les opérateurs sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="4ce87-133">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="4ce87-134">Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation.</span><span class="sxs-lookup"><span data-stu-id="4ce87-134">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="4ce87-135">Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré lever ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="4ce87-135">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="4ce87-136">Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ce87-136">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="4ce87-137">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="4ce87-137">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="4ce87-138">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4ce87-138">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="4ce87-139">Vous pouvez charger explicitement une propriété de navigation via la `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="4ce87-139">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="4ce87-140">Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées.</span><span class="sxs-lookup"><span data-stu-id="4ce87-140">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="4ce87-141">Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core automatiquement définit les propriétés de navigation de l’entité qui vient d’être chargé pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargé pour faire référence à la entité qui vient d’être chargé.</span><span class="sxs-lookup"><span data-stu-id="4ce87-141">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="4ce87-142">Interrogation des entités connexes</span><span class="sxs-lookup"><span data-stu-id="4ce87-142">Querying related entities</span></span>

<span data-ttu-id="4ce87-143">Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="4ce87-143">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="4ce87-144">Cela vous permet de vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="4ce87-144">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="4ce87-145">Vous pouvez également filtrer les entités associées sont chargées en mémoire.</span><span class="sxs-lookup"><span data-stu-id="4ce87-145">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="4ce87-146">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="4ce87-146">Lazy loading</span></span>

<span data-ttu-id="4ce87-147">Chargement différé n’est pas encore pris en charge par EF Core.</span><span class="sxs-lookup"><span data-stu-id="4ce87-147">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="4ce87-148">Vous pouvez afficher le [élément chargement différé notre backlog](https://github.com/aspnet/EntityFramework/issues/3797) pour effectuer le suivi de cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="4ce87-148">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="4ce87-149">Sérialisation et les données associées</span><span class="sxs-lookup"><span data-stu-id="4ce87-149">Related data and serialization</span></span>

<span data-ttu-id="4ce87-150">Étant donné que les propriétés de EF Core sera automatiquement correctives navigation, vous pouvez retrouver avec des cycles dans votre graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="4ce87-150">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="4ce87-151">Par exemple, le chargement d’un blog et il est lié billets aboutit à un objet de blog qui fait référence à une collection de publications.</span><span class="sxs-lookup"><span data-stu-id="4ce87-151">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="4ce87-152">Chacune de ces publications aura une référence vers le blog.</span><span class="sxs-lookup"><span data-stu-id="4ce87-152">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="4ce87-153">Certaines infrastructures de sérialisation n’autorisent pas ces cycles.</span><span class="sxs-lookup"><span data-stu-id="4ce87-153">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="4ce87-154">Par exemple, Json.NET lève l’exception suivante si un cycle est prématurément.</span><span class="sxs-lookup"><span data-stu-id="4ce87-154">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="4ce87-155">Newtonsoft.Json.JsonSerializationException : Self faisant référence à la boucle détectée pour la propriété « Blog » avec le type 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="4ce87-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="4ce87-156">Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il se trouve dans le graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="4ce87-156">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="4ce87-157">Cette opération est effectuée le `ConfigureServices(...)` méthode dans `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="4ce87-157">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
