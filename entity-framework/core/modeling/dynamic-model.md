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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="bee6b-102">Alternance entre plusieurs modèles ayant le même type DbContext</span><span class="sxs-lookup"><span data-stu-id="bee6b-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="bee6b-103">Le modèle intégré `OnModelCreating` peut utiliser une propriété sur le contexte pour modifier la façon dont le modèle est généré.</span><span class="sxs-lookup"><span data-stu-id="bee6b-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="bee6b-104">Par exemple, il peut être utilisé pour exclure une certaine propriété :</span><span class="sxs-lookup"><span data-stu-id="bee6b-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="bee6b-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="bee6b-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="bee6b-106">Toutefois, si vous avez essayé d’effectuer les opérations ci-dessus sans modification supplémentaire, vous obtiendriez le même modèle chaque fois qu’un nouveau contexte est créé pour une valeur de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="bee6b-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="bee6b-107">Cela est dû au fait que le mécanisme de mise en cache du modèle EF utilise pour améliorer les performances en appelant uniquement `OnModelCreating` une fois et en mettant en cache le modèle.</span><span class="sxs-lookup"><span data-stu-id="bee6b-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="bee6b-108">Par défaut, EF suppose que, pour un type de contexte donné, le modèle sera le même.</span><span class="sxs-lookup"><span data-stu-id="bee6b-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="bee6b-109">Pour ce faire, l’implémentation par défaut de `IModelCacheKeyFactory` retourne une clé qui contient simplement le type de contexte.</span><span class="sxs-lookup"><span data-stu-id="bee6b-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="bee6b-110">Pour changer cela, vous devez remplacer le service `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="bee6b-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="bee6b-111">La nouvelle implémentation doit retourner un objet qui peut être comparé à d’autres clés de modèle à l’aide de la méthode `Equals` qui prend en compte toutes les variables qui affectent le modèle :</span><span class="sxs-lookup"><span data-stu-id="bee6b-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
