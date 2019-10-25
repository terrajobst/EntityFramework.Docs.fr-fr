---
title: Services au moment du design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811940"
---
# <a name="design-time-services"></a><span data-ttu-id="64538-102">Services au moment du design</span><span class="sxs-lookup"><span data-stu-id="64538-102">Design-time services</span></span>

<span data-ttu-id="64538-103">Certains services utilisés par les outils sont utilisés uniquement au moment du Design.</span><span class="sxs-lookup"><span data-stu-id="64538-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="64538-104">Ces services sont gérés séparément des services d’exécution de EF Core pour les empêcher d’être déployés avec votre application.</span><span class="sxs-lookup"><span data-stu-id="64538-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="64538-105">Pour remplacer l’un de ces services (par exemple, le service pour générer des fichiers de migration), ajoutez une implémentation de `IDesignTimeServices` à votre projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="64538-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
