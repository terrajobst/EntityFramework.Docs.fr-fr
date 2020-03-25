---
title: Nouveautés – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136135"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="90af4-102">Nouveautés dans EF6</span><span class="sxs-lookup"><span data-stu-id="90af4-102">What's new in EF6</span></span>

<span data-ttu-id="90af4-103">Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.</span><span class="sxs-lookup"><span data-stu-id="90af4-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="90af4-104">Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.</span><span class="sxs-lookup"><span data-stu-id="90af4-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="90af4-105">Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="90af4-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="90af4-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="90af4-106">EF 6.4.0</span></span>

<span data-ttu-id="90af4-107">Le runtime EF 6.4.0 a été publié sur NuGet en décembre 2019.</span><span class="sxs-lookup"><span data-stu-id="90af4-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="90af4-108">L’objectif principal d’EF 6.4 est de peaufiner les fonctionnalités et les scénarios fournis par EF 6.3.</span><span class="sxs-lookup"><span data-stu-id="90af4-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="90af4-109">Consultez la [liste des correctifs importants](https://github.com/dotnet/ef6/milestone/14?closed=1) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="90af4-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="90af4-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="90af4-110">EF 6.3.0</span></span>

<span data-ttu-id="90af4-111">Le runtime EF 6.3.0 a été publié sur NuGet en septembre 2019.</span><span class="sxs-lookup"><span data-stu-id="90af4-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="90af4-112">L’objectif principal de cette version était de faciliter la migration des applications existantes qui utilisent EF 6 vers .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="90af4-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="90af4-113">La communauté a également contribué à plusieurs correctifs de bogues et améliorations.</span><span class="sxs-lookup"><span data-stu-id="90af4-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="90af4-114">Pour plus d’informations, consultez les problèmes fermés à chaque [jalon](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la version 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="90af4-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="90af4-115">Voici les plus connus :</span><span class="sxs-lookup"><span data-stu-id="90af4-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="90af4-116">Prise en charge de .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="90af4-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="90af4-117">Le package EntityFramework cible maintenant .NET Standard 2.1 en plus de .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="90af4-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="90af4-118">Cela signifie que EF 6.3 est multiplateforme et pris en charge sur d’autres systèmes d’exploitation que Windows, comme Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="90af4-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="90af4-119">Les commandes de migrations ont été réécrites pour s’exécuter hors processus et fonctionner avec des projets de type SDK.</span><span class="sxs-lookup"><span data-stu-id="90af4-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="90af4-120">Prise en charge de SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="90af4-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="90af4-121">Compatibilité améliorée avec Roslyn et NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="90af4-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="90af4-122">Ajout de l’utilitaire `ef6.exe` pour l’activation, l’ajout, l’écriture de scripts et l’application de migrations à partir d’assemblys.</span><span class="sxs-lookup"><span data-stu-id="90af4-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="90af4-123">Ceci remplace `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="90af4-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="90af4-124">Prise en charge du concepteur EF</span><span class="sxs-lookup"><span data-stu-id="90af4-124">EF designer support</span></span>

<span data-ttu-id="90af4-125">L’utilisation du concepteur EF directement dans les projets .NET Core ou .NET Standard, ou dans les projets .NET Framework de style SDK, n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="90af4-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="90af4-126">Vous pouvez contourner cette limitation en ajoutant le fichier EDMX et les classes générées pour les entités et pour DbContext en tant que fichiers liés à un projet .NET Core 3.0 ou .NET Standard 2.1 dans la même solution.</span><span class="sxs-lookup"><span data-stu-id="90af4-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="90af4-127">Les fichiers liés ressembleront à ce qui suit dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="90af4-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="90af4-128">Notez que le fichier EDMX est lié à l’action de création EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="90af4-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="90af4-129">Il s’agit d’une tâche MSBuild spéciale (désormais incluse dans le package EF 6.3) qui prend en charge l’ajout du modèle EF à l’assembly cible en tant que ressources incorporées (ou copiez-le en tant que fichiers dans le dossier de sortie, en fonction du paramètre de traitement des artefacts de métadonnées dans EDMX).</span><span class="sxs-lookup"><span data-stu-id="90af4-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="90af4-130">Pour plus d’informations sur la façon d’obtenir cette configuration, consultez notre [exemple .NET Core EDMX](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="90af4-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="90af4-131">Avertissement : vérifiez que le projet .NET Framework utilisant l’ancien style (autre que le style SDK) qui définit le « vrai » fichier .edmx est situé _avant_ le projet qui définit le lien situé dans le fichier .sln.</span><span class="sxs-lookup"><span data-stu-id="90af4-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="90af4-132">Sinon, lorsque vous ouvrirez le fichier .edmx dans le concepteur, vous verrez le message d’erreur « Entity Framework n’est pas disponible dans la version cible de .NET Framework actuellement spécifiée pour le projet.</span><span class="sxs-lookup"><span data-stu-id="90af4-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="90af4-133">Vous pouvez changer la version cible de .NET Framework du projet ou modifier le modèle dans XmlEditor ».</span><span class="sxs-lookup"><span data-stu-id="90af4-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="90af4-134">Versions précédentes</span><span class="sxs-lookup"><span data-stu-id="90af4-134">Past Releases</span></span>

<span data-ttu-id="90af4-135">La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.</span><span class="sxs-lookup"><span data-stu-id="90af4-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
