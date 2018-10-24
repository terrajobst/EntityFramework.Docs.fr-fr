---
title: Informations de référence sur les outils Entity Framework Core - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447116"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="f5b2f-102">Informations de référence sur les outils Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f5b2f-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="f5b2f-103">Les outils Entity Framework Core aident à effectuer les tâches de développement au moment du design.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="f5b2f-104">Ils servent principalement à gérer les migrations et à structurer un `DbContext` et des types d’entité en reconstituant la logique du schéma d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="f5b2f-105">Les [outils de la Console du Gestionnaire de package EF Core](powershell.md)) s’exécutent dans la [Console du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console) dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-105">The [EF Core Package Manager Console tools](powershell.md)) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="f5b2f-106">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="f5b2f-107">Les [outils de l’interface de ligne de commande (CLI) EF Core .NET](dotnet.md) sont une extension multiplateforme des [outils CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="f5b2f-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="f5b2f-108">Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).</span><span class="sxs-lookup"><span data-stu-id="f5b2f-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="f5b2f-109">Les deux outils exposent les mêmes fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="f5b2f-110">Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la **Console du Gestionnaire de package**, car ils procurent une expérience plus intégrée.</span><span class="sxs-lookup"><span data-stu-id="f5b2f-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5b2f-111">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f5b2f-111">Next steps</span></span>

* [<span data-ttu-id="f5b2f-112">Informations de référence sur les outils de la Console du Gestionnaire de package EF Core</span><span class="sxs-lookup"><span data-stu-id="f5b2f-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="f5b2f-113">Informations de référence sur les outils CLI .NET EF Core</span><span class="sxs-lookup"><span data-stu-id="f5b2f-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)