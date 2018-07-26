---
title: Le chargement d’entités - EF6 associées
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121146"
---
# <a name="loading-related-entities"></a><span data-ttu-id="6f5a7-102">Chargement des entités connexes</span><span class="sxs-lookup"><span data-stu-id="6f5a7-102">Loading Related Entities</span></span>
<span data-ttu-id="6f5a7-103">Entity Framework prend en charge les trois façons de charger les données associées - chargement hâtif, le chargement différé et le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="6f5a7-104">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="6f5a7-105">Chargement de manière anticipée</span><span class="sxs-lookup"><span data-stu-id="6f5a7-105">Eagerly Loading</span></span>  

<span data-ttu-id="6f5a7-106">Le chargement hâtif est le processus par lequel une requête pour un type d’entité charge également les entités associées dans le cadre de la requête.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="6f5a7-107">Le chargement hâtif est obtenu à l’aide de la méthode Include.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="6f5a7-108">Par exemple, les requêtes ci-dessous chargera les blogs et toutes les publications relatives à chaque blog.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="6f5a7-109">Notez que Include est une méthode d’extension dans l’espace de noms System.Data.Entity Veillez donc à l’aide de cet espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="6f5a7-110">Chargement de manière anticipée plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="6f5a7-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="6f5a7-111">Il est également possible de charger plusieurs niveaux d’entités associées de manière anticipée.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="6f5a7-112">Les requêtes ci-dessous illustrent comment effectuer cette opération pour la collecte et de propriétés de navigation de référence.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="6f5a7-113">Notez qu’il n’est pas encore possible de filtrer les entités associées sont chargées.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="6f5a7-114">Inclure sera toujours apporter dans toutes les entités.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="6f5a7-115">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="6f5a7-115">Lazy Loading</span></span>  

<span data-ttu-id="6f5a7-116">Le chargement différé est le processus par lequel une entité ou une collection d’entités est automatiquement chargée à partir de la base de données la première fois que l’accès à une propriété qui fait référence à l’entité/entités.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="6f5a7-117">Lorsque vous utilisez des types d’entités POCO, le chargement différé est obtenu en créant des instances de types de proxy dérivée et puis en substituant les propriétés virtuelles pour ajouter le raccordement de chargement.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="6f5a7-118">Par exemple, lorsque vous utilisez la classe d’entité Blog définie ci-dessous, les messages liés seront chargées la première fois que la propriété de navigation de billets est accessible :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="6f5a7-119">Activer le chargement différé désactivé pour sérialisation</span><span class="sxs-lookup"><span data-stu-id="6f5a7-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="6f5a7-120">Sérialisation et le chargement différé ne mélangez pas correctement, et si vous ne faites pas attention vous pouvez retrouver d’interrogation pour votre base de données entière juste, car le chargement différé est activé.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="6f5a7-121">La plupart des sérialiseurs fonctionnent en accédant à chaque propriété sur une instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="6f5a7-122">Accès à la propriété déclenche le chargement différé, donc plusieurs entités sont sérialisées.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="6f5a7-123">Sur ces entités les propriétés sont accessibles, et d’autres entités sont chargées.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="6f5a7-124">Il est conseillé d’activer le chargement désactivé avant de vous sérialisez une entité différé.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="6f5a7-125">Les sections suivantes montrent comment effectuer cette opération.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="6f5a7-126">Désactivation de chargement différé pour les propriétés de navigation spécifique</span><span class="sxs-lookup"><span data-stu-id="6f5a7-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="6f5a7-127">Le chargement différé de la collection de billets peut être désactivé en rendant la propriété de billets non virtuelle :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="6f5a7-128">Le chargement des billets de collection encore possible à l’aide d’un chargement hâtif (consultez *chargement de manière anticipée* ci-dessus) ou la méthode Load (consultez *chargement explicite* ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="6f5a7-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="6f5a7-129">Désactivez chargement différé pour toutes les entités</span><span class="sxs-lookup"><span data-stu-id="6f5a7-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="6f5a7-130">Le chargement différé peut être désactivé pour toutes les entités dans le contexte en définissant un indicateur sur la propriété de Configuration.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="6f5a7-131">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="6f5a7-132">Chargement des entités connexes encore possible à l’aide d’un chargement hâtif (consultez *chargement de manière anticipée* ci-dessus) ou la méthode Load (consultez *chargement explicite* ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="6f5a7-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="6f5a7-133">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="6f5a7-133">Explicitly Loading</span></span>  

<span data-ttu-id="6f5a7-134">Même lorsque le chargement différé désactivé, il est toujours possible de charger en différé des entités associées, mais elle doit être effectuée avec un appel explicite.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="6f5a7-135">Pour ce faire, vous utilisez la méthode de charge sur l’entrée de l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="6f5a7-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="6f5a7-137">Notez que la méthode de référence doit être utilisée lorsqu’une entité a une propriété de navigation à une autre entité unique.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="6f5a7-138">En revanche, la méthode de collecte doit être utilisée lorsqu’une entité a une propriété de navigation à une collection d’autres entités.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="6f5a7-139">Appliquer des filtres lors du chargement explicite d’entités associées</span><span class="sxs-lookup"><span data-stu-id="6f5a7-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="6f5a7-140">La méthode de requête fournit l’accès à la requête sous-jacente qui utilise Entity Framework lors du chargement des entités associées.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="6f5a7-141">Vous pouvez ensuite utiliser LINQ pour appliquer des filtres à la requête avant de l’exécuter avec un appel à une méthode d’extension LINQ comme ToList, charge, etc. La méthode de requête peut être utilisée avec les propriétés de navigation de référence et de collection, mais est particulièrement utile pour les collections, où il peut être utilisé pour charger uniquement une partie de la collection.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="6f5a7-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="6f5a7-143">Lorsque vous utilisez la méthode de requête, il est généralement préférable désactiver le chargement différé pour la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="6f5a7-144">Il s’agit, car sinon la collection entière peut sont chargée automatiquement par le mécanisme de chargement différé avant ou après l’exécution de la requête filtrée.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="6f5a7-145">Notez que lors de la relation peut être spécifiée sous forme de chaîne au lieu d’une expression lambda, IQueryable retournée n’est pas générique lorsqu’une chaîne est utilisée et la méthode de conversion n’est donc généralement nécessaire avant que quelque chose d’utile peut être fait avec elle.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="6f5a7-146">À l’aide de la requête pour compter les entités associées sans leur chargement</span><span class="sxs-lookup"><span data-stu-id="6f5a7-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="6f5a7-147">Il est parfois utile de connaître le nombre d’entités est lié à une autre entité dans la base de données sans réellement impliquer le coût du chargement de toutes ces entités.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="6f5a7-148">La méthode de requête avec la méthode Count de LINQ peut être utilisée pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="6f5a7-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="6f5a7-149">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6f5a7-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  