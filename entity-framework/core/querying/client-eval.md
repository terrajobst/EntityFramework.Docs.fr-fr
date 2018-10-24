---
title: 'Évaluation sur le client ou le serveur : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 47e22be274d02b5221c638d07151d9607aa7e24f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250801"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="41ce9-102">Évaluation sur le client ou le serveur</span><span class="sxs-lookup"><span data-stu-id="41ce9-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="41ce9-103">Entity Framework Core prend en charge l’évaluation de parties de la requête sur le client et l’envoi de parties de celle-ci vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="41ce9-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="41ce9-104">Il incombe au fournisseur de base de données de déterminer les parties de la requête qui seront évaluées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="41ce9-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="41ce9-105">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="41ce9-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="41ce9-106">Évaluation sur le client</span><span class="sxs-lookup"><span data-stu-id="41ce9-106">Client evaluation</span></span>

<span data-ttu-id="41ce9-107">Dans l’exemple suivant, une méthode d’assistance est utilisée afin de normaliser les URL pour les blogs qui sont retournés à partir d’une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="41ce9-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="41ce9-108">Étant donné que le fournisseur SQL Server n’a pas connaissance de la façon dont cette méthode est implémentée, il n’est pas possible de la traduire en SQL.</span><span class="sxs-lookup"><span data-stu-id="41ce9-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="41ce9-109">Tous les autres aspects de la requête sont évalués dans la base de données, mais la transmission de la valeur `URL` retournée via cette méthode est exécutée sur le client.</span><span class="sxs-lookup"><span data-stu-id="41ce9-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="41ce9-110">Problèmes de performances d’évaluation sur le client</span><span class="sxs-lookup"><span data-stu-id="41ce9-110">Client evaluation performance issues</span></span>

<span data-ttu-id="41ce9-111">Si l’évaluation sur le client peut être très utile, dans certains cas, cela peut entraîner une dégradation des performances.</span><span class="sxs-lookup"><span data-stu-id="41ce9-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="41ce9-112">Considérez la requête suivante, où la méthode d’assistance est maintenant utilisée dans un filtre.</span><span class="sxs-lookup"><span data-stu-id="41ce9-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="41ce9-113">Étant donné que cela ne peut pas être effectué dans la base de données, toutes les données sont extraites en mémoire, puis le filtre est appliqué sur le client.</span><span class="sxs-lookup"><span data-stu-id="41ce9-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="41ce9-114">Selon la quantité de données et la mesure dans laquelle ces données sont filtrées, cela peut entraîner une dégradation des performances.</span><span class="sxs-lookup"><span data-stu-id="41ce9-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="41ce9-115">Journalisation de l’évaluation sur le client</span><span class="sxs-lookup"><span data-stu-id="41ce9-115">Client evaluation logging</span></span>

<span data-ttu-id="41ce9-116">Par défaut, EF Core consigne un avertissement lorsque l’évaluation sur le client est effectuée.</span><span class="sxs-lookup"><span data-stu-id="41ce9-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="41ce9-117">Consultez [Journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de la journalisation.</span><span class="sxs-lookup"><span data-stu-id="41ce9-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="41ce9-118">Comportement facultatif : lever une exception pour l’évaluation sur le client</span><span class="sxs-lookup"><span data-stu-id="41ce9-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="41ce9-119">Vous pouvez modifier le comportement lorsque l’évaluation sur le client survient, entre journaliser ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="41ce9-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="41ce9-120">Cette opération est effectuée lors de la définition des options pour votre contexte, généralement dans `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41ce9-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
