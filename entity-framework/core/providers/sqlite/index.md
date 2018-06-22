---
title: Fournisseur de base de données SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678746"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="96fd3-102">Fournisseur de base de données EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="96fd3-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="96fd3-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQLite.</span><span class="sxs-lookup"><span data-stu-id="96fd3-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="96fd3-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="96fd3-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="96fd3-105">Installer</span><span class="sxs-lookup"><span data-stu-id="96fd3-105">Install</span></span>

<span data-ttu-id="96fd3-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="96fd3-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="96fd3-107">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="96fd3-107">Get Started</span></span>

<span data-ttu-id="96fd3-108">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="96fd3-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="96fd3-109">Base de données SQLite locale sur UWP</span><span class="sxs-lookup"><span data-stu-id="96fd3-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="96fd3-110">Application .NET Core avec une nouvelle base de données SQLite</span><span class="sxs-lookup"><span data-stu-id="96fd3-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="96fd3-111">Exemple d’application Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="96fd3-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="96fd3-112">Exemple d’application Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="96fd3-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="96fd3-113">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="96fd3-113">Supported Database Engines</span></span>

* <span data-ttu-id="96fd3-114">SQLite (versions 3.7 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="96fd3-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="96fd3-115">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="96fd3-115">Supported Platforms</span></span>

* <span data-ttu-id="96fd3-116">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="96fd3-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="96fd3-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="96fd3-117">.NET Core</span></span>

* <span data-ttu-id="96fd3-118">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="96fd3-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="96fd3-119">Plateforme Windows universelle</span><span class="sxs-lookup"><span data-stu-id="96fd3-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="96fd3-120">Limitations</span><span class="sxs-lookup"><span data-stu-id="96fd3-120">Limitations</span></span>

<span data-ttu-id="96fd3-121">Consultez [Limitations de SQLite](limitations.md) pour connaître certaines limitations importantes du fournisseur SQLite.</span><span class="sxs-lookup"><span data-stu-id="96fd3-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
