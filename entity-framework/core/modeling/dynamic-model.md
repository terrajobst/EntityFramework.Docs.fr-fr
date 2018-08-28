---
title: Alternant entre plusieurs modèles avec le même type DbContext - EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994984"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternant entre plusieurs modèles avec le même type DbContext

Le modèle créé `OnModelCreating` Impossible d’utiliser une propriété sur le contexte pour modifier la façon dont le modèle est construit. Par exemple, il peut être utilisé pour exclure une certaine propriété :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Toutefois si vous avez essayé d’effectuer la méthode ci-dessus sans modification supplémentaire vous obtiendriez le même modèle chaque fois qu’un nouveau contexte est créé pour toute valeur de `IgnoreIntProperty`. Cela est provoqué par le modèle de mise en cache du mécanisme utilisé par Entity Framework pour améliorer les performances en appelant uniquement `OnModelCreating` une seule fois et le modèle de mise en cache.

Par défaut, EF suppose que pour n’importe quel contexte donné le modèle de type sera le même. Pour effectuer cette opération l’implémentation par défaut de `IModelCacheKeyFactory` retourne une clé qui contient uniquement le type de contexte. Pour modifier cela vous devez remplacer le `IModelCacheKeyFactory` service. La nouvelle implémentation doit retourner un objet qui peut être comparé aux autres clés de modèle à l’aide de la `Equals` méthode qui prend en compte toutes les variables qui affectent le modèle :

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
