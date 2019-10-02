---
title: Fournisseur de base de données Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813502"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="b1ac1-102">Fournisseur de base de données EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b1ac1-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="b1ac1-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="b1ac1-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="b1ac1-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b1ac1-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b1ac1-105">Installez</span><span class="sxs-lookup"><span data-stu-id="b1ac1-105">Install</span></span>

<span data-ttu-id="b1ac1-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="b1ac1-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="b1ac1-107">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="b1ac1-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="b1ac1-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1ac1-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="b1ac1-109">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="b1ac1-109">Supported Database Engines</span></span>

* <span data-ttu-id="b1ac1-110">Microsoft SQL Server (versions 2012 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="b1ac1-110">Microsoft SQL Server (2012 onwards)</span></span>
