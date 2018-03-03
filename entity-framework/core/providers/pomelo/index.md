---
title: "Fournisseur de base de données Pomelo MySQL - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="4d63e-102">Fournisseur de base de données EF Core Pomelo pour MySQL</span><span class="sxs-lookup"><span data-stu-id="4d63e-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="4d63e-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec MySQL.</span><span class="sxs-lookup"><span data-stu-id="4d63e-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="4d63e-104">Le fournisseur est géré dans le cadre du [projet Pomelo Foundation](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="4d63e-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="4d63e-105">Il n’est pas géré dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4d63e-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="4d63e-106">Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="4d63e-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="4d63e-107">Installez</span><span class="sxs-lookup"><span data-stu-id="4d63e-107">Install</span></span>

<span data-ttu-id="4d63e-108">Installez le [package NuGet Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="4d63e-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="4d63e-109">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="4d63e-109">Get Started</span></span>

<span data-ttu-id="4d63e-110">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="4d63e-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="4d63e-111">Documentation de prise en main</span><span class="sxs-lookup"><span data-stu-id="4d63e-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="4d63e-112">Exemple d’application de blog Yuuko</span><span class="sxs-lookup"><span data-stu-id="4d63e-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="4d63e-113">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="4d63e-113">Supported Database Engines</span></span>

* <span data-ttu-id="4d63e-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="4d63e-114">MySQL</span></span>
* <span data-ttu-id="4d63e-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="4d63e-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4d63e-116">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="4d63e-116">Supported Platforms</span></span>

* <span data-ttu-id="4d63e-117">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="4d63e-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="4d63e-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d63e-118">.NET Core</span></span>

* <span data-ttu-id="4d63e-119">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="4d63e-119">Mono (4.2.0 onwards)</span></span>
