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
# <a name="sqlite-ef-core-database-provider"></a>Fournisseur de base de données EF Core SQLite

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQLite. Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* SQLite (versions 3.7 et suivantes)

## <a name="limitations"></a>Limitations

Consultez [Limitations de SQLite](limitations.md) pour connaître certaines limitations importantes du fournisseur SQLite.
