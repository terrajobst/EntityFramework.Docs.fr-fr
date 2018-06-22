---
title: Configuration d’un DbContext - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152388"
---
# <a name="configuring-a-dbcontext"></a>Configuration d’un DbContext

Cet article explique les modèles de base pour la configuration d’un `DbContext` via un `DbContextOptions` pour se connecter à une base de données à l’aide d’un fournisseur EF Core spécifique et les comportements facultatifs.

## <a name="design-time-dbcontext-configuration"></a>Configuration de DbContext au moment du design

EF Core au moment du design des outils tels que [migrations](xref:core/managing-schemas/migrations/index) doivent être en mesure de découvrir et de créer une instance de l’utilisation d’un `DbContext` type afin de regrouper des informations sur les types d’entité et comment ils sont mappés à un schéma de base de données de l’application. Ce processus peut être automatique, que l’outil peut facilement créer le `DbContext` de sorte qu’il sera configuré de la même façon à la façon dont il est configuré au moment de l’exécution.

Lors d’un modèle qui fournit les informations de configuration nécessaires à la `DbContext` peut utiliser au moment de l’exécution, les outils qui requièrent l’utilisation d’un `DbContext` au moment du design ne fonctionnent qu’avec un nombre limité de modèles. Elles sont traitées plus en détail dans les [la création du contexte au moment du Design](xref:core/miscellaneous/cli/dbcontext-creation) section.

## <a name="configuring-dbcontextoptions"></a>Configuration DbContextOptions

`DbContext` doit avoir une instance de `DbContextOptions` afin d’effectuer des tâches. Le `DbContextOptions` instance transfère les informations de configuration telles que :

- Le fournisseur de base de données à utiliser, généralement sélectionné en appelant une méthode comme `UseSqlServer` ou `UseSqlite`
- Toute chaîne de connexion nécessaire ou d’un identificateur de l’instance de la base de données, généralement passé en tant qu’argument à la méthode de sélection de fournisseur mentionnée ci-dessus
- Tous les sélecteurs de comportement de facultatif au niveau du fournisseur, en général également chaînés à l’intérieur de l’appel à la méthode de sélection du fournisseur
- Tous les sélecteurs de comportement générales EF Core, généralement chaînés après ou avant la méthode de sélection du fournisseur

L’exemple suivant configure le `DbContextOptions` pour utiliser le fournisseur SQL Server, une connexion contenue dans le `connectionString` variable, un délai d’attente au niveau du fournisseur de commande et un sélecteur de comportement EF Core qui rend toutes les requêtes exécutées dans le `DbContext` [non suivi](xref:core/querying/tracking#no-tracking-queries) par défaut :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Méthodes de sélection de fournisseur et d’autres méthodes de sélecteur de comportement mentionnés ci-dessus sont des méthodes d’extension sur `DbContextOptions` ou les classes d’option spécifique au fournisseur. Pour accéder à ces méthodes d’extension que vous devrez peut-être disposer d’un espace de noms (en général `Microsoft.EntityFrameworkCore`) dans l’étendue et inclure des dépendances de package supplémentaires dans le projet.

Le `DbContextOptions` peuvent être fournis à la `DbContext` en substituant la `OnConfiguring` méthode ou en externe via un argument de constructeur.

Si les deux sont utilisés, `OnConfiguring` est appliqué en dernier et peuvent remplacer des options fournies pour l’argument du constructeur.

### <a name="constructor-argument"></a>Argument du constructeur

Code de contexte avec constructeur :

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
> Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`, mais à l’aide de la version non générique n’est pas recommandé pour les applications avec plusieurs types de contexte.

Code de l’application d’initialiser à partir de l’argument du constructeur :

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Code de contexte avec `OnConfiguring`:

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

Code d’application pour initialiser un `DbContext` qui utilise `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Cette approche ne se prête pas au test, à moins que les tests ciblent la base de données complète.

### <a name="using-dbcontext-with-dependency-injection"></a>À l’aide de DbContext avec injection de dépendance

EF Core prend en charge à l’aide de `DbContext` avec un conteneur d’injection de dépendance. Le type DbContext peut être ajouté au conteneur de service à l’aide de la `AddDbContext<TContext>` (méthode).

`AddDbContext<TContext>` permettront à la fois votre type DbContext, `TContext`et le correspondant `DbContextOptions<TContext>` disponibles pour l’injection de code à partir du conteneur de service.

Consultez [lecture plus](#more-reading) ci-dessous pour plus d’informations sur l’injection de dépendances.

Ajout de la `Dbcontext` pour l’injection de dépendances :

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Cela requiert l’ajout d’un [argument du constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions<TContext>`.

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

Code d’application (à l’aide de ServiceProvider directement, moins courants) :

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Plus la lecture

* Lecture [mise en route sur ASP.NET Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation de EF avec ASP.NET Core.
* Lecture [Injection de dépendance](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) pour en savoir plus sur l’utilisation de DI.
* Lecture [test](testing/index.md) pour plus d’informations.
