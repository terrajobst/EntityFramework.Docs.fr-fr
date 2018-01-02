---
title: "Fournisseur de base de données Npgsql pour PostgreSQL - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="568d7-102">Fournisseur de base de données EF Core Npgsql pour PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="568d7-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="568d7-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="568d7-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="568d7-104">Le fournisseur est géré dans le cadre du [projet Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="568d7-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="568d7-105">Il n’est pas géré dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="568d7-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="568d7-106">Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="568d7-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="568d7-107">Installer</span><span class="sxs-lookup"><span data-stu-id="568d7-107">Install</span></span>

<span data-ttu-id="568d7-108">Installez le [package NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="568d7-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="568d7-109">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="568d7-109">Get Started</span></span>

<span data-ttu-id="568d7-110">Pour commencer, consultez la [documentation de Npgsql](http://www.npgsql.org/efcore/index.html).</span><span class="sxs-lookup"><span data-stu-id="568d7-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="568d7-111">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="568d7-111">Supported Database Engines</span></span>

* <span data-ttu-id="568d7-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="568d7-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="568d7-113">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="568d7-113">Supported Platforms</span></span>

* <span data-ttu-id="568d7-114">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="568d7-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="568d7-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="568d7-115">.NET Core</span></span>

* <span data-ttu-id="568d7-116">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="568d7-116">Mono (4.2.0 onwards)</span></span>
