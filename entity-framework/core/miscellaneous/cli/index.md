---
title: "Référence de la ligne de commande - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="1b9e2-102">Outils Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1b9e2-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="1b9e2-103">Les outils Entity Framework Core vous permettent de développer des applications EF Core.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="1b9e2-104">Ils servent principalement à structurer un DbContext et des types d’entité en reconstituant la logique du schéma d’une base de données et à gérer les migrations.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="1b9e2-105">Les [outils de la console du Gestionnaire de package EF Core][1] procurent une expérience de qualité supérieure dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="1b9e2-106">Exécutez-les dans la [console du Gestionnaire de package][2] de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="1b9e2-107">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="1b9e2-108">Les [outils en ligne de commande .NET EF Core][3] sont une extension des [outils de l’interface de ligne de commande .NET Core][4] qui sont multiplateformes et peuvent s’exécuter en dehors de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="1b9e2-109">Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).</span><span class="sxs-lookup"><span data-stu-id="1b9e2-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="1b9e2-110">Les deux outils exposent les mêmes fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="1b9e2-111">Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la console du Gestionnaire de package, car ils procurent une expérience plus intégrée.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="1b9e2-112">Frameworks</span><span class="sxs-lookup"><span data-stu-id="1b9e2-112">Frameworks</span></span>
----------
<span data-ttu-id="1b9e2-113">Les outils prennent en charge les projets qui ciblent .NET Framework ou .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="1b9e2-114">Si vous voulez utiliser une bibliothèque de classes, envisagez d’utiliser si possible une bibliothèque de classes .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="1b9e2-115">Ceci permet de rencontrer moins de problèmes avec les outils .NET.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="1b9e2-116">Si vous voulez utiliser à la place une bibliothèque de classes .NET Standard, vous devez utiliser un projet de démarrage qui cible .NET Framework ou .NET Core, pour que les outils aient une plateforme cible concrète dans laquelle ils peuvent charger votre bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="1b9e2-117">Ce projet de démarrage peut être un projet factice sans code réel ; il est nécessaire seulement pour fournir une cible aux outils.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="1b9e2-118">Si votre projet cible un autre framework (par exemple Windows universel ou Xamarin), vous devez créer une bibliothèque de classes .NET Standard distincte.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="1b9e2-119">Dans ce cas, suivez les instructions ci-dessus pour créer également un projet de démarrage qui peut être utilisé par les outils.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="1b9e2-120">Projets de démarrage et cible</span><span class="sxs-lookup"><span data-stu-id="1b9e2-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="1b9e2-121">Chaque fois que vous appelez une commande, deux projets sont impliqués : le projet cible et le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="1b9e2-122">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="1b9e2-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="1b9e2-123">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="1b9e2-124">Le projet cible et le projet de démarrage peuvent être le même.</span><span class="sxs-lookup"><span data-stu-id="1b9e2-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
