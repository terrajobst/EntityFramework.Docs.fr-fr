---
title: Créer et supprimer des API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688627"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="405b0-102">Créer et supprimer des API</span><span class="sxs-lookup"><span data-stu-id="405b0-102">Create and Drop APIs</span></span>

<span data-ttu-id="405b0-103">Les méthodes EnsureCreated et EnsureDeleted fournissent une alternative légère à [Migrations](migrations/index.md) pour gérer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="405b0-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="405b0-104">Ces méthodes sont utiles dans les scénarios lorsque les données sont temporaires et peuvent être supprimées lorsque le schéma est modifié.</span><span class="sxs-lookup"><span data-stu-id="405b0-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="405b0-105">Par exemple lors d’une création de prototypes, dans les tests, ou pour les caches locaux.</span><span class="sxs-lookup"><span data-stu-id="405b0-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="405b0-106">Certains fournisseurs (en particulier ceux non relationnelles) ne prennent pas en charge les Migrations.</span><span class="sxs-lookup"><span data-stu-id="405b0-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="405b0-107">Pour ces fournisseurs, EnsureCreated est souvent le moyen le plus simple pour initialiser le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="405b0-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="405b0-108">EnsureCreated et Migrations ne fonctionnent pas bien ensemble.</span><span class="sxs-lookup"><span data-stu-id="405b0-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="405b0-109">Si vous utilisez des Migrations, n’utilisez pas EnsureCreated pour initialiser le schéma.</span><span class="sxs-lookup"><span data-stu-id="405b0-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="405b0-110">Transition à partir de EnsureCreated aux Migrations n’est pas une expérience transparente.</span><span class="sxs-lookup"><span data-stu-id="405b0-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="405b0-111">Pour cela, le plus simple consiste à supprimer la base de données et de créer de nouveau à l’aide des Migrations.</span><span class="sxs-lookup"><span data-stu-id="405b0-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="405b0-112">Si vous prévoyez d’utiliser des migrations à l’avenir, il est préférable de simplement commencer avec des Migrations au lieu d’utiliser EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="405b0-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="405b0-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="405b0-113">EnsureDeleted</span></span>

<span data-ttu-id="405b0-114">La méthode EnsureDeleted sera supprimée de la base de données si elle existe.</span><span class="sxs-lookup"><span data-stu-id="405b0-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="405b0-115">Si vous n’avez pas les autorisations appropriées, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="405b0-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="405b0-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="405b0-116">EnsureCreated</span></span>

<span data-ttu-id="405b0-117">EnsureCreated crée la base de données si elle n’existe pas et initialiser le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="405b0-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="405b0-118">Si des tables existent (y compris les tables pour une autre classe DbContext), le schéma ne sont pas initialisé.</span><span class="sxs-lookup"><span data-stu-id="405b0-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="405b0-119">Les versions asynchrones de ces méthodes sont également disponibles.</span><span class="sxs-lookup"><span data-stu-id="405b0-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="405b0-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="405b0-120">SQL Script</span></span>

<span data-ttu-id="405b0-121">Pour obtenir le SQL utilisé par EnsureCreated, vous pouvez utiliser la méthode GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="405b0-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="405b0-122">Plusieurs classes de DbContext</span><span class="sxs-lookup"><span data-stu-id="405b0-122">Multiple DbContext classes</span></span>

<span data-ttu-id="405b0-123">EnsureCreated fonctionne uniquement lorsque aucune table n’est présent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="405b0-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="405b0-124">Si nécessaire, vous pouvez écrire votre propre vérification pour voir si le schéma doit être initialisé et utiliser le service IRelationalDatabaseCreator sous-jacent pour initialiser le schéma.</span><span class="sxs-lookup"><span data-stu-id="405b0-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
