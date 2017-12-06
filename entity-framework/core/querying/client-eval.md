---
title: "Vs du client. Évaluation du serveur - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a>Vs du client. Évaluation du serveur

Entity Framework Core prend en charge les parties de la requête en cours d’évaluation sur le client et les parties de celui-ci qui est envoyée à la base de données. Il incombe au fournisseur de base de données de déterminer quelles parties de la requête seront évaluées dans la base de données.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="client-evaluation"></a>Évaluation du client

Dans l’exemple suivant, une méthode d’assistance est utilisée afin de normaliser les URL pour les blogs qui sont retournées à partir d’une base de données SQL Server. Étant donné que le fournisseur SQL Server n’a pas connaissance des comment cette méthode est implémentée, il n’est pas possible de le traduire en SQL. Tous les autres aspects de la requête sont évalués dans la base de données, mais en transmettant retourné `URL` via cette méthode est exécutée sur le client.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
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

## <a name="disabling-client-evaluation"></a>La désactivation d’évaluation du client

Lors de l’évaluation du client peut être très utile, dans certains cas, cela peut entraîner une dégradation des performances. Considérez la requête suivante, où la méthode d’assistance est maintenant utilisée dans un filtre. Étant donné que cela ne peut pas être effectuée dans la base de données, toutes les données sont extraites en mémoire, puis le filtre est appliqué sur le client. Selon la quantité de données et la quantité de données est filtré, cela peut entraîner une dégradation des performances.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Par défaut, EF Core consigne un avertissement lors de l’évaluation du client est effectuée. Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation. Vous pouvez modifier le comportement en cas d’évaluation du client soit lever ou ne rien faire. Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
