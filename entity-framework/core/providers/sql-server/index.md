---
title: Fournisseur de base de données Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995667"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="3a260-102">Fournisseur de base de données EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="3a260-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="3a260-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="3a260-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="3a260-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="3a260-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="3a260-105">Installez</span><span class="sxs-lookup"><span data-stu-id="3a260-105">Install</span></span>

<span data-ttu-id="3a260-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="3a260-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="3a260-107">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="3a260-107">Get Started</span></span>

<span data-ttu-id="3a260-108">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="3a260-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="3a260-109">Bien démarrer sur .NET Framework (Console, WinForms, WPF, etc.)</span><span class="sxs-lookup"><span data-stu-id="3a260-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="3a260-110">Bien démarrer sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a260-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="3a260-111">Exemple d’application UnicornStore</span><span class="sxs-lookup"><span data-stu-id="3a260-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="3a260-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="3a260-112">Supported Database Engines</span></span>

* <span data-ttu-id="3a260-113">Microsoft SQL Server (versions 2008 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="3a260-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3a260-114">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="3a260-114">Supported Platforms</span></span>

* <span data-ttu-id="3a260-115">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="3a260-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="3a260-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a260-116">.NET Core</span></span>

* <span data-ttu-id="3a260-117">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="3a260-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
