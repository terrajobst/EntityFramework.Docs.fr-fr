---
title: "En alternant entre plusieurs modèles avec le même type DbContext - EF Core"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>En alternant entre plusieurs modèles avec le même type de DbContext

Le modèle généré `OnModelCreating` Impossible d’utiliser une propriété sur le contexte pour modifier la façon dont le modèle est construit. Par exemple, il peut être utilisé pour exclure une propriété donnée :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Toutefois si vous avez essayé de faire ci-dessus sans apporter de modifications supplémentaires vous pourriez obtenir le même modèle chaque fois qu’un nouveau contexte est créé pour toute valeur de `IgnoreIntProperty`. Cela est provoqué par le modèle de mise en cache de mécanisme EF utilise pour améliorer les performances en appelant uniquement `OnModelCreating` qu’une seule fois et le modèle de mise en cache.

Par défaut EF suppose que pour n’importe quel contexte donné le type du modèle sera le même. Pour cela l’implémentation par défaut de `IModelCacheKeyFactory` retourne une clé qui contient seulement le type de contexte. Pour modifier ce vous devez remplacer le `IModelCacheKeyFactory` service. La nouvelle implémentation doit retourner un objet qui peut être comparé à d’autres clés de modèle à l’aide de la `Equals` méthode qui prend en compte toutes les variables qui affectent le modèle :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
