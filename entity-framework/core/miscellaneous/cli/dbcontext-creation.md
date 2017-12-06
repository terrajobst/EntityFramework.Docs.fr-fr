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
<a name="design-time-dbcontext-creation"></a>Au moment du design DbContext création
==============================
Certains outils EF commandes nécessitent une instance de DbContext doit être créé lors de la conception de temps (par exemple, lors de l’exécution des commandes de Migrations). Il existe différentes façons que les outils réessayez de le créer.

<a name="from-application-services"></a>À partir des services d’application
-------------------------
Si votre projet de démarrage est une application ASP.NET Core, les outils de tenter d’obtenir l’objet DbContext à partir du fournisseur de services de l’application. Il l’obtenir en appelant `Program.BuildWebHost()` et l’accès à la `IWebHost.Services` propriété. N’importe quel DbContext inscrit à l’aide de `IServiceCollection.AddDbContext<TContext>()` peuvent être trouvés et créées de cette façon. Ce modèle a été [introduit dans ASP.NET Core 2.0][1]

<a name="using-the-default-constructor"></a>À l’aide du constructeur par défaut
-----------------------------
Si le DbContext ne peut pas être obtenu à partir du fournisseur de service d’application, les outils de recherche le type DbContext situé dans le projet. Ils tenteront de le créer à l’aide de son constructeur par défaut.

<a name="from-a-design-time-factory"></a>À partir d’une fabrique au moment du design
--------------------------
Vous pouvez également indiquer les outils de la création de votre DbContext en implémentant `IDesignTimeDbContextFactory`. Si une classe qui implémente cette interface est trouvée à l’intérieur de votre projet, les outils de contournent les autres façons de créer le DbContext.
Ils utilisent toujours la fabrique au moment du design. Une fabrique est particulièrement utile si vous avez besoin configurer le DbContext différemment pour le moment de la conception que lors de l’exécution.

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
> Le `args` paramètre est actuellement inutilisé. Il est [un problème] [ 2] la possibilité de spécifier des arguments au moment du design à partir des outils de suivi.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
