---
title: Chargement associées données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="4e66a-102">Chargement de données connexes</span><span class="sxs-lookup"><span data-stu-id="4e66a-102">Loading Related Data</span></span>

<span data-ttu-id="4e66a-103">Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées.</span><span class="sxs-lookup"><span data-stu-id="4e66a-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="4e66a-104">Il existe trois modèles d’O/RM communs utilisés pour charger les données associées.</span><span class="sxs-lookup"><span data-stu-id="4e66a-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="4e66a-105">**Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.</span><span class="sxs-lookup"><span data-stu-id="4e66a-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="4e66a-106">**Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à une date ultérieure.</span><span class="sxs-lookup"><span data-stu-id="4e66a-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="4e66a-107">**Chargement différé** signifie que les données associées en toute transparence sont chargées à partir de la base de données que lorsque la propriété de navigation est accessible.</span><span class="sxs-lookup"><span data-stu-id="4e66a-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="4e66a-108">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="4e66a-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="4e66a-109">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="4e66a-109">Eager loading</span></span>

<span data-ttu-id="4e66a-110">Vous pouvez utiliser la `Include` méthode pour spécifier les données connexes à inclure dans les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="4e66a-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="4e66a-111">Dans l’exemple suivant, les blogs sont retournés dans les résultats auront leurs `Posts` propriété remplie avec les messages liés.</span><span class="sxs-lookup"><span data-stu-id="4e66a-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="4e66a-112">Entity Framework Core va corriger automatiquement des propriétés de navigation à toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="4e66a-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="4e66a-113">Par conséquent, même si vous n’incluez pas explicitement les données pour une propriété de navigation, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="4e66a-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="4e66a-114">Vous pouvez inclure des données liées provenant de plusieurs relations dans une requête unique.</span><span class="sxs-lookup"><span data-stu-id="4e66a-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="4e66a-115">Y compris plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="4e66a-115">Including multiple levels</span></span>

<span data-ttu-id="4e66a-116">Vous pouvez descendre, par le biais des relations à inclure plusieurs niveaux de données connexes à l’aide du `ThenInclude` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4e66a-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="4e66a-117">L’exemple suivant charge tous les blogs, leurs messages liés et l’auteur de chaque publication.</span><span class="sxs-lookup"><span data-stu-id="4e66a-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="4e66a-118">Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrect et peut provoquer des expressions correctes marquage avec des erreurs de syntaxe lorsque vous utilisez la `ThenInclude` méthode après une propriété de navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="4e66a-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="4e66a-119">Ceci est le symptôme d’un bogue IntelliSense suivi à https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="4e66a-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="4e66a-120">Il est possible d’ignorer ces erreurs de syntaxe fausses tant que le code est correct et qu’il peut être compilé avec succès.</span><span class="sxs-lookup"><span data-stu-id="4e66a-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="4e66a-121">Vous pouvez chaîner plusieurs appels à `ThenInclude` pour continuer, notamment les prochaines niveaux de données associées.</span><span class="sxs-lookup"><span data-stu-id="4e66a-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="4e66a-122">Vous pouvez combiner tous de cette option pour inclure les données associées à partir de plusieurs niveaux et plusieurs racines dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="4e66a-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="4e66a-123">Voulez-vous inclure plusieurs entités associées pour l’une des entités qui est incluse.</span><span class="sxs-lookup"><span data-stu-id="4e66a-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="4e66a-124">Par exemple, lors de l’interrogation `Blog`s, vous incluez `Posts` , puis à inclure à la fois le `Author` et `Tags` de la `Posts`.</span><span class="sxs-lookup"><span data-stu-id="4e66a-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="4e66a-125">Pour ce faire, vous devez spécifier chaque inclure le chemin d’accès à partir de la racine.</span><span class="sxs-lookup"><span data-stu-id="4e66a-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="4e66a-126">Par exemple, `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="4e66a-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="4e66a-127">Cela ne signifie pas que vous obtiendrez des jointures redondantes, dans la plupart des cas QU'EF consolide les jointures lors de la génération SQL.</span><span class="sxs-lookup"><span data-stu-id="4e66a-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="4e66a-128">Inclure dans les types dérivés</span><span class="sxs-lookup"><span data-stu-id="4e66a-128">Include on derived types</span></span>

<span data-ttu-id="4e66a-129">Vous pouvez inclure des données liées provenant uniquement définies sur un type dérivé à l’aide de navigations `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="4e66a-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="4e66a-130">Étant donné le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="4e66a-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="4e66a-131">Contenu de `School` navigation de toutes les personnes qui sont les étudiants peut être chargée dynamiquement à l’aide d’un certain nombre de modèles :</span><span class="sxs-lookup"><span data-stu-id="4e66a-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="4e66a-132">utilisation de cast</span><span class="sxs-lookup"><span data-stu-id="4e66a-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="4e66a-133">à l’aide de `as` (opérateur)</span><span class="sxs-lookup"><span data-stu-id="4e66a-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="4e66a-134">à l’aide de la surcharge de `Include` qui accepte le paramètre de type `string`</span><span class="sxs-lookup"><span data-stu-id="4e66a-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="4e66a-135">Ignoré inclut</span><span class="sxs-lookup"><span data-stu-id="4e66a-135">Ignored includes</span></span>

<span data-ttu-id="4e66a-136">Si vous modifiez la requête afin qu’elle retourne ne sont plus des instances de la requête a commencé avec le type d’entité, les opérateurs include sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="4e66a-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="4e66a-137">Dans l’exemple suivant, les opérateurs include sont basées sur les `Blog`, puis le `Select` opérateur est utilisé pour modifier la requête pour retourner un type anonyme.</span><span class="sxs-lookup"><span data-stu-id="4e66a-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="4e66a-138">Dans ce cas, les opérateurs include n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="4e66a-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="4e66a-139">Par défaut, EF Core consigne un avertissement lorsque incluent les opérateurs sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="4e66a-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="4e66a-140">Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation.</span><span class="sxs-lookup"><span data-stu-id="4e66a-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="4e66a-141">Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré lever ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="4e66a-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="4e66a-142">Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e66a-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="4e66a-143">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="4e66a-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="4e66a-144">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4e66a-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="4e66a-145">Vous pouvez charger explicitement une propriété de navigation via la `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="4e66a-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="4e66a-146">Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées.</span><span class="sxs-lookup"><span data-stu-id="4e66a-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="4e66a-147">Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core automatiquement définit les propriétés de navigation de l’entité qui vient d’être chargé pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargé pour faire référence à la entité qui vient d’être chargé.</span><span class="sxs-lookup"><span data-stu-id="4e66a-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="4e66a-148">Interrogation des entités connexes</span><span class="sxs-lookup"><span data-stu-id="4e66a-148">Querying related entities</span></span>

<span data-ttu-id="4e66a-149">Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="4e66a-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="4e66a-150">Cela vous permet de vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="4e66a-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="4e66a-151">Vous pouvez également filtrer les entités associées sont chargées en mémoire.</span><span class="sxs-lookup"><span data-stu-id="4e66a-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="4e66a-152">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="4e66a-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="4e66a-153">Cette fonctionnalité a été introduite dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4e66a-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="4e66a-154">La façon la plus simple d’utiliser le chargement différé est en installant le [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package et l’activer avec un appel à `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="4e66a-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="4e66a-155">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4e66a-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="4e66a-156">Ou bien, lors de l’utilisation de AddDbContext :</span><span class="sxs-lookup"><span data-stu-id="4e66a-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="4e66a-157">EF Core ensuite d’activer le chargement différé pour n’importe quelle propriété de navigation qui peut être substituée--qui est, elle doit être `virtual` et sur une classe qui peut être héritée.</span><span class="sxs-lookup"><span data-stu-id="4e66a-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="4e66a-158">Par exemple, dans les entités suivantes, le `Post.Blog` et `Blog.Posts` propriétés de navigation seront chargées en différé.</span><span class="sxs-lookup"><span data-stu-id="4e66a-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="4e66a-159">Lazy-chargement sans proxy</span><span class="sxs-lookup"><span data-stu-id="4e66a-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="4e66a-160">Chargement Lazy proxys fonctionnent en injectant le `ILazyLoader` de service dans une entité, comme décrit dans [constructeurs de Type d’entité](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="4e66a-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="4e66a-161">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4e66a-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="4e66a-162">Cela ne nécessite pas hérité de types d’entité ou de propriétés de navigation à être des ordinateurs virtuels et permet aux instances d’entité créées avec `new` charger en différé-une fois attaché à un contexte.</span><span class="sxs-lookup"><span data-stu-id="4e66a-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="4e66a-163">Toutefois, il requiert une référence à le `ILazyLoader` service, qui combine des types d’entités à l’assembly EF Core.</span><span class="sxs-lookup"><span data-stu-id="4e66a-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="4e66a-164">Pour éviter ce cœur EF permet la `ILazyLoader.Load` méthode être ajoutés en tant que délégué.</span><span class="sxs-lookup"><span data-stu-id="4e66a-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="4e66a-165">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="4e66a-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="4e66a-166">Le code ci-dessus utilise un `Load` pour rendre l’utilisation du délégué un peu plus propre méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="4e66a-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="4e66a-167">Le paramètre de constructeur pour le chargement différé de délégué doit être appelé « lazyLoader ».</span><span class="sxs-lookup"><span data-stu-id="4e66a-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="4e66a-168">Configuration à utiliser un nom différent, qu'il est prévu pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="4e66a-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="4e66a-169">Sérialisation et les données associées</span><span class="sxs-lookup"><span data-stu-id="4e66a-169">Related data and serialization</span></span>

<span data-ttu-id="4e66a-170">Étant donné que les propriétés de EF Core sera automatiquement correctives navigation, vous pouvez retrouver avec des cycles dans votre graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="4e66a-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="4e66a-171">Par exemple, le chargement d’un blog et il est lié billets aboutit à un objet de blog qui fait référence à une collection de publications.</span><span class="sxs-lookup"><span data-stu-id="4e66a-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="4e66a-172">Chacune de ces publications aura une référence vers le blog.</span><span class="sxs-lookup"><span data-stu-id="4e66a-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="4e66a-173">Certaines infrastructures de sérialisation n’autorisent pas ces cycles.</span><span class="sxs-lookup"><span data-stu-id="4e66a-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="4e66a-174">Par exemple, Json.NET lève l’exception suivante si un cycle est rencontré.</span><span class="sxs-lookup"><span data-stu-id="4e66a-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="4e66a-175">Newtonsoft.Json.JsonSerializationException : Self faisant référence à la boucle détectée pour la propriété « Blog » avec le type 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="4e66a-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="4e66a-176">Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il se trouve dans le graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="4e66a-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="4e66a-177">Cette opération est effectuée le `ConfigureServices(...)` méthode dans `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="4e66a-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
