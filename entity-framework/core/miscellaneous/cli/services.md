---
title: Services au moment du design - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="48277-102">Services au moment du design</span><span class="sxs-lookup"><span data-stu-id="48277-102">Design-time services</span></span>
====================
<span data-ttu-id="48277-103">Certains services utilisées par les outils sont utilisés uniquement au moment du design.</span><span class="sxs-lookup"><span data-stu-id="48277-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="48277-104">Ces services sont gérés séparément à partir des services d’exécution de EF Core pour les empêcher d’être déployés avec votre application.</span><span class="sxs-lookup"><span data-stu-id="48277-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="48277-105">Pour remplacer un de ces services (par exemple, le service pour générer des fichiers de migration), ajout d’une implémentation de `IDesignTimeServices` à votre projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="48277-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
