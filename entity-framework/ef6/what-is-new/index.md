---
title: Nouveautés - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149128"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="529cc-102">Nouveautés dans EF6</span><span class="sxs-lookup"><span data-stu-id="529cc-102">What's New in EF6</span></span>

<span data-ttu-id="529cc-103">Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.</span><span class="sxs-lookup"><span data-stu-id="529cc-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="529cc-104">Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.</span><span class="sxs-lookup"><span data-stu-id="529cc-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="529cc-105">Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="529cc-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="529cc-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="529cc-106">EF 6.3.0</span></span>

<span data-ttu-id="529cc-107">Le runtime EF 6.3.0 a été publié sur NuGet en septembre 2019.</span><span class="sxs-lookup"><span data-stu-id="529cc-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="529cc-108">L’objectif principal de cette version était de faciliter la migration des applications existantes qui utilisent EF 6 vers .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="529cc-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="529cc-109">La communauté a également contribué à plusieurs correctifs de bogues et améliorations.</span><span class="sxs-lookup"><span data-stu-id="529cc-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="529cc-110">Pour plus d’informations, consultez les problèmes fermés à chaque [jalon](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la version 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="529cc-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="529cc-111">Voici les plus connus :</span><span class="sxs-lookup"><span data-stu-id="529cc-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="529cc-112">Prise en charge de .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="529cc-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="529cc-113">Le package EntityFramework cible maintenant .NET Standard 2.1 en plus de .NET Framework 4.x</span><span class="sxs-lookup"><span data-stu-id="529cc-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="529cc-114">Les commandes de migrations ont été réécrites pour s’exécuter hors processus et fonctionner avec des projets de type SDK</span><span class="sxs-lookup"><span data-stu-id="529cc-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="529cc-115">Prise en charge de SQL Server HierarchyId</span><span class="sxs-lookup"><span data-stu-id="529cc-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="529cc-116">Compatibilité améliorée avec Roslyn et NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="529cc-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="529cc-117">Ajout de ef6.exe pour l’activation, l’ajout, l’écriture de scripts et l’application de migrations à partir d’assemblys.</span><span class="sxs-lookup"><span data-stu-id="529cc-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="529cc-118">Cela remplace migrate.exe</span><span class="sxs-lookup"><span data-stu-id="529cc-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="529cc-119">Versions précédentes</span><span class="sxs-lookup"><span data-stu-id="529cc-119">Past Releases</span></span>

<span data-ttu-id="529cc-120">La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.</span><span class="sxs-lookup"><span data-stu-id="529cc-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
