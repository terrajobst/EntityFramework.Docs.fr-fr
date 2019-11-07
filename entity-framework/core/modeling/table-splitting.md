---
title: Fractionnement de table-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656038"
---
# <a name="table-splitting"></a>Fractionnement de table

>[!NOTE]
> Cette fonctionnalité est une nouveauté de EF Core 2,0.

EF Core permet de mapper deux entités ou plus sur une seule ligne. C’est ce que l’on appelle le _fractionnement de table_ ou le _partage de table_.

## <a name="configuration"></a>Configuration

Pour utiliser le fractionnement de table, les types d’entités doivent être mappés à la même table, les clés primaires sont mappées aux mêmes colonnes et au moins une relation configurée entre la clé primaire d’un type d’entité et une autre dans la même table.

Un scénario courant de fractionnement de table n’utilise qu’un sous-ensemble des colonnes de la table pour améliorer les performances ou l’encapsulation.

Dans cet exemple `Order` représente un sous-ensemble de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Outre la configuration requise, nous appelons `Property(o => o.Status).HasColumnName("Status")` pour mapper `DetailedOrder.Status` à la même colonne que `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Pour plus de contexte, consultez l' [exemple de projet complet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Utilisation

L’enregistrement et l’interrogation d’entités à l’aide du fractionnement de table s’effectuent de la même façon que pour les autres entités. Et à partir de EF Core 3,0, la référence d’entité dépendante peut être `null`. Si toutes les colonnes utilisées par l’entité dépendante sont `NULL` est la base de données, aucune instance n’est créée lors de la requête. Toutes les propriétés sont également facultatives et définies sur `null`, ce qui n’est peut-être pas prévu.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Jetons d’accès concurrentiel

Si l’un des types d’entité partageant une table a un jeton d’accès concurrentiel, il doit être inclus dans tous les autres types d’entité pour éviter une valeur de jeton d’accès concurrentiel obsolète lorsqu’une seule des entités mappées à la même table est mise à jour.

Pour éviter de l’exposer au code consommateur, il est possible d’en créer un dans l’État Shadow.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
