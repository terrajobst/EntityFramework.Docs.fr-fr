---
title: Alternance entre plusieurs modèles avec le même type DbContext-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655723"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternance entre plusieurs modèles ayant le même type DbContext

Le modèle intégré `OnModelCreating` peut utiliser une propriété sur le contexte pour modifier la façon dont le modèle est généré. Par exemple, il peut être utilisé pour exclure une certaine propriété :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Toutefois, si vous avez essayé d’effectuer les opérations ci-dessus sans modification supplémentaire, vous obtiendriez le même modèle chaque fois qu’un nouveau contexte est créé pour une valeur de `IgnoreIntProperty`. Cela est dû au fait que le mécanisme de mise en cache du modèle EF utilise pour améliorer les performances en appelant uniquement `OnModelCreating` une fois et en mettant en cache le modèle.

Par défaut, EF suppose que, pour un type de contexte donné, le modèle sera le même. Pour ce faire, l’implémentation par défaut de `IModelCacheKeyFactory` retourne une clé qui contient simplement le type de contexte. Pour changer cela, vous devez remplacer le service `IModelCacheKeyFactory`. La nouvelle implémentation doit retourner un objet qui peut être comparé à d’autres clés de modèle à l’aide de la méthode `Equals` qui prend en compte toutes les variables qui affectent le modèle :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
