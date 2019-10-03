---
title: Créer et supprimer des API-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813443"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="b5303-102">Créer et supprimer des API</span><span class="sxs-lookup"><span data-stu-id="b5303-102">Create and Drop APIs</span></span>

<span data-ttu-id="b5303-103">Les méthodes EnsureCreated et EnsureDeleted fournissent une alternative légère aux [migrations](migrations/index.md) pour la gestion du schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="b5303-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="b5303-104">Ces méthodes sont utiles dans les scénarios où les données sont temporaires et peuvent être supprimées lorsque le schéma est modifié.</span><span class="sxs-lookup"><span data-stu-id="b5303-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="b5303-105">Par exemple pendant le prototypage, les tests ou les caches locaux.</span><span class="sxs-lookup"><span data-stu-id="b5303-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="b5303-106">Certains fournisseurs (surtout non relationnels) ne prennent pas en charge les migrations.</span><span class="sxs-lookup"><span data-stu-id="b5303-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="b5303-107">Pour ces fournisseurs, EnsureCreated est souvent le moyen le plus simple d’initialiser le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="b5303-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="b5303-108">EnsureCreated et les migrations ne fonctionnent pas correctement ensemble.</span><span class="sxs-lookup"><span data-stu-id="b5303-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="b5303-109">Si vous utilisez des migrations, n’utilisez pas EnsureCreated pour initialiser le schéma.</span><span class="sxs-lookup"><span data-stu-id="b5303-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="b5303-110">La transition de EnsureCreated à des migrations n’est pas une expérience transparente.</span><span class="sxs-lookup"><span data-stu-id="b5303-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="b5303-111">La façon la plus simple de procéder consiste à supprimer la base de données et à la recréer à l’aide des migrations.</span><span class="sxs-lookup"><span data-stu-id="b5303-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="b5303-112">Si vous prévoyez d’utiliser des migrations à l’avenir, il est préférable de commencer par les migrations au lieu d’utiliser EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="b5303-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="b5303-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="b5303-113">EnsureDeleted</span></span>

<span data-ttu-id="b5303-114">La méthode EnsureDeleted supprime la base de données, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="b5303-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="b5303-115">Si vous ne disposez pas des autorisations appropriées, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="b5303-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="b5303-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="b5303-116">EnsureCreated</span></span>

<span data-ttu-id="b5303-117">EnsureCreated crée la base de données si elle n’existe pas et initialise le schéma de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b5303-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="b5303-118">Si des tables existent (y compris des tables pour une autre classe DbContext), le schéma ne sera pas initialisé.</span><span class="sxs-lookup"><span data-stu-id="b5303-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="b5303-119">Les versions asynchrones de ces méthodes sont également disponibles.</span><span class="sxs-lookup"><span data-stu-id="b5303-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="b5303-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="b5303-120">SQL Script</span></span>

<span data-ttu-id="b5303-121">Pour récupérer le SQL utilisé par EnsureCreated, vous pouvez utiliser la méthode GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="b5303-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="b5303-122">Plusieurs classes DbContext</span><span class="sxs-lookup"><span data-stu-id="b5303-122">Multiple DbContext classes</span></span>

<span data-ttu-id="b5303-123">EnsureCreated fonctionne uniquement si aucune table n’est présente dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b5303-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="b5303-124">Si nécessaire, vous pouvez écrire votre propre vérification pour voir si le schéma doit être initialisé et utiliser le service IRelationalDatabaseCreator sous-jacent pour initialiser le schéma.</span><span class="sxs-lookup"><span data-stu-id="b5303-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
