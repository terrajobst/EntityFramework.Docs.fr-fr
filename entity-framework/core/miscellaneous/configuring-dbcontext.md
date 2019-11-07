---
title: Configuration d’un DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655799"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="104d7-102">Configuration d’un DbContext</span><span class="sxs-lookup"><span data-stu-id="104d7-102">Configuring a DbContext</span></span>

<span data-ttu-id="104d7-103">Cet article présente les modèles de base pour la configuration d’un `DbContext` via une `DbContextOptions` pour se connecter à une base de données à l’aide d’un fournisseur de EF Core spécifique et de comportements facultatifs.</span><span class="sxs-lookup"><span data-stu-id="104d7-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="104d7-104">Configuration de DbContext au moment du design</span><span class="sxs-lookup"><span data-stu-id="104d7-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="104d7-105">EF Core des outils au moment du design, tels que les [migrations](xref:core/managing-schemas/migrations/index) , doivent être en mesure de découvrir et de créer une instance de travail d’un type `DbContext` afin de rassembler des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="104d7-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="104d7-106">Ce processus peut être automatique tant que l’outil peut facilement créer le `DbContext` de manière à ce qu’il soit configuré de façon similaire à la façon dont il est configuré au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="104d7-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="104d7-107">Alors que tout modèle qui fournit les informations de configuration nécessaires au `DbContext` peut fonctionner au moment de l’exécution, les outils qui nécessitent l’utilisation d’une `DbContext` au moment du design peuvent uniquement fonctionner avec un nombre limité de modèles.</span><span class="sxs-lookup"><span data-stu-id="104d7-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="104d7-108">Celles-ci sont traitées plus en détail dans la section [création de contexte au moment du design](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="104d7-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="104d7-109">Configuration de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="104d7-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="104d7-110">`DbContext` doit avoir une instance de `DbContextOptions` afin d’effectuer n’importe quel travail.</span><span class="sxs-lookup"><span data-stu-id="104d7-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="104d7-111">L’instance `DbContextOptions` contient des informations de configuration telles que :</span><span class="sxs-lookup"><span data-stu-id="104d7-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="104d7-112">Fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode telle que `UseSqlServer` ou `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="104d7-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="104d7-113">Ces méthodes d’extension nécessitent le package du fournisseur correspondant, tel que `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="104d7-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="104d7-114">Les méthodes sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="104d7-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="104d7-115">Toute chaîne de connexion ou tout identificateur nécessaire de l’instance de base de données, généralement passé comme argument à la méthode de sélection du fournisseur mentionnée ci-dessus</span><span class="sxs-lookup"><span data-stu-id="104d7-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="104d7-116">Tous les sélecteurs de comportements facultatifs au niveau du fournisseur, généralement chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur</span><span class="sxs-lookup"><span data-stu-id="104d7-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="104d7-117">Tous les sélecteurs de comportements EF Core généraux, en général chaînés après ou avant la méthode du sélecteur du fournisseur</span><span class="sxs-lookup"><span data-stu-id="104d7-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="104d7-118">L’exemple suivant configure la `DbContextOptions` pour utiliser le fournisseur de SQL Server, une connexion contenue dans la variable `connectionString`, un délai d’attente de commande au niveau du fournisseur et un sélecteur de comportement EF Core qui effectue toutes les requêtes exécutées dans le `DbContext` [sans suivi](xref:core/querying/tracking#no-tracking-queries) . par défaut :</span><span class="sxs-lookup"><span data-stu-id="104d7-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="104d7-119">Les méthodes de sélection de fournisseur et d’autres méthodes de sélecteur de comportements mentionnées ci-dessus sont des méthodes d’extension sur `DbContextOptions` ou des classes d’options spécifiques au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="104d7-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="104d7-120">Pour pouvoir accéder à ces méthodes d’extension, vous devrez peut-être avoir un espace de noms (généralement `Microsoft.EntityFrameworkCore`) dans la portée et inclure des dépendances de package supplémentaires dans le projet.</span><span class="sxs-lookup"><span data-stu-id="104d7-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="104d7-121">Le `DbContextOptions` peut être fourni à la `DbContext` en substituant la méthode `OnConfiguring` ou en externe à l’aide d’un argument de constructeur.</span><span class="sxs-lookup"><span data-stu-id="104d7-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="104d7-122">Si les deux sont utilisés, `OnConfiguring` est appliqué en dernier et peut remplacer les options fournies à l’argument du constructeur.</span><span class="sxs-lookup"><span data-stu-id="104d7-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="104d7-123">Argument de constructeur</span><span class="sxs-lookup"><span data-stu-id="104d7-123">Constructor argument</span></span>

<span data-ttu-id="104d7-124">Votre constructeur peut simplement accepter un `DbContextOptions` comme suit :</span><span class="sxs-lookup"><span data-stu-id="104d7-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="104d7-125">Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`, mais l’utilisation de la version non générique n’est pas recommandée pour les applications avec plusieurs types de contexte.</span><span class="sxs-lookup"><span data-stu-id="104d7-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="104d7-126">Votre application peut maintenant passer le `DbContextOptions` lors de l’instanciation d’un contexte, comme suit :</span><span class="sxs-lookup"><span data-stu-id="104d7-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="104d7-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="104d7-127">OnConfiguring</span></span>

<span data-ttu-id="104d7-128">Vous pouvez également initialiser le `DbContextOptions` dans le contexte lui-même.</span><span class="sxs-lookup"><span data-stu-id="104d7-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="104d7-129">Bien que vous puissiez utiliser cette technique pour la configuration de base, vous devrez généralement obtenir certains détails de configuration de l’extérieur, par exemple une chaîne de connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="104d7-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="104d7-130">Pour ce faire, vous pouvez utiliser une API de configuration ou tout autre moyen.</span><span class="sxs-lookup"><span data-stu-id="104d7-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="104d7-131">Pour initialiser `DbContextOptions` dans le contexte, substituez la méthode `OnConfiguring` et appelez les méthodes sur le `DbContextOptionsBuilder`fourni :</span><span class="sxs-lookup"><span data-stu-id="104d7-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="104d7-132">Une application peut simplement instancier ce type de contexte sans passer quoi que ce soit à son constructeur :</span><span class="sxs-lookup"><span data-stu-id="104d7-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="104d7-133">Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.</span><span class="sxs-lookup"><span data-stu-id="104d7-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="104d7-134">Utilisation de DbContext avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="104d7-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="104d7-135">EF Core prend en charge l’utilisation de `DbContext` avec un conteneur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="104d7-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="104d7-136">Votre type DbContext peut être ajouté au conteneur de service à l’aide de la méthode `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="104d7-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="104d7-137">`AddDbContext<TContext>` fera à la fois votre type DbContext, `TContext` et le `DbContextOptions<TContext>` correspondant disponible pour l’injection à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="104d7-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="104d7-138">Pour plus d’informations sur l’injection de dépendances, voir [plus de lectures](#more-reading) ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="104d7-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="104d7-139">Ajout du `DbContext` à l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="104d7-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="104d7-140">Cela nécessite l’ajout d’un [argument de constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="104d7-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="104d7-141">Code de contexte :</span><span class="sxs-lookup"><span data-stu-id="104d7-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="104d7-142">Code d’application (dans ASP.NET Core) :</span><span class="sxs-lookup"><span data-stu-id="104d7-142">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="104d7-143">Code d’application (à l’aide de ServiceProvider directement, moins courant) :</span><span class="sxs-lookup"><span data-stu-id="104d7-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="104d7-144">Éviter les problèmes de thread DbContext</span><span class="sxs-lookup"><span data-stu-id="104d7-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="104d7-145">Entity Framework Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="104d7-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="104d7-146">Cela comprend à la fois l’exécution parallèle des requêtes Async et toute utilisation simultanée explicite à partir de plusieurs threads.</span><span class="sxs-lookup"><span data-stu-id="104d7-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="104d7-147">Par conséquent, toujours `await` appels asynchrones, ou utiliser des instances `DbContext` distinctes pour les opérations qui s’exécutent en parallèle.</span><span class="sxs-lookup"><span data-stu-id="104d7-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="104d7-148">Lorsque EF Core détecte une tentative d’utilisation d’une instance `DbContext` simultanément, vous voyez un `InvalidOperationException` avec un message semblable à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="104d7-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="104d7-149">Une deuxième opération a démarré sur ce contexte avant la fin d’une opération précédente.</span><span class="sxs-lookup"><span data-stu-id="104d7-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="104d7-150">Cela est généralement dû à différents threads utilisant la même instance de DbContext, mais il n’est pas garanti que les membres d’instance soient thread-safe.</span><span class="sxs-lookup"><span data-stu-id="104d7-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="104d7-151">Lorsque l’accès simultané n’est pas détecté, cela peut entraîner un comportement non défini, des blocages d’application et des données endommagées.</span><span class="sxs-lookup"><span data-stu-id="104d7-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="104d7-152">Il existe des erreurs courantes qui peuvent entraîner par inadvertance un accès simultané sur la même instance de `DbContext` :</span><span class="sxs-lookup"><span data-stu-id="104d7-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="104d7-153">Oubli de l’attente de la fin d’une opération asynchrone avant le démarrage d’une autre opération sur le même DbContext</span><span class="sxs-lookup"><span data-stu-id="104d7-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="104d7-154">Les méthodes asynchrones permettent à EF Core de lancer des opérations qui accèdent à la base de données de façon non bloquante.</span><span class="sxs-lookup"><span data-stu-id="104d7-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="104d7-155">Toutefois, si un appelant n’attend pas la fin de l’une de ces méthodes et poursuit l’exécution d’autres opérations sur le `DbContext`, l’état de la `DbContext` peut être, (et très probablement) endommagé.</span><span class="sxs-lookup"><span data-stu-id="104d7-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="104d7-156">Attendez toujours EF Core les méthodes asynchrones immédiatement.</span><span class="sxs-lookup"><span data-stu-id="104d7-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="104d7-157">Partage implicite d’instances de DbContext sur plusieurs threads via l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="104d7-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="104d7-158">La méthode d’extension [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) enregistre les types `DbContext` avec une [durée de vie limitée](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) par défaut.</span><span class="sxs-lookup"><span data-stu-id="104d7-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="104d7-159">Cela ne pose pas de problèmes d’accès simultané dans les applications ASP.NET Core, car il n’existe qu’un seul thread qui exécute chaque demande client à un moment donné, et parce que chaque demande obtient une étendue d’injection de dépendances distincte (et par conséquent une instance `DbContext` distincte).</span><span class="sxs-lookup"><span data-stu-id="104d7-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="104d7-160">Toutefois, tout code qui exécute explicitement plusieurs threads en parallèle doit s’assurer que les instances `DbContext` ne sont jamais accessibles simultanément.</span><span class="sxs-lookup"><span data-stu-id="104d7-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="104d7-161">À l’aide de l’injection de dépendances, cela peut être obtenu en inscrivant le contexte comme étant étendu et en créant des étendues (à l’aide de `IServiceScopeFactory`) pour chaque thread, ou en inscrivant le `DbContext` comme transitoire (à l’aide de la surcharge de `AddDbContext` qui accepte un paramètre `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="104d7-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="104d7-162">Plus de lectures</span><span class="sxs-lookup"><span data-stu-id="104d7-162">More reading</span></span>

- <span data-ttu-id="104d7-163">Lisez [injection de dépendances](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) pour en savoir plus sur l’utilisation de di.</span><span class="sxs-lookup"><span data-stu-id="104d7-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="104d7-164">Pour plus d’informations, consultez le [test](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="104d7-164">Read [Testing](testing/index.md) for more information.</span></span>
