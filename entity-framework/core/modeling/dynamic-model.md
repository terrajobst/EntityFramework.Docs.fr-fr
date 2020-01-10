---
title: Alternance entre plusieurs modèles avec le même type DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781129"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternance entre plusieurs modèles ayant le même type DbContext

Le modèle intégré `OnModelCreating` peut utiliser une propriété du contexte pour modifier la façon dont le modèle est généré. Par exemple, supposons que vous souhaitiez configurer une entité différemment en fonction de certaines propriétés :

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Malheureusement, ce code ne fonctionnerait pas tel quel, car EF génère le modèle et s’exécute `OnModelCreating` une seule fois, en mettant en cache le résultat pour des raisons de performances. Toutefois, vous pouvez vous connecter au mécanisme de mise en cache du modèle pour que EF prenne en charge la propriété qui produit des modèles différents.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF utilise la `IModelCacheKeyFactory` pour générer des clés de cache pour les modèles ; par défaut, EF suppose que, pour tout type de contexte donné, le modèle sera le même, l’implémentation par défaut de ce service retourne donc une clé qui contient simplement le type de contexte. Pour produire différents modèles à partir du même type de contexte, vous devez remplacer le service `IModelCacheKeyFactory` par l’implémentation correcte. la clé générée est comparée à d’autres clés de modèle à l’aide de la méthode `Equals`, en tenant compte de toutes les variables qui affectent le modèle :

L’implémentation suivante prend en compte les `IgnoreIntProperty` lors de la génération d’une clé de cache de modèle :

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Enfin, inscrivez votre nouveau `IModelCacheKeyFactory` dans le `OnConfiguring`de votre contexte :

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Pour plus de contexte, consultez l' [exemple de projet complet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .
