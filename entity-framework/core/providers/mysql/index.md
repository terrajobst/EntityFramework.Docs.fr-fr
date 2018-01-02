---
title: "Fournisseur de base de données MySQL - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="mysql-ef-core-database-provider"></a>Fournisseur de base de données EF Core MySQL

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec MySQL. Il est géré dans le cadre du [projet MySQL](http://dev.mysql.com).

> [!WARNING]  
> Ce fournisseur est disponible en préversion.

> [!NOTE]  
> Il n’est pas géré dans le cadre du projet Entity Framework Core. Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.

## <a name="install"></a>Installer

Installez le [package NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Bien démarrer

Consultez [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Bien démarrer avec le fournisseur EF Core MySQL et Connector/Net 7.0.4).

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* MySQL

## <a name="supported-platforms"></a>Plateformes prises en charge

* .NET Framework (versions 4.5.1 et suivantes)

* .NET Core
