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
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Fournisseur de base de données EF Core Microsoft SQL Server

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Microsoft SQL Server (y compris SQL Azure). Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Bien démarrer

Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.
* [Bien démarrer sur .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md)

* [Bien démarrer sur ASP.NET Core](../../get-started/aspnetcore/index.md)

* [Exemple d’application UnicornStore](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* Microsoft SQL Server (versions 2008 et suivantes)

## <a name="supported-platforms"></a>Plateformes prises en charge

* .NET Framework (versions 4.5.1 et suivantes)

* .NET Core

* Mono (versions 4.2.0 et suivantes)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
