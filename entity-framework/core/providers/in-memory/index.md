---
title: Fournisseur de base de données InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 2aed8db79157f994bcc259acdf1f7c161017a50a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824404"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="90646-102">Fournisseur de base de données InMemory EF Core</span><span class="sxs-lookup"><span data-stu-id="90646-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="90646-103">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="90646-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="90646-104">Ceci peut s’avérer utile pour les tests, bien que le fournisseur SQLite en mode en mémoire constitue peut-être un remplacement de test plus approprié pour les bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="90646-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="90646-105">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="90646-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="90646-106">Installez</span><span class="sxs-lookup"><span data-stu-id="90646-106">Install</span></span>

<span data-ttu-id="90646-107">Installez le [package NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="90646-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="90646-108">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="90646-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="90646-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90646-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="90646-110">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="90646-110">Get Started</span></span>

<span data-ttu-id="90646-111">Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.</span><span class="sxs-lookup"><span data-stu-id="90646-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="90646-112">Test avec InMemory</span><span class="sxs-lookup"><span data-stu-id="90646-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="90646-113">Exemple de tests d’application UnicornStore</span><span class="sxs-lookup"><span data-stu-id="90646-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="90646-114">Moteurs de base de données pris en charge</span><span class="sxs-lookup"><span data-stu-id="90646-114">Supported Database Engines</span></span>

<span data-ttu-id="90646-115">Base de données en mémoire in-process (conçue uniquement à des fins de test)</span><span class="sxs-lookup"><span data-stu-id="90646-115">In-process memory database (designed for testing purposes only)</span></span>
