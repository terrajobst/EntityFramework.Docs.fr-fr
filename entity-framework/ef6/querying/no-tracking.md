---
title: Pas de suivi des requêtes - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994238"
---
# <a name="no-tracking-queries"></a>sans suivi
Vous pouvez être amené à obtenir des entités d’une requête, mais n’a pas ces entités être suivis par le contexte. Cela peut entraîner de meilleures performances lors de l’interrogation pour un grand nombre d’entités dans les scénarios en lecture seule. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

Une nouvelle méthode d’extension AsNoTracking permet à n’importe quelle requête à exécuter de cette façon. Exemple :  

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
