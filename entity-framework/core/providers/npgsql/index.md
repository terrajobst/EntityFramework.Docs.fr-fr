---
title: "Fournisseur de base de données Npgsql pour PostgreSQL - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>Fournisseur de base de données EF Core Npgsql pour PostgreSQL

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec PostgreSQL. Le fournisseur est géré dans le cadre du [projet Npgsql](http://www.npgsql.org).

> [!NOTE]  
> Il n’est pas géré dans le cadre du projet Entity Framework Core. Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.

## <a name="install"></a>Installer

Installez le [package NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>Bien démarrer

Pour commencer, consultez la [documentation de Npgsql](http://www.npgsql.org/efcore/index.html).

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* PostgreSQL

## <a name="supported-platforms"></a>Plateformes prises en charge

* .NET Framework (versions 4.5.1 et suivantes)

* .NET Core

* Mono (versions 4.2.0 et suivantes)
