---
title: Fractionnement de table-EF Core
description: Comment configurer le fractionnement de table à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824556"
---
# <a name="table-splitting"></a><span data-ttu-id="df3b6-103">Fractionnement de table</span><span class="sxs-lookup"><span data-stu-id="df3b6-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="df3b6-104">Cette fonctionnalité est une nouveauté de EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="df3b6-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="df3b6-105">EF Core permet de mapper deux entités ou plus sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="df3b6-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="df3b6-106">C’est ce que l’on appelle le _fractionnement de table_ ou le _partage de table_.</span><span class="sxs-lookup"><span data-stu-id="df3b6-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="df3b6-107">Configuration</span><span class="sxs-lookup"><span data-stu-id="df3b6-107">Configuration</span></span>

<span data-ttu-id="df3b6-108">Pour utiliser le fractionnement de table, les types d’entités doivent être mappés à la même table, les clés primaires sont mappées aux mêmes colonnes et au moins une relation configurée entre la clé primaire d’un type d’entité et une autre dans la même table.</span><span class="sxs-lookup"><span data-stu-id="df3b6-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="df3b6-109">Un scénario courant de fractionnement de table n’utilise qu’un sous-ensemble des colonnes de la table pour améliorer les performances ou l’encapsulation.</span><span class="sxs-lookup"><span data-stu-id="df3b6-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="df3b6-110">Dans cet exemple `Order` représente un sous-ensemble de `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="df3b6-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="df3b6-111">Outre la configuration requise, nous appelons `Property(o => o.Status).HasColumnName("Status")` pour mapper `DetailedOrder.Status` à la même colonne que `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="df3b6-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="df3b6-112">Pour plus de contexte, consultez l' [exemple de projet complet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="df3b6-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="df3b6-113">Contrôle</span><span class="sxs-lookup"><span data-stu-id="df3b6-113">Usage</span></span>

<span data-ttu-id="df3b6-114">L’enregistrement et l’interrogation d’entités à l’aide du fractionnement de table s’effectuent de la même façon que pour les autres entités.</span><span class="sxs-lookup"><span data-stu-id="df3b6-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="df3b6-115">Et à partir de EF Core 3,0, la référence d’entité dépendante peut être `null`.</span><span class="sxs-lookup"><span data-stu-id="df3b6-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="df3b6-116">Si toutes les colonnes utilisées par l’entité dépendante sont `NULL` est la base de données, aucune instance n’est créée lors de la requête.</span><span class="sxs-lookup"><span data-stu-id="df3b6-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="df3b6-117">Toutes les propriétés sont également facultatives et définies sur `null`, ce qui n’est peut-être pas prévu.</span><span class="sxs-lookup"><span data-stu-id="df3b6-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="df3b6-118">Jetons d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="df3b6-118">Concurrency tokens</span></span>

<span data-ttu-id="df3b6-119">Si l’un des types d’entité partageant une table a un jeton d’accès concurrentiel, il doit être inclus dans tous les autres types d’entité pour éviter une valeur de jeton d’accès concurrentiel obsolète lorsqu’une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="df3b6-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="df3b6-120">Pour éviter de l’exposer au code consommateur, il est possible d’en créer un dans l’État Shadow.</span><span class="sxs-lookup"><span data-stu-id="df3b6-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
