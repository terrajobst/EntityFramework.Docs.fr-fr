---
title: Création de DbContext au moment du design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197576"
---
<a name="design-time-dbcontext-creation"></a>Création de DbContext au moment du design
==============================
Certaines des commandes des outils de EF Core (par exemple, les commandes de [migration][1] ) requièrent `DbContext` la création d’une instance dérivée au moment de la conception afin de collecter des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données. Dans la plupart des cas, il est souhaitable `DbContext` que le créé soit configuré de manière similaire à la façon dont il est [configuré au moment][2]de l’exécution.

Les outils essaient de créer l’un `DbContext`des moyens suivants :

<a name="from-application-services"></a>À partir des services d’application
-------------------------
Si votre projet de démarrage utilise l' [hôte Web ASP.net Core][3] ou l' [hôte générique .net Core][4], les outils essaient d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.

Les outils essaient d’abord d’obtenir le fournisseur de services en `Program.CreateHostBuilder()`appelant, `Build()`en appelant, puis en `Services` accédant à la propriété.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Lorsque vous créez une application de ASP.NET Core, ce hook est inclus par défaut.

`DbContext` Lui-même et toutes les dépendances de son constructeur doivent être inscrits en tant que services dans le fournisseur de services de l’application. Pour ce faire, il est facile d’avoir [un constructeur `DbContext` sur qui accepte une instance `DbContextOptions<TContext>` de comme argument][5] et utilise la [ `AddDbContext<TContext>` méthode][6].

<a name="using-a-constructor-with-no-parameters"></a>Utilisation d’un constructeur sans paramètres
--------------------------------------
Si la DbContext ne peut pas être obtenue à partir du fournisseur de services d’application, les `DbContext` outils recherchent le type dérivé dans le projet. Ensuite, ils essaient de créer une instance à l’aide d’un constructeur sans paramètres. Il peut s’agir du constructeur par défaut `DbContext` si le est configuré [`OnConfiguring`][7] à l’aide de la méthode.

<a name="from-a-design-time-factory"></a>À partir d’une fabrique au moment de la conception
--------------------------
Vous pouvez également indiquer aux outils comment créer votre DbContext en implémentant l' `IDesignTimeDbContextFactory<TContext>` interface : Si une classe implémentant cette interface est trouvée dans le même projet que le dérivé `DbContext` ou dans le projet de démarrage de l’application, les outils ignorent les autres méthodes de création de DbContext et utilisent la fabrique au moment du design à la place.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> Le `args` paramètre est actuellement inutilisé. [Un problème][8] est survenu lors du suivi de la capacité à spécifier des arguments au moment du design à partir des outils.

Une fabrique au moment du design peut être particulièrement utile si vous avez besoin de configurer DbContext différemment pour le moment de la conception plutôt qu’au `DbContext` moment de l’exécution, si le constructeur prend des paramètres supplémentaires qui ne sont pas inscrits dans di, si vous n’utilisez pas de di du tout ou si pour certains la raison pour laquelle vous préférez ne `BuildWebHost` pas avoir de méthode dans la `Main` classe de votre application ASP.net core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
