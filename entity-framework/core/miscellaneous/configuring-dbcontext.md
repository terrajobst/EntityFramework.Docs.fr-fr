---
title: Configuration d’un DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59363935"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="dc2bc-102">Configuration d’un DbContext</span><span class="sxs-lookup"><span data-stu-id="dc2bc-102">Configuring a DbContext</span></span>

<span data-ttu-id="dc2bc-103">Cet article présente les modèles de base pour la configuration un `DbContext` via un `DbContextOptions` pour se connecter à une base de données à l’aide d’un fournisseur EF Core spécifique et les comportements facultatifs.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="dc2bc-104">Configuration de DbContext au moment du design</span><span class="sxs-lookup"><span data-stu-id="dc2bc-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="dc2bc-105">EF Core au moment du design des outils tels que [migrations](xref:core/managing-schemas/migrations/index) doivent être en mesure de détecter et de créer une instance de l’utilisation d’un `DbContext` type afin de collecter des informations détaillées sur les types d’entité de l’application et comment ils sont mappés à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="dc2bc-106">Ce processus peut être automatique, que l’outil peut facilement créer le `DbContext` de sorte qu’il sera configuré de la même façon à la façon dont elle est configurée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="dc2bc-107">Bien que n’importe quel modèle qui fournit les informations de configuration nécessaires à la `DbContext` peut fonctionner au moment de l’exécution, les outils qui requièrent l’utilisation un `DbContext` au moment du design ne fonctionnent qu’avec un nombre limité de modèles.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="dc2bc-108">Elles sont traitées plus en détail dans le [la création du contexte au moment du Design](xref:core/miscellaneous/cli/dbcontext-creation) section.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="dc2bc-109">Configuration de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="dc2bc-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="dc2bc-110">`DbContext` doit avoir une instance de `DbContextOptions` afin d’effectuer des tâches.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="dc2bc-111">Le `DbContextOptions` instance comporte des informations de configuration telles que :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="dc2bc-112">Le fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode comme `UseSqlServer` ou `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="dc2bc-113">Ces méthodes d’extension requièrent le package de fournisseur correspondant, tel que `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="dc2bc-114">Les méthodes sont définies dans le `Microsoft.EntityFrameworkCore` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="dc2bc-115">Toute chaîne de connexion nécessaires ou d’un identificateur de l’instance de la base de données, généralement passé en tant qu’argument à la méthode de sélection de fournisseur mentionnée ci-dessus</span><span class="sxs-lookup"><span data-stu-id="dc2bc-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="dc2bc-116">Tous les sélecteurs comportement facultatif au niveau du fournisseur, généralement également chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur</span><span class="sxs-lookup"><span data-stu-id="dc2bc-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="dc2bc-117">N’importe quel sélecteurs de comportement générales EF Core, généralement chaînées après ou avant la méthode de sélection du fournisseur</span><span class="sxs-lookup"><span data-stu-id="dc2bc-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="dc2bc-118">L’exemple suivant configure le `DbContextOptions` pour utiliser le fournisseur SQL Server, une connexion contenue dans le `connectionString` variable, un délai d’expiration au niveau du fournisseur de commande et un sélecteur de comportement d’EF Core qui effectue toutes les requêtes exécutées dans le `DbContext` [NoTracking](xref:core/querying/tracking#no-tracking-queries) par défaut :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="dc2bc-119">Méthodes de sélecteur de fournisseur et d’autres méthodes de sélecteur de comportement mentionnés ci-dessus sont des méthodes d’extension sur `DbContextOptions` ou classes d’option spécifique au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="dc2bc-120">Pour accéder à ces méthodes d’extension que vous devrez peut-être disposer d’un espace de noms (généralement `Microsoft.EntityFrameworkCore`) dans l’étendue et inclure les dépendances de package supplémentaires dans le projet.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="dc2bc-121">Le `DbContextOptions` peuvent être fournis à la `DbContext` en substituant le `OnConfiguring` méthode ou en externe via un argument de constructeur.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="dc2bc-122">Si les deux sont utilisés, `OnConfiguring` est appliqué en dernier et peuvent remplacer des options fournies à l’argument de constructeur.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="dc2bc-123">Argument du constructeur</span><span class="sxs-lookup"><span data-stu-id="dc2bc-123">Constructor argument</span></span>

<span data-ttu-id="dc2bc-124">Code de contexte avec le constructeur :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="dc2bc-125">Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`, mais à l’aide de la version non générique n’est pas recommandé pour les applications avec plusieurs types de contexte.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="dc2bc-126">Code d’application d’initialiser à partir de l’argument du constructeur :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="dc2bc-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="dc2bc-127">OnConfiguring</span></span>

<span data-ttu-id="dc2bc-128">Code de contexte avec `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="dc2bc-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="dc2bc-129">Code d’application pour initialiser un `DbContext` qui utilise `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="dc2bc-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="dc2bc-130">Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="dc2bc-131">À l’aide de DbContext avec l’injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="dc2bc-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="dc2bc-132">EF Core prend en charge à l’aide de `DbContext` avec un conteneur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="dc2bc-133">Votre type DbContext peut être ajouté au conteneur de service en utilisant le `AddDbContext<TContext>` (méthode).</span><span class="sxs-lookup"><span data-stu-id="dc2bc-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="dc2bc-134">`AddDbContext<TContext>` rendra à la fois votre type DbContext, `TContext`et le correspondantes `DbContextOptions<TContext>` disponibles pour l’injection de code à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="dc2bc-135">Consultez [lire plus](#more-reading) ci-dessous pour plus d’informations sur l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="dc2bc-136">Ajout de la `Dbcontext` pour l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="dc2bc-137">Cela nécessite l’ajout d’un [argument du constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="dc2bc-138">Code de contexte :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="dc2bc-139">Code d’application (dans ASP.NET Core) :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="dc2bc-140">Code d’application (à l’aide de ServiceProvider directement, moins fréquent) :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="dc2bc-141">Éviter les problèmes liés aux threads de DbContext</span><span class="sxs-lookup"><span data-stu-id="dc2bc-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="dc2bc-142">Entity Framework Core ne prend pas en charge plusieurs opérations parallèles en cours d’exécution sur le même `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="dc2bc-143">Accès simultané peut entraîner un comportement non défini, les blocages d’application et une altération des données.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="dc2bc-144">C’est pourquoi il est important de toujours utiliser séparer `DbContext` instances pour les opérations qui s’exécutent en parallèle.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="dc2bc-145">Il existe des erreurs courantes qui peuvent inadvernetly cause des accès simultané sur le même `DbContext` instance :</span><span class="sxs-lookup"><span data-stu-id="dc2bc-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="dc2bc-146">Oubli à attendre la fin d’une opération asynchrone avant de commencer toute autre opération sur le même DbContext</span><span class="sxs-lookup"><span data-stu-id="dc2bc-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="dc2bc-147">Les méthodes asynchrones permettent EF Core lancer des opérations qui accèdent à la base de données de façon non bloquante.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="dc2bc-148">Mais si un appelant n’attend pas la fin d’une des méthodes suivantes, puis passe à effectuer d’autres opérations sur le `DbContext`, l’état de la `DbContext` peut être (et sera très probablement) endommagé.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="dc2bc-149">Toujours attendre les méthodes asynchrones EF Core immédiatement.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="dc2bc-150">Partage implicitement des instances de DbContext entre plusieurs threads par le biais de l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="dc2bc-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="dc2bc-151">Le [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) enregistre la méthode d’extension `DbContext` types avec un [durée de vie délimitée](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) par défaut.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="dc2bc-152">Cela est protégée contre les problèmes d’accès simultané dans les applications ASP.NET Core, car il y n'a qu’un seul thread à un moment donné, l’exécution de chaque demande du client, et étant donné que chaque requête obtient une étendue de l’injection de dépendance distinct (et par conséquent une `DbContext` instance).</span><span class="sxs-lookup"><span data-stu-id="dc2bc-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="dc2bc-153">Toutefois, tout code qui exécute explicitement plusieurs threads en parallèle devez vous assurer de que `DbContext` instances ne sont pas jamais accesed simultanément.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="dc2bc-154">À l’aide de l’injection de dépendances, cela est possible en inscrire soit le contexte comme étendues et de création des étendues (à l’aide de `IServiceScopeFactory`) pour chaque thread ou en inscrivant le `DbContext` comme provisoire (à l’aide de la surcharge de `AddDbContext` qui accepte un `ServiceLifetime` paramètre).</span><span class="sxs-lookup"><span data-stu-id="dc2bc-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="dc2bc-155">Lire plus</span><span class="sxs-lookup"><span data-stu-id="dc2bc-155">More reading</span></span>

* <span data-ttu-id="dc2bc-156">Lecture [bien démarrer avec ASP.NET Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation d’EF avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="dc2bc-157">Lecture [l’Injection de dépendances](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) pour en savoir plus sur l’utilisation de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="dc2bc-158">Lecture [test](testing/index.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="dc2bc-158">Read [Testing](testing/index.md) for more information.</span></span>
