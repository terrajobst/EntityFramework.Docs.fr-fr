---
title: Configuration d’un DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306511"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="e0665-102">Configuration d’un DbContext</span><span class="sxs-lookup"><span data-stu-id="e0665-102">Configuring a DbContext</span></span>

<span data-ttu-id="e0665-103">Cet article présente des modèles de base pour configurer `DbContext` un via `DbContextOptions` un pour se connecter à une base de données à l’aide d’un fournisseur de EF Core spécifique et de comportements facultatifs.</span><span class="sxs-lookup"><span data-stu-id="e0665-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="e0665-104">Configuration de DbContext au moment du design</span><span class="sxs-lookup"><span data-stu-id="e0665-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="e0665-105">EF Core des outils au moment du design, tels que les [migrations](xref:core/managing-schemas/migrations/index), doivent être en mesure de découvrir et de `DbContext` créer une instance de travail d’un type afin de rassembler des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="e0665-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="e0665-106">Ce processus peut être automatique tant que l’outil peut facilement créer le de `DbContext` façon à ce qu’il soit configuré de façon similaire à la façon dont il est configuré au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e0665-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="e0665-107">Alors que tout modèle qui fournit les informations de configuration nécessaires `DbContext` au peut fonctionner au moment de l’exécution, les outils qui `DbContext` nécessitent l’utilisation d’un au moment du design peuvent uniquement fonctionner avec un nombre limité de modèles.</span><span class="sxs-lookup"><span data-stu-id="e0665-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="e0665-108">Celles-ci sont traitées plus en détail dans la section [création de contexte au moment du design](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="e0665-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="e0665-109">Configuration de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="e0665-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="e0665-110">`DbContext`doit avoir une instance de `DbContextOptions` afin d’effectuer n’importe quel travail.</span><span class="sxs-lookup"><span data-stu-id="e0665-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="e0665-111">L' `DbContextOptions` instance contient des informations de configuration telles que:</span><span class="sxs-lookup"><span data-stu-id="e0665-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="e0665-112">Fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode telle `UseSqlServer` que `UseSqlite`ou.</span><span class="sxs-lookup"><span data-stu-id="e0665-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="e0665-113">Ces méthodes d’extension requièrent le package du fournisseur correspondant `Microsoft.EntityFrameworkCore.SqlServer` , `Microsoft.EntityFrameworkCore.Sqlite`tel que ou.</span><span class="sxs-lookup"><span data-stu-id="e0665-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="e0665-114">Les méthodes sont définies dans l' `Microsoft.EntityFrameworkCore` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="e0665-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="e0665-115">Toute chaîne de connexion ou tout identificateur nécessaire de l’instance de base de données, généralement passé comme argument à la méthode de sélection du fournisseur mentionnée ci-dessus</span><span class="sxs-lookup"><span data-stu-id="e0665-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="e0665-116">Tous les sélecteurs de comportements facultatifs au niveau du fournisseur, généralement chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur</span><span class="sxs-lookup"><span data-stu-id="e0665-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="e0665-117">Tous les sélecteurs de comportements EF Core généraux, en général chaînés après ou avant la méthode du sélecteur du fournisseur</span><span class="sxs-lookup"><span data-stu-id="e0665-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="e0665-118">L’exemple suivant configure `DbContextOptions` pour utiliser le fournisseur SQL Server, une connexion contenue dans la `connectionString` variable, un délai d’attente de commande au niveau du fournisseur et un sélecteur de comportement EF Core qui effectue toutes les `DbContext` requêtes exécutées dans le [sans suivi](xref:core/querying/tracking#no-tracking-queries) par défaut:</span><span class="sxs-lookup"><span data-stu-id="e0665-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="e0665-119">Les méthodes de sélection de fournisseur et d’autres méthodes de sélecteur de comportements `DbContextOptions` mentionnées ci-dessus sont des méthodes d’extension sur les classes d’options spécifiques au fournisseur ou.</span><span class="sxs-lookup"><span data-stu-id="e0665-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="e0665-120">Pour pouvoir accéder à ces méthodes d’extension, vous devrez peut-être disposer d’un espace `Microsoft.EntityFrameworkCore`de noms (en général) dans la portée et inclure des dépendances de package supplémentaires dans le projet.</span><span class="sxs-lookup"><span data-stu-id="e0665-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="e0665-121">Peut être fourni `DbContext` à en substituant la `OnConfiguring` méthode ou en externe via un argument de constructeur. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="e0665-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="e0665-122">Si les deux sont utilisés `OnConfiguring` , est appliqué en dernier et peut remplacer les options fournies à l’argument du constructeur.</span><span class="sxs-lookup"><span data-stu-id="e0665-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="e0665-123">Argument de constructeur</span><span class="sxs-lookup"><span data-stu-id="e0665-123">Constructor argument</span></span>

<span data-ttu-id="e0665-124">Code de contexte avec le constructeur:</span><span class="sxs-lookup"><span data-stu-id="e0665-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="e0665-125">Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`, mais l’utilisation de la version non générique n’est pas recommandée pour les applications avec plusieurs types de contexte.</span><span class="sxs-lookup"><span data-stu-id="e0665-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="e0665-126">Code d’application à initialiser à partir d’un argument de constructeur:</span><span class="sxs-lookup"><span data-stu-id="e0665-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="e0665-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="e0665-127">OnConfiguring</span></span>

<span data-ttu-id="e0665-128">Code de contexte `OnConfiguring`avec:</span><span class="sxs-lookup"><span data-stu-id="e0665-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="e0665-129">Code d’application pour initialiser `DbContext` un qui `OnConfiguring`utilise:</span><span class="sxs-lookup"><span data-stu-id="e0665-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="e0665-130">Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.</span><span class="sxs-lookup"><span data-stu-id="e0665-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="e0665-131">Utilisation de DbContext avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e0665-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="e0665-132">EF Core prend en `DbContext` charge l’utilisation de avec un conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e0665-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="e0665-133">Votre type DbContext peut être ajouté au conteneur de service à l’aide `AddDbContext<TContext>` de la méthode.</span><span class="sxs-lookup"><span data-stu-id="e0665-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="e0665-134">`AddDbContext<TContext>`rendra à la fois votre type `TContext`DbContext,, et `DbContextOptions<TContext>` le correspondant disponible pour l’injection à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="e0665-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="e0665-135">Pour plus d’informations sur l’injection de dépendances, voir [plus de lectures](#more-reading) ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e0665-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="e0665-136">Ajout de `DbContext` à l’injection de dépendances:</span><span class="sxs-lookup"><span data-stu-id="e0665-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="e0665-137">Cela nécessite l’ajout d’un [argument de constructeur](#constructor-argument) à votre type `DbContextOptions<TContext>`DbContext qui accepte.</span><span class="sxs-lookup"><span data-stu-id="e0665-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="e0665-138">Code de contexte:</span><span class="sxs-lookup"><span data-stu-id="e0665-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="e0665-139">Code d’application (dans ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="e0665-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="e0665-140">Code d’application (à l’aide de ServiceProvider directement, moins courant):</span><span class="sxs-lookup"><span data-stu-id="e0665-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="e0665-141">Éviter les problèmes de thread DbContext</span><span class="sxs-lookup"><span data-stu-id="e0665-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="e0665-142">Entity Framework Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="e0665-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="e0665-143">Cela comprend à la fois l’exécution parallèle des requêtes Async et toute utilisation simultanée explicite à partir de plusieurs threads.</span><span class="sxs-lookup"><span data-stu-id="e0665-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="e0665-144">Par conséquent, `await` toujours Async appelle immédiatement, ou utilisez `DbContext` des instances distinctes pour les opérations qui s’exécutent en parallèle.</span><span class="sxs-lookup"><span data-stu-id="e0665-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="e0665-145">Lorsque EF Core détecte une tentative d’utilisation d’une `DbContext` instance simultanément, vous voyez un `InvalidOperationException` avec un message semblable à celui-ci:</span><span class="sxs-lookup"><span data-stu-id="e0665-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="e0665-146">Une deuxième opération a démarré sur ce contexte avant la fin d’une opération précédente.</span><span class="sxs-lookup"><span data-stu-id="e0665-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="e0665-147">Cela est généralement dû à différents threads utilisant la même instance de DbContext, mais il n’est pas garanti que les membres d’instance soient thread-safe.</span><span class="sxs-lookup"><span data-stu-id="e0665-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="e0665-148">Lorsque l’accès simultané n’est pas détecté, cela peut entraîner un comportement non défini, des blocages d’application et des données endommagées.</span><span class="sxs-lookup"><span data-stu-id="e0665-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="e0665-149">Il existe des erreurs courantes qui peuvent entraîner par inadvertance un accès simultané `DbContext` sur la même instance:</span><span class="sxs-lookup"><span data-stu-id="e0665-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="e0665-150">Oubli de l’attente de la fin d’une opération asynchrone avant le démarrage d’une autre opération sur le même DbContext</span><span class="sxs-lookup"><span data-stu-id="e0665-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="e0665-151">Les méthodes asynchrones permettent à EF Core de lancer des opérations qui accèdent à la base de données de façon non bloquante.</span><span class="sxs-lookup"><span data-stu-id="e0665-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="e0665-152">Toutefois, si un appelant n’attend pas la fin de l’une de ces méthodes et poursuit l’exécution d’autres opérations `DbContext`sur le, l’état `DbContext` du peut être, (et très probablement) endommagé.</span><span class="sxs-lookup"><span data-stu-id="e0665-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="e0665-153">Attendez toujours EF Core les méthodes asynchrones immédiatement.</span><span class="sxs-lookup"><span data-stu-id="e0665-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="e0665-154">Partage implicite d’instances de DbContext sur plusieurs threads via l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e0665-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="e0665-155">La [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) méthode d’extension `DbContext` inscrit les types avec une [durée de vie limitée](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) par défaut.</span><span class="sxs-lookup"><span data-stu-id="e0665-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="e0665-156">Cela ne pose pas de problèmes d’accès simultané dans les applications ASP.NET Core, car il n’existe qu’un seul thread qui exécute chaque demande client à un moment donné, et parce que chaque demande obtient une étendue d' `DbContext` injection de dépendances distincte (et donc un instance).</span><span class="sxs-lookup"><span data-stu-id="e0665-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="e0665-157">Toutefois, tout code qui exécute explicitement plusieurs threads en parallèle doit s' `DbContext` assurer que les instances ne sont jamais accessibles simultanément.</span><span class="sxs-lookup"><span data-stu-id="e0665-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="e0665-158">À l’aide de l’injection de dépendances, cela peut être obtenu en inscrivant le contexte comme étant étendu `IServiceScopeFactory`et en créant des portées (à l' `DbContext` aide de) pour chaque thread, `AddDbContext` ou en inscrivant le comme transitoire (à l’aide de la surcharge de qui prend un `ServiceLifetime` paramètre).</span><span class="sxs-lookup"><span data-stu-id="e0665-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="e0665-159">Plus de lectures</span><span class="sxs-lookup"><span data-stu-id="e0665-159">More reading</span></span>

* <span data-ttu-id="e0665-160">Lisez [prise en main sur ASP.net Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation d’EF avec ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="e0665-160">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="e0665-161">Lisez [injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) de dépendances pour en savoir plus sur l’utilisation de di.</span><span class="sxs-lookup"><span data-stu-id="e0665-161">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="e0665-162">Pour plus d’informations, consultez le [test](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="e0665-162">Read [Testing](testing/index.md) for more information.</span></span>
