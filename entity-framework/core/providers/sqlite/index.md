---
title: Fournisseur de base de données SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 4637044b1712280d3cdca6bcca1ca61564ff78ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656073"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="143e6-102">Fournisseur de base de données EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="143e6-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="143e6-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQLite.</span><span class="sxs-lookup"><span data-stu-id="143e6-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="143e6-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="143e6-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="143e6-105">Installez</span><span class="sxs-lookup"><span data-stu-id="143e6-105">Install</span></span>

<span data-ttu-id="143e6-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="143e6-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="143e6-107">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="143e6-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="143e6-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="143e6-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="143e6-109">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="143e6-109">Supported Database Engines</span></span>

* <span data-ttu-id="143e6-110">SQLite (versions 3.7 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="143e6-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="143e6-111">Limitations</span><span class="sxs-lookup"><span data-stu-id="143e6-111">Limitations</span></span>

<span data-ttu-id="143e6-112">Consultez [Limitations de SQLite](limitations.md) pour connaître certaines limitations importantes du fournisseur SQLite.</span><span class="sxs-lookup"><span data-stu-id="143e6-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
