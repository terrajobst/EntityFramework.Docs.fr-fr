---
title: Fournisseur de base de données SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149266"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="09c66-102">Fournisseur de base de données EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="09c66-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="09c66-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQLite.</span><span class="sxs-lookup"><span data-stu-id="09c66-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="09c66-104">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="09c66-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="09c66-105">Installez</span><span class="sxs-lookup"><span data-stu-id="09c66-105">Install</span></span>

<span data-ttu-id="09c66-106">Installez le [package NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="09c66-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="09c66-107">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="09c66-107">Supported Database Engines</span></span>

* <span data-ttu-id="09c66-108">SQLite (versions 3.7 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="09c66-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="09c66-109">Limitations</span><span class="sxs-lookup"><span data-stu-id="09c66-109">Limitations</span></span>

<span data-ttu-id="09c66-110">Consultez [Limitations de SQLite](limitations.md) pour connaître certaines limitations importantes du fournisseur SQLite.</span><span class="sxs-lookup"><span data-stu-id="09c66-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
