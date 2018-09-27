---
title: Création au moment du design DbContext - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993716"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="e5d76-102">Création de DbContext d’au moment du design</span><span class="sxs-lookup"><span data-stu-id="e5d76-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="e5d76-103">Certaines commandes outils EF Core (par exemple, le [Migrations][1] commandes) nécessitent une dérivée `DbContext` instance doit être créé au moment du design afin de collecter des informations détaillées sur l’application types d’entité et comment elles correspondent à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="e5d76-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="e5d76-104">Dans la plupart des cas, il est souhaitable que le `DbContext` ainsi créée est configuré de manière similaire à la façon dont il serait [configuré au moment de l’exécution][2].</span><span class="sxs-lookup"><span data-stu-id="e5d76-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="e5d76-105">Il existe différentes façons les outils essaie de créer le `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="e5d76-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="e5d76-106">À partir des services d’application</span><span class="sxs-lookup"><span data-stu-id="e5d76-106">From application services</span></span>
-------------------------
<span data-ttu-id="e5d76-107">Si votre projet de démarrage est une application ASP.NET Core, les outils d’essayer d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="e5d76-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="e5d76-108">Les outils essayer tout d’abord d’obtenir le fournisseur de services en appelant `Program.BuildWebHost()` et l’accès à la `IWebHost.Services` propriété.</span><span class="sxs-lookup"><span data-stu-id="e5d76-108">The tools first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="e5d76-109">Lorsque vous créez une nouvelle application ASP.NET Core 2.0, ce hook est inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="e5d76-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="e5d76-110">Dans les versions précédentes d’EF Core et ASP.NET Core, les outils tentez d’appeler `Startup.ConfigureServices` directement afin d’obtenir de fournisseur de services de l’application, mais ce modèle n’est plus fonctionne correctement dans les applications ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e5d76-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="e5d76-111">Si vous mettez à niveau une application 1.x de ASP.NET Core 2.0, vous pouvez [modifier votre `Program` classe à suivre le nouveau modèle][3].</span><span class="sxs-lookup"><span data-stu-id="e5d76-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="e5d76-112">Le `DbContext` lui-même et les dépendances dans son constructeur doivent être inscrits en tant que services dans le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="e5d76-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="e5d76-113">Vous pouvez facilement faire en ayant [un constructeur sur la `DbContext` qui prend une instance de `DbContextOptions<TContext>` en tant qu’argument][ 4] et à l’aide de la [`AddDbContext<TContext>` (méthode)][5].</span><span class="sxs-lookup"><span data-stu-id="e5d76-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="e5d76-114">À l’aide d’un constructeur sans paramètres</span><span class="sxs-lookup"><span data-stu-id="e5d76-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="e5d76-115">Si le DbContext ne peut pas être obtenu à partir du fournisseur de service d’application, les outils Recherchez dérivé `DbContext` type dans le projet.</span><span class="sxs-lookup"><span data-stu-id="e5d76-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="e5d76-116">Puis, ils tentent de créer une instance à l’aide d’un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="e5d76-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="e5d76-117">Il peut s’agir du constructeur par défaut si le `DbContext` est configuré à l’aide de la [`OnConfiguring`][6] (méthode).</span><span class="sxs-lookup"><span data-stu-id="e5d76-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="e5d76-118">À partir d’une fabrique au moment du design</span><span class="sxs-lookup"><span data-stu-id="e5d76-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="e5d76-119">Vous pouvez également indiquer aux outils comment créer votre DbContext en implémentant la `IDesignTimeDbContextFactory<TContext>` interface : si une classe qui implémente cette interface se trouve dans le même projet que dérivé `DbContext` ou dans le projet de démarrage de l’application, les outils de contournement les autres méthodes de création de DbContext et utiliser la fabrique au moment du design à la place.</span><span class="sxs-lookup"><span data-stu-id="e5d76-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="e5d76-120">Le `args` paramètre est actuellement pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="e5d76-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="e5d76-121">Il est [un problème][7] la possibilité de spécifier des arguments au moment du design à partir des outils de suivi.</span><span class="sxs-lookup"><span data-stu-id="e5d76-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="e5d76-122">Une fabrique au moment du design peut être particulièrement utile si vous avez besoin configurer le DbContext différemment pour le moment du design qu’au moment de l’exécution, si le `DbContext` prend constructeur paramètres supplémentaires ne sont pas inscrits dans l’injection de dépendances, si vous n’utilisez pas du tout l’injection de dépendances, ou si pour certains vous préférez ne pas avoir de raison un `BuildWebHost` méthode dans votre application ASP.NET Core `Main` classe.</span><span class="sxs-lookup"><span data-stu-id="e5d76-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
