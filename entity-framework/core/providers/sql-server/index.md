---
title: Fournisseur de base de données Microsoft SQL Server - EF Core
description: La documentation du fournisseur de base de données qui permet d’utiliser Entity Framework Core avec Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824772"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Fournisseur de base de données EF Core Microsoft SQL Server

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (notamment Azure SQL Database). Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Depuis la version 3.0.0, le fournisseur référence Microsoft.Data.SqlClient (les versions précédentes dépendaient de System.Data.SqlClient). Si votre projet dépend directement de SqlClient, assurez-vous qu’il fait référence au package Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* Microsoft SQL Server (versions 2012 et suivantes)
