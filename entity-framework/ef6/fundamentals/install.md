---
title: Obtenir Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 7f840a4f9e437ec12f699184339e386976e1528b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490637"
---
# <a name="get-entity-framework"></a><span data-ttu-id="68181-102">Obtenir Entity Framework</span><span class="sxs-lookup"><span data-stu-id="68181-102">Get Entity Framework</span></span>
<span data-ttu-id="68181-103">Entity Framework est constitué par les outils Entity Framework pour Visual Studio et le Runtime Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="68181-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="68181-104">Outils Entity Framework pour Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68181-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="68181-105">Entity Framework Tools pour Visual Studio incluent le Concepteur EF et l’Assistant de modèle EF et sont nécessaires pour la base de données tout d’abord et premier flux de travail de modèle.</span><span class="sxs-lookup"><span data-stu-id="68181-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="68181-106">Outils Entity Framework sont inclus dans toutes les versions récentes de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68181-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="68181-107">Si vous effectuez une installation personnalisée de Visual Studio, vous devez vous assurer que l’élément « Entity Framework 6 Tools » est sélectionné par en choisissant une charge de travail qui l’inclut ou en le sélectionnant de manière individuelle.</span><span class="sxs-lookup"><span data-stu-id="68181-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="68181-108">Pour d’anciennes versions de Visual Studio, les outils EF mis à jour sont disponibles en téléchargement.</span><span class="sxs-lookup"><span data-stu-id="68181-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="68181-109">Consultez [Versions de Visual Studio](~/ef6/what-is-new/visual-studio.md) pour obtenir des conseils sur la façon d’obtenir la dernière version des outils Entity Framework disponibles pour votre version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68181-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="68181-110">Runtime EF</span><span class="sxs-lookup"><span data-stu-id="68181-110">EF Runtime</span></span>

<span data-ttu-id="68181-111">La dernière version d’Entity Framework est disponible en tant que le [package EntityFramework NuGet](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="68181-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="68181-112">Si vous n’êtes pas familiarisé avec le Gestionnaire de Package NuGet, nous vous encourageons à lire le [vue d’ensemble de NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="68181-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="68181-113">Installation du Package de NuGet EF</span><span class="sxs-lookup"><span data-stu-id="68181-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="68181-114">Vous pouvez installer le package EntityFramework en cliquant sur le **références** dossier de votre projet et en sélectionnant **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="68181-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Gérer les Packages NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="68181-116">Installation à partir de la Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="68181-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="68181-117">Vous pouvez également installer EntityFramework en exécutant la commande suivante dans le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="68181-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="68181-118">Installation d’une version spécifique d’EF</span><span class="sxs-lookup"><span data-stu-id="68181-118">Installing a specific version of EF</span></span>

<span data-ttu-id="68181-119">À partir d’Entity Framework 4.1 et versions ultérieures, les nouvelles versions du runtime EF ont été publiées en tant que le [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="68181-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="68181-120">Une de ces versions peuvent être ajoutée à un projet basé sur le .NET Framework en exécutant la commande suivante dans Visual Studio [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="68181-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="68181-121">Notez que `<number>` représente la version spécifique d’EF à installer.</span><span class="sxs-lookup"><span data-stu-id="68181-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="68181-122">Par exemple, 6.2.0 est la version du nombre de EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="68181-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="68181-123">Runtimes d’EF avant 4.1 faisaient partie du .NET Framework et ne peut pas être installé séparément.</span><span class="sxs-lookup"><span data-stu-id="68181-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="68181-124">L’installation de la dernière version préliminaire</span><span class="sxs-lookup"><span data-stu-id="68181-124">Installing the Latest Preview</span></span>

<span data-ttu-id="68181-125">Les méthodes ci-dessus vous donnera la dernière version entièrement prise en charge de la version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="68181-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="68181-126">Il existe souvent des versions préliminaires d’Entity Framework disponibles que nous aimerions vous permettent de tester et envoyez-nous vos commentaires sur.</span><span class="sxs-lookup"><span data-stu-id="68181-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="68181-127">Pour installer la dernière version préliminaire d’Entity Framework vous pouvez sélectionner **inclure la version préliminaire** dans la fenêtre Gérer les Packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="68181-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="68181-128">Si aucune version préliminaire n’est disponibles vous obtenez automatiquement la dernière version entièrement prise en charge d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="68181-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Inclure la version préliminaire](~/ef6/media/includeprerelease.png)

<span data-ttu-id="68181-130">Ou bien, vous pouvez exécuter la commande suivante le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="68181-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
