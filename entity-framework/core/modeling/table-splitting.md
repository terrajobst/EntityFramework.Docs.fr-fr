---
title: Table de fractionnement - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562585"
---
# <a name="table-splitting"></a><span data-ttu-id="bdc2e-102">Fractionnement de table</span><span class="sxs-lookup"><span data-stu-id="bdc2e-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="bdc2e-103">Cette fonctionnalité est une nouveauté dans EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="bdc2e-104">EF Core permet de mapper deux ou plusieurs entités à une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="bdc2e-105">Il s’agit _fractionnement de table_ ou _table partage_.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="bdc2e-106">Configuration</span><span class="sxs-lookup"><span data-stu-id="bdc2e-106">Configuration</span></span>

<span data-ttu-id="bdc2e-107">Pour utiliser les types d’entités doivent être mappées à la même table de fractionnement de table, avoir les clés primaires mappées vers les mêmes colonnes et au moins une relation configuré entre la clé primaire d’un type d’entité et l’autre dans la même table.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="bdc2e-108">Un scénario courant pour le fractionnement de table est en utilisant uniquement un sous-ensemble des colonnes du tableau pour supérieur de performances ou d’encapsulation.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="bdc2e-109">Dans cet exemple `Order` représente un sous-ensemble des `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="bdc2e-110">En plus de la configuration requise que nous appelons `HasBaseType((string)null)` afin d’éviter le mappage `DetailedOrder` dans la même hiérarchie que `Order`.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="bdc2e-111">Consultez le [exemple complet de projet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pour plus de contexte.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="bdc2e-112">Utilisation</span><span class="sxs-lookup"><span data-stu-id="bdc2e-112">Usage</span></span>

<span data-ttu-id="bdc2e-113">L’enregistrement et l’interrogation des entités à l’aide du fractionnement de table s’effectuées de la même façon que d’autres entités, la seule différence est que toutes les entités qui partage une ligne doivent être suivies pour l’insertion.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="bdc2e-114">Jetons d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="bdc2e-114">Concurrency tokens</span></span>

<span data-ttu-id="bdc2e-115">Si un des types d’entités partage une table possède un jeton d’accès concurrentiel il doit être inclus dans tous les autres types d’entité afin d’éviter une valeur de jeton d’accès concurrentiel obsolètes lorsque qu’un seul des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bdc2e-116">Pour éviter d’exposer au code de consommation, il est possible de créer un dans l’état de l’ombre.</span><span class="sxs-lookup"><span data-stu-id="bdc2e-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]