---
title: "Configuration d’un DbContext - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="4d902-102">Configuration d’un DbContext</span><span class="sxs-lookup"><span data-stu-id="4d902-102">Configuring a DbContext</span></span>

<span data-ttu-id="4d902-103">Cet article présente les modèles pour configurer un `DbContext` avec `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4d902-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="4d902-104">Options sont principalement utilisées pour sélectionner et configurer le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="4d902-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="4d902-105">Configuration DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="4d902-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="4d902-106">`DbContext`doit avoir une instance de `DbContextOptions` afin d’exécuter.</span><span class="sxs-lookup"><span data-stu-id="4d902-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="4d902-107">Ceci peut être configuré en substituant `OnConfiguring`, ou fourni en externe via un argument de constructeur.</span><span class="sxs-lookup"><span data-stu-id="4d902-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="4d902-108">Si les deux sont utilisés, `OnConfiguring` est exécutée sur les options fournies, ce qui signifie qu’il est additif et peuvent remplacer des options fournies pour l’argument du constructeur.</span><span class="sxs-lookup"><span data-stu-id="4d902-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="4d902-109">Argument du constructeur</span><span class="sxs-lookup"><span data-stu-id="4d902-109">Constructor argument</span></span>

<span data-ttu-id="4d902-110">Code de contexte avec un constructeur</span><span class="sxs-lookup"><span data-stu-id="4d902-110">Context code with constructor</span></span>

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
> <span data-ttu-id="4d902-111">Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4d902-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="4d902-112">À l’aide de la version non générique n’est pas recommandé pour les applications avec plusieurs types de contexte.</span><span class="sxs-lookup"><span data-stu-id="4d902-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="4d902-113">Code de l’application d’initialiser à partir de l’argument du constructeur</span><span class="sxs-lookup"><span data-stu-id="4d902-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="4d902-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="4d902-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="4d902-115">`OnConfiguring`dernière et peuvent remplacer des options obtenues à partir de DI ou le constructeur.</span><span class="sxs-lookup"><span data-stu-id="4d902-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="4d902-116">Cette approche ne se prête pas à tester (sauf si vous ciblez la base de données complète).</span><span class="sxs-lookup"><span data-stu-id="4d902-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="4d902-117">Code de contexte avec `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4d902-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="4d902-118">Code d’application pour l’initialiser avec `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4d902-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="4d902-119">À l’aide de DbContext avec injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="4d902-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="4d902-120">EF prend en charge à l’aide de `DbContext` avec un conteneur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="4d902-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="4d902-121">Le type DbContext peut être ajouté au conteneur de service à l’aide de `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4d902-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="4d902-122">`AddDbContext`permettront à la fois votre type DbContext, `TContext`, et `DbContextOptions<TContext>` disponibles pour l’injection de code à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="4d902-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="4d902-123">Consultez [lecture plus](#more-reading) ci-dessous pour plus d’informations sur l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="4d902-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="4d902-124">Ajout de dbcontext risque d’injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="4d902-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="4d902-125">Cela requiert l’ajout d’un [argument du constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4d902-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="4d902-126">Code de contexte :</span><span class="sxs-lookup"><span data-stu-id="4d902-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="4d902-127">Code d’application (dans ASP.NET Core) :</span><span class="sxs-lookup"><span data-stu-id="4d902-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="4d902-128">Code d’application (à l’aide de ServiceProvider directement, moins courants) :</span><span class="sxs-lookup"><span data-stu-id="4d902-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="4d902-129">À l’aide de`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="4d902-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="4d902-130">Comme alternative aux options ci-dessus, vous pouvez également fournir une implémentation de `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4d902-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="4d902-131">Outils de la EF peuvent utiliser cette fabrique pour créer une instance de votre DbContext.</span><span class="sxs-lookup"><span data-stu-id="4d902-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="4d902-132">Cela peut être nécessaire pour permettre des expériences au moment du design spécifiques tels que des migrations.</span><span class="sxs-lookup"><span data-stu-id="4d902-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="4d902-133">Implémentez cette interface pour permettre aux services au moment du design pour les types de contexte qui n’ont pas de constructeur public par défaut.</span><span class="sxs-lookup"><span data-stu-id="4d902-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="4d902-134">Les services au moment du design détecte automatiquement les implémentations de cette interface qui se trouvent dans le même assembly que le contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="4d902-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="4d902-135">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4d902-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="4d902-136">Plus la lecture</span><span class="sxs-lookup"><span data-stu-id="4d902-136">More reading</span></span>

* <span data-ttu-id="4d902-137">Lecture [mise en route sur ASP.NET Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation de EF avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d902-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="4d902-138">Lecture [Injection de dépendance](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) pour en savoir plus sur l’utilisation de DI.</span><span class="sxs-lookup"><span data-stu-id="4d902-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="4d902-139">Lecture [test](testing/index.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="4d902-139">Read [Testing](testing/index.md) for more information.</span></span>
