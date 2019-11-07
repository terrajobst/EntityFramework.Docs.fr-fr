---
title: Création de DbContext au moment du design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655526"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="f0f14-102">Création de DbContext au moment de la conception</span><span class="sxs-lookup"><span data-stu-id="f0f14-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="f0f14-103">Certaines des commandes des outils de EF Core (par exemple, les commandes de [migration][1] ) requièrent la création d’une instance de `DbContext` dérivée au moment de la conception afin de collecter des détails sur les types d’entité de l’application et la façon dont ils sont mappés à un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="f0f14-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="f0f14-104">Dans la plupart des cas, il est souhaitable que le `DbContext` créé soit configuré de manière similaire à la façon dont il est [configuré au moment][2]de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f0f14-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="f0f14-105">Les outils essaient de créer le `DbContext`de différentes manières :</span><span class="sxs-lookup"><span data-stu-id="f0f14-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="f0f14-106">À partir des services d’application</span><span class="sxs-lookup"><span data-stu-id="f0f14-106">From application services</span></span>

<span data-ttu-id="f0f14-107">Si votre projet de démarrage utilise l' [hôte Web ASP.net Core][3] ou l' [hôte générique .net Core][4], les outils essaient d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0f14-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="f0f14-108">Les outils essaient d’abord d’obtenir le fournisseur de services en appelant `Program.CreateHostBuilder()`, en appelant `Build()`, puis en accédant à la propriété `Services`.</span><span class="sxs-lookup"><span data-stu-id="f0f14-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="f0f14-109">Lorsque vous créez une application de ASP.NET Core, ce hook est inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="f0f14-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="f0f14-110">La `DbContext` elle-même et toutes les dépendances de son constructeur doivent être inscrites en tant que services dans le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0f14-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="f0f14-111">Pour ce faire, il est facile d’avoir [un constructeur sur la `DbContext` qui accepte une instance de `DbContextOptions<TContext>` en tant qu’argument][5] et utilise la [méthode`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="f0f14-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="f0f14-112">Utilisation d’un constructeur sans paramètres</span><span class="sxs-lookup"><span data-stu-id="f0f14-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="f0f14-113">Si DbContext ne peut pas être obtenu à partir du fournisseur de services d’application, les outils recherchent le type de `DbContext` dérivé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="f0f14-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="f0f14-114">Ensuite, ils essaient de créer une instance à l’aide d’un constructeur sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="f0f14-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="f0f14-115">Il peut s’agir du constructeur par défaut si la `DbContext` est configurée à l’aide de la méthode [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="f0f14-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="f0f14-116">À partir d’une fabrique au moment de la conception</span><span class="sxs-lookup"><span data-stu-id="f0f14-116">From a design-time factory</span></span>

<span data-ttu-id="f0f14-117">Vous pouvez également indiquer aux outils comment créer votre DbContext en implémentant l’interface `IDesignTimeDbContextFactory<TContext>` : si une classe qui implémente cette interface est trouvée dans le même projet que le `DbContext` dérivé ou dans le projet de démarrage de l’application, les outils ignorent l’autre méthodes de création de DbContext et d’utilisation de la fabrique au moment du Design.</span><span class="sxs-lookup"><span data-stu-id="f0f14-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="f0f14-118">Le paramètre `args` est actuellement inutilisé.</span><span class="sxs-lookup"><span data-stu-id="f0f14-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="f0f14-119">[Un problème][8] est survenu lors du suivi de la capacité à spécifier des arguments au moment du design à partir des outils.</span><span class="sxs-lookup"><span data-stu-id="f0f14-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="f0f14-120">Une fabrique au moment de la conception peut être particulièrement utile si vous devez configurer la DbContext différemment pour le moment de la conception plutôt qu’au moment de l’exécution, si le constructeur `DbContext` prend des paramètres supplémentaires qui ne sont pas inscrits dans DI, si vous n’utilisez pas de DI du tout ou si, pour une raison quelconque, vous préférez ne pas avoir de méthode `BuildWebHost` dans la classe `Main` de votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0f14-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
