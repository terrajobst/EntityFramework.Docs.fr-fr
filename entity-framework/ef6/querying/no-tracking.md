---
title: Pas de suivi des requêtes - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490112"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="f65e5-102">sans suivi</span><span class="sxs-lookup"><span data-stu-id="f65e5-102">No-Tracking Queries</span></span>
<span data-ttu-id="f65e5-103">Vous pouvez être amené à obtenir des entités d’une requête, mais n’a pas ces entités être suivis par le contexte.</span><span class="sxs-lookup"><span data-stu-id="f65e5-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="f65e5-104">Cela peut entraîner de meilleures performances lors de l’interrogation pour un grand nombre d’entités dans les scénarios en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="f65e5-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="f65e5-105">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f65e5-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f65e5-106">Une nouvelle méthode d’extension AsNoTracking permet à n’importe quelle requête à exécuter de cette façon.</span><span class="sxs-lookup"><span data-stu-id="f65e5-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="f65e5-107">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f65e5-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
