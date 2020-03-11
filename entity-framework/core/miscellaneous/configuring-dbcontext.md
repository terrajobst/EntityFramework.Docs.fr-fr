---
title: Configuration d’un DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416668"
---
# <a name="configuring-a-dbcontext"></a>Configuration d’un DbContext

Cet article présente les modèles de base pour la configuration d’un `DbContext` par le biais d’une `DbContextOptions` pour se connecter à une base de données à l’aide d’un fournisseur de EF Core spécifique et de comportements facultatifs.

## <a name="design-time-dbcontext-configuration"></a>Configuration de DbContext au moment du design

EF Core des outils au moment du design, tels que les [migrations](xref:core/managing-schemas/migrations/index) , doivent être en mesure de découvrir et de créer une instance de travail d’un type de `DbContext` afin de rassembler des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données. Ce processus peut être automatique tant que l’outil peut facilement créer l' `DbContext` de manière à ce qu’il soit configuré de façon similaire à la façon dont il est configuré au moment de l’exécution.

Bien qu’un modèle qui fournit les informations de configuration nécessaires au `DbContext` puisse fonctionner au moment de l’exécution, les outils qui nécessitent l’utilisation d’un `DbContext` au moment du design peuvent uniquement fonctionner avec un nombre limité de modèles. Celles-ci sont traitées plus en détail dans la section [création de contexte au moment du design](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Configuration de DbContextOptions

`DbContext` doit avoir une instance de `DbContextOptions` afin d’effectuer n’importe quel travail. L’instance `DbContextOptions` contient des informations de configuration telles que :

- Fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode telle que `UseSqlServer` ou `UseSqlite`. Ces méthodes d’extension requièrent le package du fournisseur correspondant, par exemple `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`. Les méthodes sont définies dans l’espace de noms `Microsoft.EntityFrameworkCore`.
- Toute chaîne de connexion ou tout identificateur nécessaire de l’instance de base de données, généralement passé comme argument à la méthode de sélection du fournisseur mentionnée ci-dessus
- Tous les sélecteurs de comportements facultatifs au niveau du fournisseur, généralement chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur
- Tous les sélecteurs de comportements EF Core généraux, en général chaînés après ou avant la méthode du sélecteur du fournisseur

L’exemple suivant configure la `DbContextOptions` pour utiliser le fournisseur SQL Server, une connexion contenue dans la variable `connectionString`, un délai d’attente de commande au niveau du fournisseur et un sélecteur de comportement EF Core qui effectue toutes les requêtes exécutées dans le `DbContext` [sans suivi](xref:core/querying/tracking#no-tracking-queries) par défaut :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Les méthodes de sélection de fournisseur et d’autres méthodes de sélecteur de comportements mentionnées ci-dessus sont des méthodes d’extension sur des `DbContextOptions` ou des classes d’options spécifiques au fournisseur. Pour pouvoir accéder à ces méthodes d’extension, vous devrez peut-être disposer d’un espace de noms (généralement `Microsoft.EntityFrameworkCore`) dans la portée et inclure des dépendances de package supplémentaires dans le projet.

La `DbContextOptions` peut être fournie au `DbContext` en substituant la méthode `OnConfiguring` ou en externe à l’aide d’un argument de constructeur.

Si les deux sont utilisés, `OnConfiguring` est appliqué en dernier et peut remplacer les options fournies à l’argument du constructeur.

### <a name="constructor-argument"></a>Argument de constructeur

Votre constructeur peut simplement accepter un `DbContextOptions` comme suit :

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

Votre application peut maintenant passer le `DbContextOptions` lors de l’instanciation d’un contexte, comme suit :

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Vous pouvez également initialiser le `DbContextOptions` dans le contexte lui-même. Bien que vous puissiez utiliser cette technique pour la configuration de base, vous devrez généralement obtenir certains détails de configuration de l’extérieur, par exemple une chaîne de connexion de base de données. Pour ce faire, vous pouvez utiliser une API de configuration ou tout autre moyen.

Pour initialiser `DbContextOptions` dans le contexte, substituez la méthode `OnConfiguring` et appelez les méthodes sur le `DbContextOptionsBuilder`fourni :

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

Une application peut simplement instancier ce type de contexte sans passer quoi que ce soit à son constructeur :

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.

### <a name="using-dbcontext-with-dependency-injection"></a>Utilisation de DbContext avec l’injection de dépendances

EF Core prend en charge l’utilisation de `DbContext` avec un conteneur d’injection de dépendances. Votre type DbContext peut être ajouté au conteneur de service à l’aide de la méthode `AddDbContext<TContext>`.

`AddDbContext<TContext>` rendra à la fois votre type DbContext, `TContext`et les `DbContextOptions<TContext>` correspondants disponibles pour l’injection à partir du conteneur de service.

Pour plus d’informations sur l’injection de dépendances, voir [plus de lectures](#more-reading) ci-dessous.

Ajout de la `DbContext` à l’injection de dépendances :

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Cela nécessite l’ajout d’un [argument de constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions<TContext>`.

Code de contexte :

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Code d’application (dans ASP.NET Core) :

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

Code d’application (à l’aide de ServiceProvider directement, moins courant) :

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a>Éviter les problèmes de thread DbContext

Entity Framework Core ne prend pas en charge l’exécution de plusieurs opérations parallèles sur la même instance de `DbContext`. Cela comprend à la fois l’exécution parallèle des requêtes Async et toute utilisation simultanée explicite à partir de plusieurs threads. Par conséquent, toujours `await` appels asynchrones immédiatement, ou utiliser des instances `DbContext` distinctes pour les opérations qui s’exécutent en parallèle.

Lorsque EF Core détecte une tentative d’utilisation d’une instance de `DbContext` simultanément, vous voyez un `InvalidOperationException` avec un message semblable à celui-ci :

> Une deuxième opération a démarré sur ce contexte avant la fin d’une opération précédente. Cela est généralement dû à différents threads utilisant la même instance de DbContext, mais il n’est pas garanti que les membres d’instance soient thread-safe.

Lorsque l’accès simultané n’est pas détecté, cela peut entraîner un comportement non défini, des blocages d’application et des données endommagées.

Il existe des erreurs courantes qui peuvent entraîner par inadvertance un accès simultané sur la même instance de `DbContext` :

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Oubli de l’attente de la fin d’une opération asynchrone avant le démarrage d’une autre opération sur le même DbContext

Les méthodes asynchrones permettent à EF Core de lancer des opérations qui accèdent à la base de données de façon non bloquante. Toutefois, si un appelant n’attend pas la fin de l’une de ces méthodes et poursuit l’exécution d’autres opérations sur le `DbContext`, l’état du `DbContext` peut être, (et très probablement) endommagé.

Attendez toujours EF Core les méthodes asynchrones immédiatement.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Partage implicite d’instances de DbContext sur plusieurs threads via l’injection de dépendances

La méthode d’extension [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) inscrit par défaut les types `DbContext` avec une [durée de vie limitée](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .

Cela ne pose pas de problèmes d’accès simultané dans les applications ASP.NET Core, car il n’existe qu’un seul thread qui exécute chaque demande client à un moment donné, et parce que chaque demande obtient une étendue d’injection de dépendances distincte (et par conséquent une instance `DbContext` distincte).

Toutefois, tout code qui exécute explicitement plusieurs threads en parallèle doit s’assurer que les instances `DbContext` ne sont jamais accessibles simultanément.

Pour ce faire, vous pouvez utiliser l’injection de dépendances en inscrivant le contexte comme étant étendu et en créant des étendues (à l’aide de `IServiceScopeFactory`) pour chaque thread, ou en inscrivant le `DbContext` comme temporaire (à l’aide de la surcharge de `AddDbContext` qui accepte un paramètre `ServiceLifetime`).

## <a name="more-reading"></a>Plus de lectures

- Lisez [injection de dépendances](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) pour en savoir plus sur l’utilisation de di.
- Pour plus d’informations, consultez le [test](testing/index.md) .
