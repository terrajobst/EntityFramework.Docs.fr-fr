---
title: Fournisseur de base de données Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812105"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Fournisseur de base de données EF Core Microsoft SQL Server

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure). Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

# <a name="net-core-clitabdotnet-core-cli"></a>[CLI .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Depuis la version 3.0.0, le fournisseur référence Microsoft.Data.SqlClient (les versions précédentes dépendaient de System.Data.SqlClient). Si votre projet dépend directement de SqlClient, assurez-vous qu’il fait référence au package approprié.

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* Microsoft SQL Server (versions 2012 et suivantes)
