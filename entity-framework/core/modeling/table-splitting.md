---
title: Fractionnement de table-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149184"
---
# <a name="table-splitting"></a><span data-ttu-id="855dc-102">Fractionnement de table</span><span class="sxs-lookup"><span data-stu-id="855dc-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="855dc-103">Cette fonctionnalité est une nouveauté de EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="855dc-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="855dc-104">EF Core permet de mapper deux entités ou plus sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="855dc-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="855dc-105">C’est ce que l’on appelle le _fractionnement de table_ ou le _partage de table_.</span><span class="sxs-lookup"><span data-stu-id="855dc-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="855dc-106">Configuration</span><span class="sxs-lookup"><span data-stu-id="855dc-106">Configuration</span></span>

<span data-ttu-id="855dc-107">Pour utiliser le fractionnement de table, les types d’entités doivent être mappés à la même table, les clés primaires sont mappées aux mêmes colonnes et au moins une relation configurée entre la clé primaire d’un type d’entité et une autre dans la même table.</span><span class="sxs-lookup"><span data-stu-id="855dc-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="855dc-108">Un scénario courant de fractionnement de table n’utilise qu’un sous-ensemble des colonnes de la table pour améliorer les performances ou l’encapsulation.</span><span class="sxs-lookup"><span data-stu-id="855dc-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="855dc-109">Dans cet exemple `Order` , représente un sous `DetailedOrder`-ensemble de.</span><span class="sxs-lookup"><span data-stu-id="855dc-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="855dc-110">Outre la configuration requise, nous appelons `Property(o => o.Status).HasColumnName("Status")` pour mapper `DetailedOrder.Status` à la même colonne que `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="855dc-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="855dc-111">Pour plus de contexte, consultez l' [exemple de projet complet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="855dc-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="855dc-112">Utilisation</span><span class="sxs-lookup"><span data-stu-id="855dc-112">Usage</span></span>

<span data-ttu-id="855dc-113">L’enregistrement et l’interrogation d’entités à l’aide du fractionnement de table s’effectuent de la même façon que pour les autres entités.</span><span class="sxs-lookup"><span data-stu-id="855dc-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="855dc-114">Et à partir de EF Core 3,0 la référence d’entité dépendante peut être `null`.</span><span class="sxs-lookup"><span data-stu-id="855dc-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="855dc-115">Si toutes les colonnes utilisées par l’entité dépendante `NULL` sont la base de données, aucune instance n’est créée lors de la requête.</span><span class="sxs-lookup"><span data-stu-id="855dc-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="855dc-116">Cela se produit également si toutes les propriétés sont facultatives et ont `null`la valeur, ce qui n’est peut-être pas prévu.</span><span class="sxs-lookup"><span data-stu-id="855dc-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="855dc-117">Jetons d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="855dc-117">Concurrency tokens</span></span>

<span data-ttu-id="855dc-118">Si l’un des types d’entité partageant une table a un jeton d’accès concurrentiel, il doit être inclus dans tous les autres types d’entité pour éviter une valeur de jeton d’accès concurrentiel obsolète lorsqu’une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="855dc-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="855dc-119">Pour éviter de l’exposer au code consommateur, il est possible d’en créer un dans l’État Shadow.</span><span class="sxs-lookup"><span data-stu-id="855dc-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]