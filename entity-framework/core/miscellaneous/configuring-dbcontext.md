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
# <a name="configuring-a-dbcontext"></a>Configuration d’un DbContext

Cet article présente les modèles pour configurer un `DbContext` avec `DbContextOptions`. Options sont principalement utilisées pour sélectionner et configurer le magasin de données.

## <a name="configuring-dbcontextoptions"></a>Configuration DbContextOptions

`DbContext`doit avoir une instance de `DbContextOptions` afin d’exécuter. Ceci peut être configuré en substituant `OnConfiguring`, ou fourni en externe via un argument de constructeur.

Si les deux sont utilisés, `OnConfiguring` est exécutée sur les options fournies, ce qui signifie qu’il est additif et peuvent remplacer des options fournies pour l’argument du constructeur.

### <a name="constructor-argument"></a>Argument du constructeur

Code de contexte avec un constructeur

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
> Le constructeur de base de DbContext accepte également la version non générique de `DbContextOptions`. À l’aide de la version non générique n’est pas recommandé pour les applications avec plusieurs types de contexte.

Code de l’application d’initialiser à partir de l’argument du constructeur

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`dernière et peuvent remplacer des options obtenues à partir de DI ou le constructeur. Cette approche ne se prête pas à tester (sauf si vous ciblez la base de données complète).

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

Code d’application pour l’initialiser avec `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>À l’aide de DbContext avec injection de dépendance

EF prend en charge à l’aide de `DbContext` avec un conteneur d’injection de dépendance. Le type DbContext peut être ajouté au conteneur de service à l’aide de `AddDbContext<TContext>`.

`AddDbContext`permettront à la fois votre type DbContext, `TContext`, et `DbContextOptions<TContext>` disponibles pour l’injection de code à partir du conteneur de service.

Consultez [lecture plus](#more-reading) ci-dessous pour plus d’informations sur l’injection de dépendances.

Ajout de dbcontext risque d’injection de dépendance

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Cela requiert l’ajout d’un [argument du constructeur](#constructor-argument) à votre type DbContext qui accepte `DbContextOptions`.

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
public MyController(BloggingContext context)
```

Code d’application (à l’aide de ServiceProvider directement, moins courants) :

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>À l’aide de`IDesignTimeDbContextFactory<TContext>`

Comme alternative aux options ci-dessus, vous pouvez également fournir une implémentation de `IDesignTimeDbContextFactory<TContext>`. Outils de la EF peuvent utiliser cette fabrique pour créer une instance de votre DbContext. Cela peut être nécessaire pour permettre des expériences au moment du design spécifiques tels que des migrations.

Implémentez cette interface pour permettre aux services au moment du design pour les types de contexte qui n’ont pas de constructeur public par défaut. Les services au moment du design détecte automatiquement les implémentations de cette interface qui se trouvent dans le même assembly que le contexte dérivé.

Exemple :

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

## <a name="more-reading"></a>Plus la lecture

* Lecture [mise en route sur ASP.NET Core](../get-started/aspnetcore/index.md) pour plus d’informations sur l’utilisation de EF avec ASP.NET Core.
* Lecture [Injection de dépendance](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) pour en savoir plus sur l’utilisation de DI.
* Lecture [test](testing/index.md) pour plus d’informations.
