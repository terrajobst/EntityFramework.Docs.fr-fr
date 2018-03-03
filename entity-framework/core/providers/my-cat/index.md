---
title: "Fournisseur de base de données Pomelo MyCat - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="358e8-102">Fournisseur de base de données EF Core Pomelo MyCat</span><span class="sxs-lookup"><span data-stu-id="358e8-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="358e8-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec [MyCat](https://github.com/MyCATApache/Mycat-Server).</span><span class="sxs-lookup"><span data-stu-id="358e8-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="358e8-104">Le fournisseur est géré dans le cadre du [projet Pomelo Foundation](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span><span class="sxs-lookup"><span data-stu-id="358e8-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="358e8-105">Il n’est pas géré dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="358e8-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="358e8-106">Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="358e8-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="358e8-107">Installez</span><span class="sxs-lookup"><span data-stu-id="358e8-107">Install</span></span>

<span data-ttu-id="358e8-108">Téléchargez et exécutez [la dernière version à partir du site du projet](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span><span class="sxs-lookup"><span data-stu-id="358e8-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="358e8-109">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="358e8-109">Get Started</span></span>

<span data-ttu-id="358e8-110">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="358e8-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="358e8-111">Étapes de la configuration</span><span class="sxs-lookup"><span data-stu-id="358e8-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="358e8-112">Vidéo de prise en main</span><span class="sxs-lookup"><span data-stu-id="358e8-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="358e8-113">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="358e8-113">Supported Database Engines</span></span>

* <span data-ttu-id="358e8-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="358e8-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="358e8-115">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="358e8-115">Supported Platforms</span></span>

* <span data-ttu-id="358e8-116">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="358e8-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="358e8-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="358e8-117">.NET Core</span></span>
* <span data-ttu-id="358e8-118">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="358e8-118">Mono (4.2.0 onwards)</span></span>
