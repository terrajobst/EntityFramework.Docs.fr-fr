---
title: "Fournisseur de base de données Microsoft SQL Server Compact Edition - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Fournisseur de base de données EF Core Microsoft SQL Server Compact Edition

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec SQL Server Compact Edition. Le fournisseur est géré dans le cadre du [projet GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Il n’est pas géré dans le cadre du projet Entity Framework Core. Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.

## <a name="install"></a>Installez

Pour utiliser SQL Server Compact Edition 4.0, installez le [package NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Pour utiliser SQL Server Compact Edition 3.5, installez le [package NuGet EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Bien démarrer

Consultez la [documentation de prise en main sur le site du projet](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Plateformes prises en charge

* .NET Framework (versions 4.5.1 et suivantes)
