---
title: "Fournisseur de base de données Microsoft SQL Server Compact Edition - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="c04d4-102">Fournisseur de base de données EF Core Microsoft SQL Server Compact Edition</span><span class="sxs-lookup"><span data-stu-id="c04d4-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="c04d4-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="c04d4-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="c04d4-104">Le fournisseur est géré dans le cadre du [projet GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="c04d4-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="c04d4-105">Il n’est pas géré dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c04d4-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c04d4-106">Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="c04d4-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c04d4-107">Installez</span><span class="sxs-lookup"><span data-stu-id="c04d4-107">Install</span></span>

<span data-ttu-id="c04d4-108">Pour utiliser SQL Server Compact Edition 4.0, installez le [package NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="c04d4-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="c04d4-109">Pour utiliser SQL Server Compact Edition 3.5, installez le [package NuGet EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="c04d4-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="c04d4-110">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="c04d4-110">Get Started</span></span>

<span data-ttu-id="c04d4-111">Consultez la [documentation de prise en main sur le site du projet](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).</span><span class="sxs-lookup"><span data-stu-id="c04d4-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c04d4-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="c04d4-112">Supported Database Engines</span></span>

* <span data-ttu-id="c04d4-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="c04d4-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="c04d4-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="c04d4-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c04d4-115">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="c04d4-115">Supported Platforms</span></span>

* <span data-ttu-id="c04d4-116">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="c04d4-116">.NET Framework (4.5.1 onwards)</span></span>
