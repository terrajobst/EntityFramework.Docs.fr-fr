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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="630ec-102">Alternant entre plusieurs modèles avec le même type DbContext</span><span class="sxs-lookup"><span data-stu-id="630ec-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="630ec-103">Le modèle créé `OnModelCreating` Impossible d’utiliser une propriété sur le contexte pour modifier la façon dont le modèle est construit.</span><span class="sxs-lookup"><span data-stu-id="630ec-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="630ec-104">Par exemple, il peut être utilisé pour exclure une certaine propriété :</span><span class="sxs-lookup"><span data-stu-id="630ec-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="630ec-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="630ec-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="630ec-106">Toutefois si vous avez essayé d’effectuer la méthode ci-dessus sans modification supplémentaire vous obtiendriez le même modèle chaque fois qu’un nouveau contexte est créé pour toute valeur de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="630ec-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="630ec-107">Cela est provoqué par le modèle de mise en cache du mécanisme utilisé par Entity Framework pour améliorer les performances en appelant uniquement `OnModelCreating` une seule fois et le modèle de mise en cache.</span><span class="sxs-lookup"><span data-stu-id="630ec-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="630ec-108">Par défaut, EF suppose que pour n’importe quel contexte donné le modèle de type sera le même.</span><span class="sxs-lookup"><span data-stu-id="630ec-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="630ec-109">Pour effectuer cette opération l’implémentation par défaut de `IModelCacheKeyFactory` retourne une clé qui contient uniquement le type de contexte.</span><span class="sxs-lookup"><span data-stu-id="630ec-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="630ec-110">Pour modifier cela vous devez remplacer le `IModelCacheKeyFactory` service.</span><span class="sxs-lookup"><span data-stu-id="630ec-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="630ec-111">La nouvelle implémentation doit retourner un objet qui peut être comparé aux autres clés de modèle à l’aide de la `Equals` méthode qui prend en compte toutes les variables qui affectent le modèle :</span><span class="sxs-lookup"><span data-stu-id="630ec-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
