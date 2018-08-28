---
title: Fournisseur de base de données InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993318"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="48233-102">Fournisseur de base de données InMemory EF Core</span><span class="sxs-lookup"><span data-stu-id="48233-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="48233-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="48233-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="48233-104">Ceci peut s’avérer utile pour les tests, bien que le fournisseur SQLite en mode en mémoire constitue peut-être un remplacement de test plus approprié pour les bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="48233-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="48233-105">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="48233-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="48233-106">Installez</span><span class="sxs-lookup"><span data-stu-id="48233-106">Install</span></span>

<span data-ttu-id="48233-107">Installez le [package NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="48233-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="48233-108">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="48233-108">Get Started</span></span>

<span data-ttu-id="48233-109">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="48233-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="48233-110">Test avec InMemory</span><span class="sxs-lookup"><span data-stu-id="48233-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="48233-111">Exemple de tests d’application UnicornStore</span><span class="sxs-lookup"><span data-stu-id="48233-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="48233-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="48233-112">Supported Database Engines</span></span>

* <span data-ttu-id="48233-113">Base de données en mémoire intégrée (conçue uniquement à des fins de test)</span><span class="sxs-lookup"><span data-stu-id="48233-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="48233-114">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="48233-114">Supported Platforms</span></span>

* <span data-ttu-id="48233-115">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="48233-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="48233-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="48233-116">.NET Core</span></span>

* <span data-ttu-id="48233-117">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="48233-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="48233-118">Plateforme Windows universelle</span><span class="sxs-lookup"><span data-stu-id="48233-118">Universal Windows Platform</span></span>
