---
title: Comparer EF Core et EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002754"
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="b0828-102">Comparer EF Core et EF6</span><span class="sxs-lookup"><span data-stu-id="b0828-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="b0828-103">Il existe deux versions différentes d’Entity Framework : Entity Framework Core et Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b0828-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="b0828-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b0828-104">Entity Framework 6</span></span>

<span data-ttu-id="b0828-105">Entity Framework 6 (EF6) est une technologie d’accès aux données éprouvée, connue depuis de nombreuses années pour sa stabilité et la richesse de ses fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="b0828-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="b0828-106">Elle a été publiée pour la première fois en 2008, dans le cadre de .NET Framework 3.5 SP1 et de Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="b0828-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="b0828-107">Depuis la version EF4.1, elle est disponible en tant que [package EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/), l’un des packages les plus populaires sur NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="b0828-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="b0828-108">EF6 continue d’être un produit pris en charge et de bénéficier de correctifs de bogues et d’améliorations mineures pendant un certain temps.</span><span class="sxs-lookup"><span data-stu-id="b0828-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="b0828-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b0828-109">Entity Framework Core</span></span>

<span data-ttu-id="b0828-110">Entity Framework Core (EF Core) est une version légère, extensible et multiplateforme d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0828-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="b0828-111">EF Core introduit de nombreuses améliorations et nouvelles fonctionnalités par rapport à EF6.</span><span class="sxs-lookup"><span data-stu-id="b0828-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="b0828-112">Par ailleurs, EF Core est une nouvelle base de code, qui n’a pas la maturité d’EF6.</span><span class="sxs-lookup"><span data-stu-id="b0828-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="b0828-113">EF Core conserve l’expérience de développement d’EF6, et la plupart des API de niveau supérieur demeurent les mêmes ; l’utilisation d’EF Core est donc très proche de celle d’EF6.</span><span class="sxs-lookup"><span data-stu-id="b0828-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="b0828-114">Parallèlement, EF Core repose sur un tout nouvel ensemble de composants de base.</span><span class="sxs-lookup"><span data-stu-id="b0828-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="b0828-115">Cela signifie qu’EF Core n’hérite pas automatiquement de toutes les fonctionnalités d’EF6.</span><span class="sxs-lookup"><span data-stu-id="b0828-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="b0828-116">Certaines de ces fonctionnalités apparaîtront dans les prochaines versions, tandis que d’autres, moins couramment utilisées, ne seront pas implémentées dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="b0828-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="b0828-117">Le nouveau cœur extensible et léger nous a permis d’ajouter des fonctionnalités à EF Core qui ne seront pas implémentées dans EF6 (telles que les clés secondaires, les mises à jour par lot et l’évaluation client/base de données mixte dans les requêtes LINQ).</span><span class="sxs-lookup"><span data-stu-id="b0828-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
