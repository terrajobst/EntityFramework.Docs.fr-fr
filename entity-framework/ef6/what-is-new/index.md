---
title: Nouveautés – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266781"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="926d7-102">Nouveautés dans EF6</span><span class="sxs-lookup"><span data-stu-id="926d7-102">What's new in EF6</span></span>

<span data-ttu-id="926d7-103">Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.</span><span class="sxs-lookup"><span data-stu-id="926d7-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="926d7-104">Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.</span><span class="sxs-lookup"><span data-stu-id="926d7-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="926d7-105">Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="926d7-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="926d7-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="926d7-106">EF 6.3.0</span></span>

<span data-ttu-id="926d7-107">Le runtime EF 6.3.0 a été publié sur NuGet en septembre 2019.</span><span class="sxs-lookup"><span data-stu-id="926d7-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="926d7-108">L’objectif principal de cette version était de faciliter la migration des applications existantes qui utilisent EF 6 vers .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="926d7-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="926d7-109">La communauté a également contribué à plusieurs correctifs de bogues et améliorations.</span><span class="sxs-lookup"><span data-stu-id="926d7-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="926d7-110">Pour plus d’informations, consultez les problèmes fermés à chaque [jalon](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la version 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="926d7-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="926d7-111">Voici les plus connus :</span><span class="sxs-lookup"><span data-stu-id="926d7-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="926d7-112">Prise en charge de .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="926d7-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="926d7-113">Le package EntityFramework cible maintenant .NET Standard 2.1 en plus de .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="926d7-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="926d7-114">Cela signifie que EF 6.3 est multiplateforme et pris en charge sur d’autres systèmes d’exploitation que Windows, comme Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="926d7-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="926d7-115">Les commandes de migrations ont été réécrites pour s’exécuter hors processus et fonctionner avec des projets de type SDK.</span><span class="sxs-lookup"><span data-stu-id="926d7-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="926d7-116">Prise en charge de SQL Server HierarchyId</span><span class="sxs-lookup"><span data-stu-id="926d7-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="926d7-117">Compatibilité améliorée avec Roslyn et NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="926d7-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="926d7-118">Ajout de l’utilitaire `ef6.exe` pour l’activation, l’ajout, l’écriture de scripts et l’application de migrations à partir d’assemblys.</span><span class="sxs-lookup"><span data-stu-id="926d7-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="926d7-119">Ceci remplace `migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="926d7-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="926d7-120">Prise en charge du concepteur EF</span><span class="sxs-lookup"><span data-stu-id="926d7-120">EF designer support</span></span>

<span data-ttu-id="926d7-121">Il n’existe actuellement aucune prise en charge pour l’utilisation du concepteur EF directement sur les projets .NET Core ou .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="926d7-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="926d7-122">Vous pouvez contourner cette limitation en ajoutant le fichier EDMX et les classes générées pour les entités et pour DbContext en tant que fichiers liés à un projet .NET Core 3.0 ou .NET Standard 2.1 dans la même solution.</span><span class="sxs-lookup"><span data-stu-id="926d7-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="926d7-123">Les fichiers liés ressembleront à ce qui suit dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="926d7-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="926d7-124">Notez que le fichier EDMX est lié à l’action de création EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="926d7-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="926d7-125">Il s’agit d’une tâche MSBuild spéciale (désormais incluse dans le package EF 6.3) qui prend en charge l’ajout du modèle EF à l’assembly cible en tant que ressources incorporées (ou copiez-le en tant que fichiers dans le dossier de sortie, en fonction du paramètre de traitement des artefacts de métadonnées dans EDMX).</span><span class="sxs-lookup"><span data-stu-id="926d7-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="926d7-126">Pour plus d’informations sur la façon d’obtenir cette configuration, consultez notre [exemple .NET Core EDMX](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="926d7-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="926d7-127">Versions précédentes</span><span class="sxs-lookup"><span data-stu-id="926d7-127">Past Releases</span></span>

<span data-ttu-id="926d7-128">La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.</span><span class="sxs-lookup"><span data-stu-id="926d7-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
