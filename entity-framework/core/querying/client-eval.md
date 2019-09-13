---
title: 'Évaluation sur le client ou le serveur : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921698"
---
# <a name="client-vs-server-evaluation"></a>Évaluation sur le client ou le serveur

Entity Framework Core prend en charge l’évaluation de parties de la requête sur le client et l’envoi de parties de celle-ci vers la base de données. Il incombe au fournisseur de base de données de déterminer les parties de la requête qui seront évaluées dans la base de données.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="client-evaluation"></a>Évaluation sur le client

Dans l’exemple suivant, une méthode d’assistance est utilisée afin de normaliser les URL pour les blogs qui sont retournés à partir d’une base de données SQL Server. Étant donné que le fournisseur SQL Server n’a pas connaissance de la façon dont cette méthode est implémentée, il n’est pas possible de la traduire en SQL. Tous les autres aspects de la requête sont évalués dans la base de données, mais la transmission de la valeur `URL` retournée via cette méthode est exécutée sur le client.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a>Problèmes de performances d’évaluation sur le client

Si l’évaluation sur le client peut être très utile, dans certains cas, cela peut entraîner une dégradation des performances. Considérez la requête suivante, où la méthode d’assistance est maintenant utilisée dans un filtre. Étant donné que cela ne peut pas être effectué dans la base de données, toutes les données sont extraites en mémoire, puis le filtre est appliqué sur le client. Selon la quantité de données et la mesure dans laquelle ces données sont filtrées, cela peut entraîner une dégradation des performances.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Journalisation de l’évaluation sur le client

Par défaut, EF Core consigne un avertissement lorsque l’évaluation sur le client est effectuée. Consultez [Journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de la journalisation. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Comportement facultatif : lever une exception pour l’évaluation sur le client

Vous pouvez modifier le comportement lorsque l’évaluation sur le client survient, entre journaliser ou ne rien faire. Cette opération est effectuée lors de la définition des options pour votre contexte, généralement dans `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
