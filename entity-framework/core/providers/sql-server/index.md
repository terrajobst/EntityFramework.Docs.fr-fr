---
title: Fournisseur de base de données Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149206"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="ae5f0-102">Fournisseur de base de données EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="ae5f0-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="ae5f0-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="ae5f0-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="ae5f0-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="ae5f0-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ae5f0-105">Installez</span><span class="sxs-lookup"><span data-stu-id="ae5f0-105">Install</span></span>

<span data-ttu-id="ae5f0-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="ae5f0-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="ae5f0-107">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="ae5f0-107">Supported Database Engines</span></span>

* <span data-ttu-id="ae5f0-108">Microsoft SQL Server (versions 2012 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="ae5f0-108">Microsoft SQL Server (2012 onwards)</span></span>
