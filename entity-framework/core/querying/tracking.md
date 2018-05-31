---
title: 'Requêtes avec suivi ou sans suivi : EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2017
ms.locfileid: "26053959"
---
# <a name="tracking-vs-no-tracking-queries"></a>Requêtes avec suivi ou sans suivi

Le comportement suivi contrôle si Entity Framework Core conserve les informations relatives à une instance d’entité dans son traceur de modifications. Si une entité est suivie, toutes les modifications détectées dans l’entité sont rendues persistantes dans la base de données pendant `SaveChanges()`. Entity Framework Core corrige également les propriétés de navigation entre les entités qui sont obtenues à partir d’une requête de suivi et les entités qui ont été précédemment chargées dans l’instance de DbContext.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="tracking-queries"></a>Requêtes avec suivi

Par défaut, les requêtes qui retournent des types d’entités ont le suivi activé. Cela signifie que vous pouvez apporter des modifications à ces instances d’entité et rendre ces modifications persistantes avec `SaveChanges()`.

Dans l’exemple suivant, la modification de l’évaluation des blogs sera détectée et rendue persistante dans la base de données pendant `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Pas de suivi des requêtes

Les requêtes sans suivi sont utiles lorsque les résultats sont utilisés dans un scénario en lecture seule. Elles sont plus rapides à exécuter, car il est inutile de configurer les informations de suivi des modifications.

Vous pouvez permuter une requête individuelle pour être sans suivi :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Vous pouvez également modifier le comportement de suivi par défaut au niveau de l’instance du contexte :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Les requêtes sans suivi continuent à effectuer la résolution d’identité lors de l’exécution de la requête. Si le jeu de résultats contient plusieurs fois à la même entité, la même instance de la classe d’entité s’affichera pour chaque occurrence du jeu de résultats. Toutefois, des références faibles sont utilisées pour effectuer le suivi des entités qui ont déjà été retournées. Si un résultat antérieur avec la même identité est hors de portée, et que le nettoyage de la mémoire s’exécute, vous risquez d’obtenir une nouvelle instance de l’entité. Pour plus d'informations, consultez la section [Fonctionnement d’une requête](overview.md).

## <a name="tracking-and-projections"></a>Suivi et projections

Même si le type de résultat de la requête n’est pas un type d’entité, si le résultat contient des types d’entité, le suivi est toujours activé par défaut. Dans la requête suivante, qui retourne un type anonyme, les instances de `Blog` dans le jeu de résultats sont suivies.

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

Si le jeu de résultats ne contient pas de types d’entité, aucun suivi n’est effectué. Dans la requête suivante, qui retourne un type anonyme avec certaines des valeurs de l’entité (mais aucune instance du type d’entité réel), aucun suivi n’est effectué.

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
