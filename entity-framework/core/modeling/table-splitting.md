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
# <a name="table-splitting"></a>Fractionnement de table

>[!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.0.

EF Core permet de mapper deux ou plusieurs entités à une seule ligne. Il s’agit _fractionnement de table_ ou _table partage_.

## <a name="configuration"></a>Configuration

Pour utiliser les types d’entités doivent être mappées à la même table de fractionnement de table, avoir les clés primaires mappées vers les mêmes colonnes et au moins une relation configuré entre la clé primaire d’un type d’entité et l’autre dans la même table.

Un scénario courant pour le fractionnement de table est en utilisant uniquement un sous-ensemble des colonnes du tableau pour supérieur de performances ou d’encapsulation.

Dans cet exemple `Order` représente un sous-ensemble des `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

En plus de la configuration requise que nous appelons `HasBaseType((string)null)` afin d’éviter le mappage `DetailedOrder` dans la même hiérarchie que `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Consultez le [exemple complet de projet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pour plus de contexte.

## <a name="usage"></a>Utilisation

L’enregistrement et l’interrogation des entités à l’aide du fractionnement de table s’effectuées de la même façon que d’autres entités, la seule différence est que toutes les entités qui partage une ligne doivent être suivies pour l’insertion.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Jetons d’accès concurrentiel

Si un des types d’entités partage une table possède un jeton d’accès concurrentiel il doit être inclus dans tous les autres types d’entité afin d’éviter une valeur de jeton d’accès concurrentiel obsolètes lorsque qu’un seul des entités mappées à la même table est mise à jour.

Pour éviter d’exposer au code de consommation, il est possible de créer un dans l’état de l’ombre.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]