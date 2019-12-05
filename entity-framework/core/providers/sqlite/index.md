---
title: Fournisseur de base de données SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 433dedbe1e0e11bf2fd83e259e321ece32b2c188
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824797"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="ee6f7-102">Fournisseur de base de données EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="ee6f7-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="ee6f7-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQLite.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="ee6f7-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ee6f7-105">Installez</span><span class="sxs-lookup"><span data-stu-id="ee6f7-105">Install</span></span>

<span data-ttu-id="ee6f7-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="ee6f7-107">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee6f7-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="ee6f7-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee6f7-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="ee6f7-109">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="ee6f7-109">Supported Database Engines</span></span>

* <span data-ttu-id="ee6f7-110">SQLite (versions 3.7 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="ee6f7-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="ee6f7-111">Limitations</span><span class="sxs-lookup"><span data-stu-id="ee6f7-111">Limitations</span></span>

<span data-ttu-id="ee6f7-112">Consultez [Limitations de SQLite](limitations.md) pour connaître certaines limitations importantes du fournisseur SQLite.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
