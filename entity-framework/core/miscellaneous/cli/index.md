---
title: "Référence de la ligne de commande - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="c5082-102">Outils Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c5082-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="c5082-103">Les outils Entity Framework Core vous permettent de développer des applications EF Core.</span><span class="sxs-lookup"><span data-stu-id="c5082-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="c5082-104">Ils servent principalement à structurer un DbContext et des types d’entité en reconstituant la logique du schéma d’une base de données et à gérer les migrations.</span><span class="sxs-lookup"><span data-stu-id="c5082-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="c5082-105">Les [outils de la console du Gestionnaire de package EF Core][1] procurent une expérience de qualité supérieure dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5082-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="c5082-106">Exécutez-les dans la [console du Gestionnaire de package][2] de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c5082-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="c5082-107">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5082-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="c5082-108">Les [outils en ligne de commande .NET EF Core][3] sont une extension des [outils de l’interface de ligne de commande .NET Core][4] qui sont multiplateformes et peuvent s’exécuter en dehors de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5082-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="c5082-109">Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).</span><span class="sxs-lookup"><span data-stu-id="c5082-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="c5082-110">Les deux outils exposent les mêmes fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="c5082-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="c5082-111">Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la console du Gestionnaire de package, car ils procurent une expérience plus intégrée.</span><span class="sxs-lookup"><span data-stu-id="c5082-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="c5082-112">Frameworks</span><span class="sxs-lookup"><span data-stu-id="c5082-112">Frameworks</span></span>
----------
<span data-ttu-id="c5082-113">Les outils prennent en charge les projets qui ciblent .NET Framework ou .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5082-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="c5082-114">Si votre projet cible un autre framework (par exemple, Windows universel ou Xamarin), nous vous recommandons de créer un projet .NET Standard distinct et d’effectuer un ciblage croisé d’un des frameworks pris en charge.</span><span class="sxs-lookup"><span data-stu-id="c5082-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="c5082-115">Pour le ciblage croisé de .NET Core, par exemple, cliquez avec le bouton droit sur le projet, puis sélectionnez **Modifier \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="c5082-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="c5082-116">Mettez à jour la propriété `TargetFramework` comme suit.</span><span class="sxs-lookup"><span data-stu-id="c5082-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="c5082-117">(Notez la forme plurielle du nom de la propriété.)</span><span class="sxs-lookup"><span data-stu-id="c5082-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="c5082-118">Si vous utilisez une bibliothèque de classes .NET Standard, vous n’avez pas besoin d’effectuer un ciblage croisé si votre projet de démarrage cible .NET Framework ou .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5082-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="c5082-119">Projets de démarrage et cible</span><span class="sxs-lookup"><span data-stu-id="c5082-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="c5082-120">Chaque fois que vous appelez une commande, deux projets sont impliqués : le projet cible et le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="c5082-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="c5082-121">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="c5082-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="c5082-122">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="c5082-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="c5082-123">Le projet cible et le projet de démarrage peuvent être le même.</span><span class="sxs-lookup"><span data-stu-id="c5082-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
