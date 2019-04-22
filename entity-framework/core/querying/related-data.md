---
title: 'Chargement des données associées : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: e98e2e601203db7ea3d3344ddc7b7e0aff7f2143
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562518"
---
# <a name="loading-related-data"></a><span data-ttu-id="b9d55-102">Chargement des données associées</span><span class="sxs-lookup"><span data-stu-id="b9d55-102">Loading Related Data</span></span>

<span data-ttu-id="b9d55-103">Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées.</span><span class="sxs-lookup"><span data-stu-id="b9d55-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="b9d55-104">Il existe trois modèles O/RM communs utilisés pour charger les données associées.</span><span class="sxs-lookup"><span data-stu-id="b9d55-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="b9d55-105">Le **Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.</span><span class="sxs-lookup"><span data-stu-id="b9d55-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="b9d55-106">Le **Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à un moment ultérieur.</span><span class="sxs-lookup"><span data-stu-id="b9d55-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="b9d55-107">Le **Chargement différé** signifie que les données associées sont chargées de façon transparente à partir de la base de données lors de l’accès à la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="b9d55-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="b9d55-108">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="b9d55-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="b9d55-109">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="b9d55-109">Eager loading</span></span>

<span data-ttu-id="b9d55-110">Vous pouvez utiliser la méthode `Include` pour spécifier les données associées à inclure dans les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="b9d55-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="b9d55-111">Dans l’exemple suivant, les blogs retournés dans les résultats auront leurs propriétés `Posts` remplies avec les billets associés.</span><span class="sxs-lookup"><span data-stu-id="b9d55-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="b9d55-112">Entity Framework Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="b9d55-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="b9d55-113">Ainsi, même vous n’incluez pas explicitement si les données pour une propriété de navigation, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="b9d55-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="b9d55-114">Vous pouvez inclure des données associées provenant de plusieurs relations dans une seule requête.</span><span class="sxs-lookup"><span data-stu-id="b9d55-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="b9d55-115">Inclusion de plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="b9d55-115">Including multiple levels</span></span>

<span data-ttu-id="b9d55-116">Vous pouvez descendre dans la hiérarchie des relations pour inclure plusieurs niveaux de données associées à l’aide de la méthode `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="b9d55-117">L’exemple suivant charge tous les blogs, leurs messages associés et l’auteur de chaque publication.</span><span class="sxs-lookup"><span data-stu-id="b9d55-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="b9d55-118">Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrectes et peuvent provoquer le marquage d’expressions correctes avec des erreurs de syntaxe lorsque vous utilisez la méthode `ThenInclude` après une propriété de navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="b9d55-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="b9d55-119">Cela est causé par un bogue d’IntelliSense suivi sur https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="b9d55-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="b9d55-120">Il est possible d’ignorer ces fausses erreurs de syntaxe tant que le code est correct et qu’il peut être compilé avec succès.</span><span class="sxs-lookup"><span data-stu-id="b9d55-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="b9d55-121">Vous pouvez enchaîner plusieurs appels à `ThenInclude` pour continuer à inclure les niveaux de données associées suivants.</span><span class="sxs-lookup"><span data-stu-id="b9d55-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="b9d55-122">Vous pouvez combiner tout cela pour inclure les données associées de plusieurs niveaux et plusieurs racines dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="b9d55-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="b9d55-123">Vous pourriez souhaiter inclure plusieurs entités associées pour une entité qui est incluse.</span><span class="sxs-lookup"><span data-stu-id="b9d55-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="b9d55-124">Par exemple, quand vous interrogez des `Blogs`, vous incluez `Posts`, puis souhaitez inclure à la fois les `Author` et les `Tags` des `Posts`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="b9d55-125">Pour ce faire, vous devez spécifier chaque chemin d’accès à inclure à partir de la racine.</span><span class="sxs-lookup"><span data-stu-id="b9d55-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="b9d55-126">Par exemple : `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="b9d55-127">Cela ne signifie pas que vous obtiendrez des jointures redondantes. Dans la plupart des cas, EF consolide les jointures lors de la génération du SQL.</span><span class="sxs-lookup"><span data-stu-id="b9d55-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="b9d55-128">Inclure des types dérivés</span><span class="sxs-lookup"><span data-stu-id="b9d55-128">Include on derived types</span></span>

<span data-ttu-id="b9d55-129">Vous pouvez inclure des données associées provenant de navigations définies uniquement sur un type dérivé à l’aide de `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="b9d55-130">En partant du modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="b9d55-130">Given the following model:</span></span>

```csharp
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

<span data-ttu-id="b9d55-131">Le contenu de la navigation `School` de toutes les personnes qui sont des étudiants peut être chargé dynamiquement à l’aide d’un certain nombre de modèles :</span><span class="sxs-lookup"><span data-stu-id="b9d55-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="b9d55-132">utilisation du forçage de type</span><span class="sxs-lookup"><span data-stu-id="b9d55-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="b9d55-133">utilisation de l’opérateur `as`</span><span class="sxs-lookup"><span data-stu-id="b9d55-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="b9d55-134">utilisation de la surcharge de `Include` qui accepte les paramètres de type `string`</span><span class="sxs-lookup"><span data-stu-id="b9d55-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="b9d55-135">Inclusions ignorées</span><span class="sxs-lookup"><span data-stu-id="b9d55-135">Ignored includes</span></span>

<span data-ttu-id="b9d55-136">Si vous modifiez la requête afin qu’elle ne renvoie plus les instances du type d’entité par lequel la requête a commencé, les opérateurs include sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="b9d55-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="b9d55-137">Dans l’exemple suivant, les opérateurs include sont basés sur le `Blog`, puis l’opérateur `Select` est utilisé pour modifier la requête pour retourner un type anonyme.</span><span class="sxs-lookup"><span data-stu-id="b9d55-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="b9d55-138">Dans ce cas, les opérateurs include n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="b9d55-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="b9d55-139">Par défaut, EF Core consigne un avertissement lorsque les opérateurs include sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="b9d55-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="b9d55-140">Consultez [Journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de la journalisation.</span><span class="sxs-lookup"><span data-stu-id="b9d55-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="b9d55-141">Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré, entre journaliser ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="b9d55-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="b9d55-142">Cette opération est effectuée lors de la définition des options pour votre contexte, généralement dans `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9d55-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="b9d55-143">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="b9d55-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="b9d55-144">Cette fonctionnalité a été introduite dans EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="b9d55-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="b9d55-145">Vous pouvez charger explicitement une propriété de navigation via l’API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="b9d55-146">Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées.</span><span class="sxs-lookup"><span data-stu-id="b9d55-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="b9d55-147">Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core définit automatiquement les propriétés de navigation de l’entité qui vient d’être chargée pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargées pour faire référence à l’entité qui vient d’être chargée.</span><span class="sxs-lookup"><span data-stu-id="b9d55-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="b9d55-148">Interrogation des entités associées</span><span class="sxs-lookup"><span data-stu-id="b9d55-148">Querying related entities</span></span>

<span data-ttu-id="b9d55-149">Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="b9d55-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="b9d55-150">Cela vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="b9d55-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="b9d55-151">Vous pouvez également filtrer les entités associées qui sont chargées en mémoire.</span><span class="sxs-lookup"><span data-stu-id="b9d55-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="b9d55-152">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="b9d55-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="b9d55-153">Cette fonctionnalité a été introduite dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b9d55-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="b9d55-154">La façon la plus simple d’utiliser le chargement différé est d’installer le package [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) et de l’activer avec un appel à `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="b9d55-155">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9d55-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="b9d55-156">Ou bien, lors de l’utilisation d’AddDbContext :</span><span class="sxs-lookup"><span data-stu-id="b9d55-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="b9d55-157">EF Core active ensuite le chargement différé pour n’importe quelle propriété de navigation qui peut être substituée, c’est-à-dire qui doit être `virtual` et sur une classe qui peut être héritée.</span><span class="sxs-lookup"><span data-stu-id="b9d55-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="b9d55-158">Par exemple, dans les entités suivantes, les propriétés de navigation `Post.Blog` et `Blog.Posts` seront chargées en différé.</span><span class="sxs-lookup"><span data-stu-id="b9d55-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="b9d55-159">Chargement différé sans proxy</span><span class="sxs-lookup"><span data-stu-id="b9d55-159">Lazy loading without proxies</span></span>

<span data-ttu-id="b9d55-160">Les proxys à chargement différé fonctionnent en injectant le service `ILazyLoader` dans une entité, comme décrit dans [Constructeurs de type d’entité](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="b9d55-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="b9d55-161">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9d55-161">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="b9d55-162">Il n’est pas nécessaire que l’héritage des types d’entité à partir des propriétés de navigation soit virtuel, et cela permet aux instances d’entité créées avec `new` d’être chargées en différé une fois jointes à un contexte.</span><span class="sxs-lookup"><span data-stu-id="b9d55-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="b9d55-163">Toutefois, il requiert une référence au service `ILazyLoader`, qui est défini dans le package [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="b9d55-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="b9d55-164">Ce package contient un ensemble minimal de types, de sorte que dépendre de celui-ci n’a qu’un impact très limité.</span><span class="sxs-lookup"><span data-stu-id="b9d55-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="b9d55-165">Toutefois, pour éviter complètement de dépendre des packages EF Core dans les types d’entité, il est possible d’injecter la méthode `ILazyLoader.Load` en tant que délégué.</span><span class="sxs-lookup"><span data-stu-id="b9d55-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="b9d55-166">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9d55-166">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="b9d55-167">Le code ci-dessus utilise une méthode d'extension `Load` pour rendre l’utilisation du délégué un peu plus propre :</span><span class="sxs-lookup"><span data-stu-id="b9d55-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
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
> <span data-ttu-id="b9d55-168">Le paramètre de constructeur pour le délégué à chargement différé doit être appelé « lazyLoader ».</span><span class="sxs-lookup"><span data-stu-id="b9d55-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="b9d55-169">La configuration permettant d’utiliser un nom différent est prévue pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b9d55-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="b9d55-170">Données associées et sérialisation</span><span class="sxs-lookup"><span data-stu-id="b9d55-170">Related data and serialization</span></span>

<span data-ttu-id="b9d55-171">Étant donné qu’EF Core corrigera automatiquement les propriétés de navigation, vous pouvez vous retrouver avec des cycles dans votre graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="b9d55-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="b9d55-172">Par exemple, le chargement d’un blog et de ses billets associés aboutit à un objet de blog qui fait référence à une collection de billets.</span><span class="sxs-lookup"><span data-stu-id="b9d55-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="b9d55-173">Chacun de ces billets aura une référence vers le blog.</span><span class="sxs-lookup"><span data-stu-id="b9d55-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="b9d55-174">Certaines infrastructures de sérialisation n’autorisent pas de tels cycles.</span><span class="sxs-lookup"><span data-stu-id="b9d55-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="b9d55-175">Par exemple, Json.NET lève l’exception suivante si un cycle est rencontré.</span><span class="sxs-lookup"><span data-stu-id="b9d55-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="b9d55-176">Newtonsoft.Json.JsonSerializationException : boucle à référence circulaire détectée pour la propriété « Blog » avec le type « MyApplication.Models.Blog ».</span><span class="sxs-lookup"><span data-stu-id="b9d55-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="b9d55-177">Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il trouve dans le graphique d’objets.</span><span class="sxs-lookup"><span data-stu-id="b9d55-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="b9d55-178">Cette opération est effectuée dans la méthode `ConfigureServices(...)` de `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="b9d55-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
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
