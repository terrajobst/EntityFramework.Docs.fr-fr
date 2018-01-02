---
title: "Fournisseur de base de données InMemory - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="b830c-102">Fournisseur de base de données InMemory EF Core</span><span class="sxs-lookup"><span data-stu-id="b830c-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="b830c-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="b830c-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="b830c-104">Cela est utile quand vous testez le code qui utilise Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b830c-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="b830c-105">Le fournisseur est géré dans le cadre du [projet GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="b830c-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="b830c-106">Installer</span><span class="sxs-lookup"><span data-stu-id="b830c-106">Install</span></span>

<span data-ttu-id="b830c-107">Installez le [package NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="b830c-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="b830c-108">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="b830c-108">Get Started</span></span>

<span data-ttu-id="b830c-109">Les ressources suivantes vous permettront de démarrer avec ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="b830c-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="b830c-110">Test avec InMemory</span><span class="sxs-lookup"><span data-stu-id="b830c-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="b830c-111">Exemple de tests d’application UnicornStore</span><span class="sxs-lookup"><span data-stu-id="b830c-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="b830c-112">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="b830c-112">Supported Database Engines</span></span>

* <span data-ttu-id="b830c-113">Base de données en mémoire intégrée (conçue uniquement à des fins de test)</span><span class="sxs-lookup"><span data-stu-id="b830c-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b830c-114">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="b830c-114">Supported Platforms</span></span>

* <span data-ttu-id="b830c-115">.NET Framework (versions 4.5.1 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="b830c-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="b830c-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b830c-116">.NET Core</span></span>

* <span data-ttu-id="b830c-117">Mono (versions 4.2.0 et suivantes)</span><span class="sxs-lookup"><span data-stu-id="b830c-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="b830c-118">Plateforme Windows universelle</span><span class="sxs-lookup"><span data-stu-id="b830c-118">Universal Windows Platform</span></span>
