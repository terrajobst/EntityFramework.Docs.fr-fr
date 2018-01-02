---
title: "Fournisseur de base de données Microsoft SQL Server - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="6955e-102">Fournisseur de base de données EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="6955e-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="6955e-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="6955e-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="6955e-104">Le fournisseur est géré dans le cadre du [projet GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="6955e-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="6955e-105">Installer</span><span class="sxs-lookup"><span data-stu-id="6955e-105">Install</span></span>

<span data-ttu-id="6955e-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="6955e-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="6955e-107">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="6955e-107">Get Started</span></span>

<span data-ttu-id="6955e-108">Les ressources suivantes vous permettront de démarrer avec ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="6955e-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="6955e-109">Bien démarrer sur .NET Framework (Console, WinForms, WPF, etc.)</span><span class="sxs-lookup"><span data-stu-id="6955e-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="6955e-110">Bien démarrer sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6955e-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="6955e-111">Exemple d’application UnicornStore</span><span class="sxs-lookup"><span data-stu-id="6955e-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="6955e-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="6955e-112">Supported Database Engines</span></span>

* <span data-ttu-id="6955e-113">Microsoft SQL Server (versions 2008 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="6955e-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6955e-114">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="6955e-114">Supported Platforms</span></span>

* <span data-ttu-id="6955e-115">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="6955e-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="6955e-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6955e-116">.NET Core</span></span>

* <span data-ttu-id="6955e-117">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="6955e-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
