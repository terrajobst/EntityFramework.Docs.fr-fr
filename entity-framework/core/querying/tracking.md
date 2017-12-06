---
title: "Suivi de Visual Studio. Aucun suivi des requêtes - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Suivi de Visual Studio. Aucun suivi des requêtes

Contrôles de comportement de suivi ou non de Entity Framework Core conserve les informations relatives à une instance d’entité dans sa traceur de modifications. Si une entité est suivie, toutes les modifications détectées dans l’entité sont rendues persistantes dans la base de données au cours de `SaveChanges()`. Entity Framework Core sera correctif également des propriétés de navigation entre les entités qui sont obtenues à partir d’une requête de suivi et des entités qui ont été précédemment chargées dans l’instance de DbContext.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="tracking-queries"></a>Requêtes de suivi

Par défaut, suivez les requêtes qui retournent des types d’entités. Cela signifie que vous pouvez apporter des modifications à des instances d’entité et ont ces modifications rendues persistantes par `SaveChanges()`.

Dans l’exemple suivant, la modification de l’évaluation de blogs sera détectée et rendues persistantes dans la base de données au cours de `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Aucun suivi des requêtes

Aucune requête de suivi n’est utiles lorsque les résultats sont utilisés dans un scénario en lecture seule. Ils sont plus rapides à exécuter, car il est inutile de modifier le programme d’installation des informations de suivi.

Vous pouvez permuter une requête individuelle pour être non suivi :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Vous pouvez également modifier le comportement au niveau de l’instance de contexte de suivi par défaut :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Aucune requête de suivi toujours n’effectuer la résolution d’identité dans la requête de la cause. Si le jeu de résultats contient plusieurs fois à la même entité, la même instance de la classe d’entité s’affichera pour chaque occurrence du jeu de résultats. Toutefois, les références faibles sont utilisés pour effectuer le suivi des entités qui ont déjà été retournées. Si un résultat antérieur avec la même identité est hors de portée, et le garbage collection s’exécute, vous risquez d’obtenir une nouvelle instance de l’entité. Pour plus d’informations, consultez [requête fonctionnement](overview.md).

## <a name="tracking-and-projections"></a>Le suivi et les projections

Même si le type de résultat de la requête n’est pas un type d’entité, si le résultat contient des types d’entités qu’ils sont toujours suivies par défaut. Dans la requête suivante, qui retourne un type anonyme, les instances de `Blog` dans le résultat de suivi ensemble.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Si le jeu de résultats ne contient pas tous les types d’entité, aucun suivi n’est effectuée. Dans la requête suivante, qui retourne un type anonyme avec certaines des valeurs de l’entité (mais aucune instance du type d’entité réel), il n’existe aucun suivi effectuées.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
