---
title: "Au moment du design DbContext création - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="30e39-102">Au moment du design DbContext création</span><span class="sxs-lookup"><span data-stu-id="30e39-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="30e39-103">Certains outils EF commandes nécessitent une instance de DbContext doit être créé lors de la conception de temps (par exemple, lors de l’exécution des commandes de Migrations).</span><span class="sxs-lookup"><span data-stu-id="30e39-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="30e39-104">Il existe différentes façons que les outils réessayez de le créer.</span><span class="sxs-lookup"><span data-stu-id="30e39-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="30e39-105">À partir des services d’application</span><span class="sxs-lookup"><span data-stu-id="30e39-105">From application services</span></span>
-------------------------
<span data-ttu-id="30e39-106">Si votre projet de démarrage est une application ASP.NET Core, les outils de tenter d’obtenir l’objet DbContext à partir du fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="30e39-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="30e39-107">Il l’obtenir en appelant `Program.BuildWebHost()` et l’accès à la `IWebHost.Services` propriété.</span><span class="sxs-lookup"><span data-stu-id="30e39-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="30e39-108">N’importe quel DbContext inscrit à l’aide de `IServiceCollection.AddDbContext<TContext>()` peuvent être trouvés et créées de cette façon.</span><span class="sxs-lookup"><span data-stu-id="30e39-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="30e39-109">Ce modèle a été [introduit dans ASP.NET Core 2.0][1]</span><span class="sxs-lookup"><span data-stu-id="30e39-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="30e39-110">À l’aide du constructeur par défaut</span><span class="sxs-lookup"><span data-stu-id="30e39-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="30e39-111">Si le DbContext ne peut pas être obtenu à partir du fournisseur de service d’application, les outils de recherche le type DbContext situé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="30e39-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="30e39-112">Ils tenteront de le créer à l’aide de son constructeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="30e39-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="30e39-113">À partir d’une fabrique au moment du design</span><span class="sxs-lookup"><span data-stu-id="30e39-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="30e39-114">Vous pouvez également indiquer les outils de la création de votre DbContext en implémentant `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="30e39-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="30e39-115">Si une classe qui implémente cette interface est trouvée à l’intérieur de votre projet, les outils de contournent les autres façons de créer le DbContext.</span><span class="sxs-lookup"><span data-stu-id="30e39-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="30e39-116">Ils utilisent toujours la fabrique au moment du design.</span><span class="sxs-lookup"><span data-stu-id="30e39-116">They always use the factory at design time.</span></span> <span data-ttu-id="30e39-117">Une fabrique est particulièrement utile si vous avez besoin configurer le DbContext différemment pour le moment de la conception que lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="30e39-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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

> [!NOTE]
> <span data-ttu-id="30e39-118">Le `args` paramètre est actuellement inutilisé.</span><span class="sxs-lookup"><span data-stu-id="30e39-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="30e39-119">Il est [un problème] [ 2] la possibilité de spécifier des arguments au moment du design à partir des outils de suivi.</span><span class="sxs-lookup"><span data-stu-id="30e39-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
