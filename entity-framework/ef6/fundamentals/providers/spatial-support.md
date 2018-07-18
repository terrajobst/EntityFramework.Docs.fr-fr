---
title: Prise en charge de fournisseur pour les Types spatiaux - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
caps.latest.revision: 3
ms.openlocfilehash: 76020e2a3127b1026a5cb8f032686cc8ce9c0c5f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120828"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="75f12-102">Prise en charge de fournisseur pour les Types spatiaux</span><span class="sxs-lookup"><span data-stu-id="75f12-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="75f12-103">Entity Framework prend en charge l’utilisation des données spatiales via les classes de DbGeography ou DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="75f12-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="75f12-104">Ces classes s’appuient sur les fonctionnalités spécifiques à la base de données offertes par le fournisseur Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="75f12-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="75f12-105">Pas tous les fournisseurs prennent en charge les données spatiales et les autres peuvent avoir des conditions préalables supplémentaires telles que l’installation des assemblys de type spatial.</span><span class="sxs-lookup"><span data-stu-id="75f12-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="75f12-106">Vous trouverez ci-dessous plus d’informations sur la prise en charge de fournisseur pour les types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="75f12-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="75f12-107">Vous trouverez des informations supplémentaires sur l’utilisation des types spatiaux dans une application dans deux procédures, une pour Code First, l’autre pour Database First ou Model First :</span><span class="sxs-lookup"><span data-stu-id="75f12-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="75f12-108">Types de données spatiales dans le Code tout d’abord</span><span class="sxs-lookup"><span data-stu-id="75f12-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="75f12-109">Types de données spatiales dans Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="75f12-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="75f12-110">Versions d’Entity Framework qui prennent en charge les types de données spatiales</span><span class="sxs-lookup"><span data-stu-id="75f12-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="75f12-111">Prise en charge pour les types spatiaux a été introduite dans EF5.</span><span class="sxs-lookup"><span data-stu-id="75f12-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="75f12-112">Toutefois, dans EF5 types spatiaux sont uniquement pris en charge quand l’application cible et s’exécute sur .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="75f12-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="75f12-113">À compter de EF6 types spatiaux sont pris en charge pour les applications ciblant le .NET 4 et .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="75f12-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="75f12-114">Fournisseurs EF qui prennent en charge les types de données spatiales</span><span class="sxs-lookup"><span data-stu-id="75f12-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="75f12-115">EF5</span><span class="sxs-lookup"><span data-stu-id="75f12-115">EF5</span></span>  

<span data-ttu-id="75f12-116">Les fournisseurs d’Entity Framework pour EF5 qui nous prennent en charge que les types spatiaux prise en charge sont :</span><span class="sxs-lookup"><span data-stu-id="75f12-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="75f12-117">Fournisseur Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="75f12-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="75f12-118">Ce fournisseur est partie intégrante d’EF5.</span><span class="sxs-lookup"><span data-stu-id="75f12-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="75f12-119">Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires devant être installé, voir ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="75f12-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="75f12-120">Devart dotconnect relative pour Oracle</span><span class="sxs-lookup"><span data-stu-id="75f12-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="75f12-121">Il s’agit d’un fournisseur tiers à partir de Devart.</span><span class="sxs-lookup"><span data-stu-id="75f12-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="75f12-122">Si vous connaissez d’un fournisseur d’EF5 prenant en charge les types spatiaux puis Veuillez contacter et nous serons heureux de vous ajouter à cette liste.</span><span class="sxs-lookup"><span data-stu-id="75f12-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="75f12-123">EF6</span><span class="sxs-lookup"><span data-stu-id="75f12-123">EF6</span></span>  

<span data-ttu-id="75f12-124">Les fournisseurs d’Entity Framework pour EF6 qui nous prennent en charge que les types spatiaux prise en charge sont :</span><span class="sxs-lookup"><span data-stu-id="75f12-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="75f12-125">Fournisseur Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="75f12-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="75f12-126">Ce fournisseur est partie intégrante d’EF6.</span><span class="sxs-lookup"><span data-stu-id="75f12-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="75f12-127">Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires devant être installé, voir ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="75f12-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="75f12-128">Devart dotconnect relative pour Oracle</span><span class="sxs-lookup"><span data-stu-id="75f12-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="75f12-129">Il s’agit d’un fournisseur tiers à partir de Devart.</span><span class="sxs-lookup"><span data-stu-id="75f12-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="75f12-130">Si vous connaissez d’un fournisseur d’EF6 prenant en charge les types spatiaux puis Veuillez contacter et nous serons heureux de vous ajouter à cette liste.</span><span class="sxs-lookup"><span data-stu-id="75f12-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="75f12-131">Conditions préalables pour les types spatiaux avec Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="75f12-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="75f12-132">Prise en charge spatiale SQL Server dépend des types spécifiques de SQL Server bas niveau, SqlGeography et SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="75f12-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="75f12-133">Ces types se trouvent dans l’assembly de Microsoft.SqlServer.Types.dll, et cet assembly n’est pas fourni dans le cadre d’EF ou en tant que partie du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="75f12-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="75f12-134">Lorsque Visual Studio est installé il installe souvent également une version de SQL Server, et cela inclut l’installation du fichier Microsoft.SqlServer.Types.dll.</span><span class="sxs-lookup"><span data-stu-id="75f12-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="75f12-135">Si SQL Server n’est pas installé sur l’ordinateur où vous souhaitez utiliser des types de données spatiales, ou si les types spatiaux ont été exclus de l’installation de SQL Server, vous devrez les installer manuellement.</span><span class="sxs-lookup"><span data-stu-id="75f12-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="75f12-136">Les types peuvent être installés à l’aide de `SQLSysClrTypes.msi`, qui fait partie du Feature Pack Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75f12-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="75f12-137">Types de données spatiales sont SQL Server spécifique à la version, nous vous recommandons de [recherche pour « Feature Pack SQL Server »](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) dans le Microsoft Download Center, puis sélectionnez et téléchargez l’option qui correspond à la version de SQL Server, vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="75f12-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
