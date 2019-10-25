---
title: Création de DbContext au moment du design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812021"
---
# <a name="design-time-dbcontext-creation"></a>Création de DbContext au moment de la conception

Certaines des commandes des outils de EF Core (par exemple, les commandes de [migration][1] ) requièrent la création d’une instance de `DbContext` dérivée au moment de la conception afin de collecter des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données. Dans la plupart des cas, il est souhaitable que le `DbContext` créé soit configuré de manière similaire à la façon dont il est [configuré au moment][2]de l’exécution.

Les outils essaient de créer le `DbContext`de différentes manières :

## <a name="from-application-services"></a>À partir des services d’application

Si votre projet de démarrage utilise l' [hôte Web ASP.net Core][3] ou l' [hôte générique .net Core][4], les outils essaient d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.

Les outils essaient d’abord d’obtenir le fournisseur de services en appelant `Program.CreateHostBuilder()`, en appelant `Build()`, puis en accédant à la propriété `Services`.

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

La `DbContext` elle-même et toutes les dépendances de son constructeur doivent être inscrites en tant que services dans le fournisseur de services de l’application. Pour ce faire, il est facile d’avoir [un constructeur sur la `DbContext` qui accepte une instance de `DbContextOptions<TContext>` en tant qu’argument][5] et utilise la [méthode`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Utilisation d’un constructeur sans paramètres

Si DbContext ne peut pas être obtenu à partir du fournisseur de services d’application, les outils recherchent le type de `DbContext` dérivé dans le projet. Ensuite, ils essaient de créer une instance à l’aide d’un constructeur sans paramètres. Il peut s’agir du constructeur par défaut si la `DbContext` est configurée à l’aide de la méthode [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>À partir d’une fabrique au moment de la conception

Vous pouvez également indiquer aux outils comment créer votre DbContext en implémentant l’interface `IDesignTimeDbContextFactory<TContext>` : si une classe qui implémente cette interface est trouvée dans le même projet que le `DbContext` dérivé ou dans le projet de démarrage de l’application, les outils ignorent l’autre méthodes de création de DbContext et d’utilisation de la fabrique au moment du Design.

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
> Le paramètre `args` est actuellement inutilisé. [Un problème][8] est survenu lors du suivi de la capacité à spécifier des arguments au moment du design à partir des outils.

Une fabrique au moment de la conception peut être particulièrement utile si vous devez configurer la DbContext différemment pour le moment de la conception plutôt qu’au moment de l’exécution, si le constructeur `DbContext` prend des paramètres supplémentaires qui ne sont pas inscrits dans DI, si vous n’utilisez pas de DI du tout ou si, pour une raison quelconque, vous préférez ne pas avoir de méthode `BuildWebHost` dans la classe `Main` de votre application ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
