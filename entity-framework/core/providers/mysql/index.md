---
title: "Fournisseur de base de données MySQL - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="fcd68-102">Fournisseur de base de données EF Core MySQL</span><span class="sxs-lookup"><span data-stu-id="fcd68-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="fcd68-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec MySQL.</span><span class="sxs-lookup"><span data-stu-id="fcd68-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="fcd68-104">Il est géré dans le cadre du [projet MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="fcd68-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="fcd68-105">Ce fournisseur est disponible en préversion.</span><span class="sxs-lookup"><span data-stu-id="fcd68-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="fcd68-106">Il n’est pas géré dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fcd68-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="fcd68-107">Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="fcd68-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="fcd68-108">Installez</span><span class="sxs-lookup"><span data-stu-id="fcd68-108">Install</span></span>

<span data-ttu-id="fcd68-109">Installez le [package NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="fcd68-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="fcd68-110">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="fcd68-110">Get Started</span></span>

<span data-ttu-id="fcd68-111">Consultez [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Bien démarrer avec le fournisseur EF Core MySQL et Connector/Net 7.0.4).</span><span class="sxs-lookup"><span data-stu-id="fcd68-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="fcd68-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="fcd68-112">Supported Database Engines</span></span>

* <span data-ttu-id="fcd68-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="fcd68-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="fcd68-114">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="fcd68-114">Supported Platforms</span></span>

* <span data-ttu-id="fcd68-115">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="fcd68-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="fcd68-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcd68-116">.NET Core</span></span>

<span data-ttu-id="fcd68-117">Veillez à consulter la documentation MySQL pour les informations de compatibilité de version [ici](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) et [ici](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="fcd68-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
