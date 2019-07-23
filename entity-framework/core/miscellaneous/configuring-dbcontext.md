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
# <a name="configuring-a-dbcontext"></a>Configuration d’un DbContext

Cet article présente des modèles de base pour configurer `DbContext` un via `DbContextOptions` un pour se connecter à une base de données à l’aide d’un fournisseur de EF Core spécifique et de comportements facultatifs.

## <a name="design-time-dbcontext-configuration"></a>Configuration de DbContext au moment du design

EF Core des outils au moment du design, tels que les [migrations](xref:core/managing-schemas/migrations/index), doivent être en mesure de découvrir et de `DbContext` créer une instance de travail d’un type afin de rassembler des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données. Ce processus peut être automatique tant que l’outil peut facilement créer le de `DbContext` façon à ce qu’il soit configuré de façon similaire à la façon dont il est configuré au moment de l’exécution.

Alors que tout modèle qui fournit les informations de configuration nécessaires `DbContext` au peut fonctionner au moment de l’exécution, les outils qui `DbContext` nécessitent l’utilisation d’un au moment du design peuvent uniquement fonctionner avec un nombre limité de modèles. Celles-ci sont traitées plus en détail dans la section [création de contexte au moment du design](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Configuration de DbContextOptions

`DbContext`doit avoir une instance de `DbContextOptions` afin d’effectuer n’importe quel travail. L' `DbContextOptions` instance contient des informations de configuration telles que:

- Fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode telle `UseSqlServer` que `UseSqlite`ou. Ces méthodes d’extension requièrent le package du fournisseur correspondant `Microsoft.EntityFrameworkCore.SqlServer` , `Microsoft.EntityFrameworkCore.Sqlite`tel que ou. Les méthodes sont définies dans l' `Microsoft.EntityFrameworkCore` espace de noms.
- Toute chaîne de connexion ou tout identificateur nécessaire de l’instance de base de données, généralement passé comme argument à la méthode de sélection du fournisseur mentionnée ci-dessus
- Tous les sélecteurs de comportements facultatifs au niveau du fournisseur, généralement chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur
- Tous les sélecteurs de comportements EF Core généraux, en général chaînés après ou avant la méthode du sélecteur du fournisseur

L’exemple suivant configure `DbContextOptions` pour utiliser le fournisseur SQL Server, une connexion contenue dans la `connectionString` variable, un délai d’attente de commande au niveau du fournisseur et un sélecteur de comportement EF Core qui effectue toutes les `DbContext` requêtes exécutées dans le [sans suivi](xref:core/querying/tracking#no-tracking-queries) par défaut:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Les méthodes de sélection de fournisseur et d’autres méthodes de sélecteur de comportements `DbContextOptions` mentionnées ci-dessus sont des méthodes d’extension sur les classes d’options spécifiques au fournisseur ou. Pour pouvoir accéder à ces méthodes d’extension, vous devrez peut-être disposer d’un espace `Microsoft.EntityFrameworkCore`de noms (en général) dans la portée et inclure des dépendances de package supplémentaires dans le projet.

Peut être fourni `DbContext` à en substituant la `OnConfiguring` méthode ou en externe via un argument de constructeur. `DbContextOptions`

Si les deux sont utilisés `OnConfiguring` , est appliqué en dernier et peut remplacer les options fournies à l’argument du constructeur.

### <a name="constructor-argument"></a>Argument de constructeur

Code de contexte avec le constructeur:

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
> Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`, mais l’utilisation de la version non générique n’est pas recommandée pour les applications avec plusieurs types de contexte.

Code d’application à initialiser à partir d’un argument de constructeur:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Code de contexte `OnConfiguring`avec:

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

Code d’application pour initialiser `DbContext` un qui `OnConfiguring`utilise:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.

### <a name="using-dbcontext-with-dependency-injection"></a>Utilisation de DbContext avec l’injection de dépendances

EF Core prend en `DbContext` charge l’utilisation de avec un conteneur d’injection de dépendances. Votre type DbContext peut être ajouté au conteneur de service à l’aide `AddDbContext<TContext>` de la méthode.

`AddDbContext<TContext>`rendra à la fois votre type `TContext`DbContext,, et `DbContextOptions<TContext>` le correspondant disponible pour l’injection à partir du conteneur de service.

Pour plus d’informations sur l’injection de dépendances, voir [plus de lectures](#more-reading) ci-dessous.

Ajout de `DbContext` à l’injection de dépendances:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Cela nécessite l’ajout d’un [argument de constructeur](#constructor-argument) à votre type `DbContextOptions<TContext>`DbContext qui accepte.

Code de contexte:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Code d’application (dans ASP.NET Core):

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

Code d’application (à l’aide de ServiceProvider directement, moins courant):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Éviter les problèmes de thread DbContext

Entity Framework Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même `DbContext` instance. Cela comprend à la fois l’exécution parallèle des requêtes Async et toute utilisation simultanée explicite à partir de plusieurs threads. Par conséquent, `await` toujours Async appelle immédiatement, ou utilisez `DbContext` des instances distinctes pour les opérations qui s’exécutent en parallèle.

Lorsque EF Core détecte une tentative d’utilisation d’une `DbContext` instance simultanément, vous voyez un `InvalidOperationException` avec un message semblable à celui-ci: 

> Une deuxième opération a démarré sur ce contexte avant la fin d’une opération précédente. Cela est généralement dû à différents threads utilisant la même instance de DbContext, mais il n’est pas garanti que les membres d’instance soient thread-safe.

Lorsque l’accès simultané n’est pas détecté, cela peut entraîner un comportement non défini, des blocages d’application et des données endommagées.

Il existe des erreurs courantes qui peuvent entraîner par inadvertance un accès simultané `DbContext` sur la même instance:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Oubli de l’attente de la fin d’une opération asynchrone avant le démarrage d’une autre opération sur le même DbContext

Les méthodes asynchrones permettent à EF Core de lancer des opérations qui accèdent à la base de données de façon non bloquante. Toutefois, si un appelant n’attend pas la fin de l’une de ces méthodes et poursuit l’exécution d’autres opérations `DbContext`sur le, l’état `DbContext` du peut être, (et très probablement) endommagé. 

Attendez toujours EF Core les méthodes asynchrones immédiatement.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Partage implicite d’instances de DbContext sur plusieurs threads via l’injection de dépendances

La [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) méthode d’extension `DbContext` inscrit les types avec une [durée de vie limitée](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) par défaut. 

Cela ne pose pas de problèmes d’accès simultané dans les applications ASP.NET Core, car il n’existe qu’un seul thread qui exécute chaque demande client à un moment donné, et parce que chaque demande obtient une étendue d' `DbContext` injection de dépendances distincte (et donc un instance).

Toutefois, tout code qui exécute explicitement plusieurs threads en parallèle doit s' `DbContext` assurer que les instances ne sont jamais accessibles simultanément.

À l’aide de l’injection de dépendances, cela peut être obtenu en inscrivant le contexte comme étant étendu `IServiceScopeFactory`et en créant des portées (à l' `DbContext` aide de) pour chaque thread, `AddDbContext` ou en inscrivant le comme transitoire (à l’aide de la surcharge de qui prend un `ServiceLifetime` paramètre).

## <a name="more-reading"></a>Plus de lectures

* Lisez [prise en main sur ASP.net Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation d’EF avec ASP.net core.
* Lisez [injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) de dépendances pour en savoir plus sur l’utilisation de di.
* Pour plus d’informations, consultez le [test](testing/index.md) .
