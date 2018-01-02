---
title: "Fournisseur de base de données IBM Data Server (DB2) - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>Fournisseurs de base de données EF Core IBM Data Server (DB2)

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec IBM Data Server. Le fournisseur est géré par IBM.

> [!NOTE]  
> Il n’est pas géré dans le cadre du projet Entity Framework Core. Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.

## <a name="install"></a>Installer

Pour utiliser IBM Data Server sur Windows, installez le [package NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Pour utiliser IBM Data Server sur Linux, installez le [package NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Bien démarrer

[Getting started with IBM .NET Provider for .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en) (Bien démarrer avec le fournisseur IBM .NET pour .NET Core)

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* zOS
* LUW, y compris IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Plateformes prises en charge

* .NET Framework (versions 4.6 et suivantes)
* .NET Core
