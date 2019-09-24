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
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="d02bb-102">Création de DbContext au moment du design</span><span class="sxs-lookup"><span data-stu-id="d02bb-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="d02bb-103">Certaines des commandes des outils de EF Core (par exemple, les commandes de [migration][1] ) requièrent `DbContext` la création d’une instance dérivée au moment de la conception afin de collecter des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="d02bb-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="d02bb-104">Dans la plupart des cas, il est souhaitable `DbContext` que le créé soit configuré de manière similaire à la façon dont il est [configuré au moment][2]de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="d02bb-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="d02bb-105">Les outils essaient de créer l’un `DbContext`des moyens suivants :</span><span class="sxs-lookup"><span data-stu-id="d02bb-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="d02bb-106">À partir des services d’application</span><span class="sxs-lookup"><span data-stu-id="d02bb-106">From application services</span></span>
-------------------------
<span data-ttu-id="d02bb-107">Si votre projet de démarrage utilise l' [hôte Web ASP.net Core][3] ou l' [hôte générique .net Core][4], les outils essaient d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="d02bb-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="d02bb-108">Les outils essaient d’abord d’obtenir le fournisseur de services en `Program.CreateHostBuilder()`appelant, `Build()`en appelant, puis en `Services` accédant à la propriété.</span><span class="sxs-lookup"><span data-stu-id="d02bb-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

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
> <span data-ttu-id="d02bb-109">Lorsque vous créez une application de ASP.NET Core, ce hook est inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="d02bb-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="d02bb-110">`DbContext` Lui-même et toutes les dépendances de son constructeur doivent être inscrits en tant que services dans le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="d02bb-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="d02bb-111">Pour ce faire, il est facile d’avoir [un constructeur `DbContext` sur qui accepte une instance `DbContextOptions<TContext>` de comme argument][5] et utilise la [ `AddDbContext<TContext>` méthode][6].</span><span class="sxs-lookup"><span data-stu-id="d02bb-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="d02bb-112">Utilisation d’un constructeur sans paramètres</span><span class="sxs-lookup"><span data-stu-id="d02bb-112">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="d02bb-113">Si la DbContext ne peut pas être obtenue à partir du fournisseur de services d’application, les `DbContext` outils recherchent le type dérivé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="d02bb-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="d02bb-114">Ensuite, ils essaient de créer une instance à l’aide d’un constructeur sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="d02bb-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="d02bb-115">Il peut s’agir du constructeur par défaut `DbContext` si le est configuré [`OnConfiguring`][7] à l’aide de la méthode.</span><span class="sxs-lookup"><span data-stu-id="d02bb-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="d02bb-116">À partir d’une fabrique au moment de la conception</span><span class="sxs-lookup"><span data-stu-id="d02bb-116">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="d02bb-117">Vous pouvez également indiquer aux outils comment créer votre DbContext en implémentant l' `IDesignTimeDbContextFactory<TContext>` interface : Si une classe implémentant cette interface est trouvée dans le même projet que le dérivé `DbContext` ou dans le projet de démarrage de l’application, les outils ignorent les autres méthodes de création de DbContext et utilisent la fabrique au moment du design à la place.</span><span class="sxs-lookup"><span data-stu-id="d02bb-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="d02bb-118">Le `args` paramètre est actuellement inutilisé.</span><span class="sxs-lookup"><span data-stu-id="d02bb-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="d02bb-119">[Un problème][8] est survenu lors du suivi de la capacité à spécifier des arguments au moment du design à partir des outils.</span><span class="sxs-lookup"><span data-stu-id="d02bb-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="d02bb-120">Une fabrique au moment du design peut être particulièrement utile si vous avez besoin de configurer DbContext différemment pour le moment de la conception plutôt qu’au `DbContext` moment de l’exécution, si le constructeur prend des paramètres supplémentaires qui ne sont pas inscrits dans di, si vous n’utilisez pas de di du tout ou si pour certains la raison pour laquelle vous préférez ne `BuildWebHost` pas avoir de méthode dans la `Main` classe de votre application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="d02bb-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
