---
title: Prise en charge des fournisseurs pour les types spatiaux-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181598"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="94e5c-102">Prise en charge des fournisseurs pour les types spatiaux</span><span class="sxs-lookup"><span data-stu-id="94e5c-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="94e5c-103">Entity Framework prend en charge l’utilisation des données spatiales via les classes DbGeography ou DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="94e5c-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="94e5c-104">Ces classes reposent sur les fonctionnalités spécifiques à la base de données offertes par le fournisseur Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="94e5c-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="94e5c-105">Tous les fournisseurs ne prennent pas en charge les données spatiales et ceux qui peuvent avoir des conditions préalables supplémentaires telles que l’installation d’assemblys de type spatial.</span><span class="sxs-lookup"><span data-stu-id="94e5c-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="94e5c-106">Vous trouverez ci-dessous des informations supplémentaires sur la prise en charge des fournisseurs pour les types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="94e5c-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="94e5c-107">Vous trouverez des informations supplémentaires sur l’utilisation des types spatiaux dans une application dans deux procédures pas à pas, une pour Code First, l’autre pour Database First ou Model First :</span><span class="sxs-lookup"><span data-stu-id="94e5c-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="94e5c-108">Types de données spatiales dans Code First</span><span class="sxs-lookup"><span data-stu-id="94e5c-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="94e5c-109">Types de données spatiales dans le concepteur EF</span><span class="sxs-lookup"><span data-stu-id="94e5c-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="94e5c-110">Versions d’EF qui prennent en charge les types spatiaux</span><span class="sxs-lookup"><span data-stu-id="94e5c-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="94e5c-111">La prise en charge des types spatiaux a été introduite dans EF5.</span><span class="sxs-lookup"><span data-stu-id="94e5c-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="94e5c-112">Toutefois, dans les types spatiaux EF5 sont uniquement pris en charge lorsque l’application cible et s’exécute sur .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="94e5c-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="94e5c-113">Le démarrage des types spatiaux EF6 est pris en charge pour les applications ciblant à la fois .NET 4 et .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="94e5c-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="94e5c-114">Fournisseurs EF qui prennent en charge les types spatiaux</span><span class="sxs-lookup"><span data-stu-id="94e5c-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="94e5c-115">EF5</span><span class="sxs-lookup"><span data-stu-id="94e5c-115">EF5</span></span>  

<span data-ttu-id="94e5c-116">Les fournisseurs de Entity Framework pour EF5 que nous avons conscients de la prise en charge des types spatiaux sont :</span><span class="sxs-lookup"><span data-stu-id="94e5c-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="94e5c-117">Fournisseur Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="94e5c-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="94e5c-118">Ce fournisseur est fourni dans le cadre de EF5.</span><span class="sxs-lookup"><span data-stu-id="94e5c-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="94e5c-119">Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires qui devront peut-être être installées. pour plus d’informations, voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="94e5c-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="94e5c-120">Devart dotConnect pour Oracle</span><span class="sxs-lookup"><span data-stu-id="94e5c-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="94e5c-121">Il s’agit d’un fournisseur tiers de Devart.</span><span class="sxs-lookup"><span data-stu-id="94e5c-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="94e5c-122">Si vous connaissez un fournisseur EF5 qui prend en charge les types spatiaux, contactez-nous et nous serons heureux de l’ajouter à cette liste.</span><span class="sxs-lookup"><span data-stu-id="94e5c-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="94e5c-123">EF6</span><span class="sxs-lookup"><span data-stu-id="94e5c-123">EF6</span></span>  

<span data-ttu-id="94e5c-124">Les fournisseurs de Entity Framework pour EF6 que nous avons conscients de la prise en charge des types spatiaux sont :</span><span class="sxs-lookup"><span data-stu-id="94e5c-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="94e5c-125">Fournisseur Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="94e5c-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="94e5c-126">Ce fournisseur est fourni dans le cadre de EF6.</span><span class="sxs-lookup"><span data-stu-id="94e5c-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="94e5c-127">Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires qui devront peut-être être installées. pour plus d’informations, voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="94e5c-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="94e5c-128">Devart dotConnect pour Oracle</span><span class="sxs-lookup"><span data-stu-id="94e5c-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="94e5c-129">Il s’agit d’un fournisseur tiers de Devart.</span><span class="sxs-lookup"><span data-stu-id="94e5c-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="94e5c-130">Si vous connaissez un fournisseur EF6 qui prend en charge les types spatiaux, contactez-nous et nous serons heureux de l’ajouter à cette liste.</span><span class="sxs-lookup"><span data-stu-id="94e5c-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="94e5c-131">Conditions préalables pour les types spatiaux avec Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="94e5c-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="94e5c-132">SQL Server la prise en charge spatiale dépend des types de SQL Server de bas niveau, SqlGeography et SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="94e5c-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="94e5c-133">Ces types se trouvent dans l’assembly Microsoft. SqlServer. types. dll, et cet assembly n’est pas fourni avec EF ou dans le cadre de l' .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="94e5c-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="94e5c-134">Quand Visual Studio est installé, il installe également une version de SQL Server, ce qui inclut l’installation de Microsoft. SqlServer. types. dll.</span><span class="sxs-lookup"><span data-stu-id="94e5c-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="94e5c-135">Si SQL Server n’est pas installé sur l’ordinateur où vous souhaitez utiliser les types spatiaux, ou si les types spatiaux ont été exclus de l’installation SQL Server, vous devez les installer manuellement.</span><span class="sxs-lookup"><span data-stu-id="94e5c-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="94e5c-136">Les types peuvent être installés à l’aide de `SQLSysClrTypes.msi`, qui fait partie de Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="94e5c-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="94e5c-137">Les types spatiaux étant spécifiques à la version SQL Server, nous vous recommandons de [Rechercher « SQL Server Feature Pack »](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) dans le centre de téléchargement Microsoft, puis de sélectionner et de télécharger l’option correspondant à la version de SQL Server que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="94e5c-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
