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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="e1698-102">Vs du client. Évaluation du serveur</span><span class="sxs-lookup"><span data-stu-id="e1698-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="e1698-103">Entity Framework Core prend en charge les parties de la requête en cours d’évaluation sur le client et les parties de celui-ci qui est envoyée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e1698-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="e1698-104">Il incombe au fournisseur de base de données de déterminer quelles parties de la requête seront évaluées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e1698-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e1698-105">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="e1698-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="e1698-106">Évaluation du client</span><span class="sxs-lookup"><span data-stu-id="e1698-106">Client evaluation</span></span>

<span data-ttu-id="e1698-107">Dans l’exemple suivant, une méthode d’assistance est utilisée afin de normaliser les URL pour les blogs qui sont retournées à partir d’une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1698-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="e1698-108">Étant donné que le fournisseur SQL Server n’a pas connaissance des comment cette méthode est implémentée, il n’est pas possible de le traduire en SQL.</span><span class="sxs-lookup"><span data-stu-id="e1698-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="e1698-109">Tous les autres aspects de la requête sont évalués dans la base de données, mais en transmettant retourné `URL` via cette méthode est exécutée sur le client.</span><span class="sxs-lookup"><span data-stu-id="e1698-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="e1698-110">La désactivation d’évaluation du client</span><span class="sxs-lookup"><span data-stu-id="e1698-110">Disabling client evaluation</span></span>

<span data-ttu-id="e1698-111">Lors de l’évaluation du client peut être très utile, dans certains cas, cela peut entraîner une dégradation des performances.</span><span class="sxs-lookup"><span data-stu-id="e1698-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="e1698-112">Considérez la requête suivante, où la méthode d’assistance est maintenant utilisée dans un filtre.</span><span class="sxs-lookup"><span data-stu-id="e1698-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="e1698-113">Étant donné que cela ne peut pas être effectuée dans la base de données, toutes les données sont extraites en mémoire, puis le filtre est appliqué sur le client.</span><span class="sxs-lookup"><span data-stu-id="e1698-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="e1698-114">Selon la quantité de données et la quantité de données est filtré, cela peut entraîner une dégradation des performances.</span><span class="sxs-lookup"><span data-stu-id="e1698-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="e1698-115">Par défaut, EF Core consigne un avertissement lors de l’évaluation du client est effectuée.</span><span class="sxs-lookup"><span data-stu-id="e1698-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="e1698-116">Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation.</span><span class="sxs-lookup"><span data-stu-id="e1698-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e1698-117">Vous pouvez modifier le comportement en cas d’évaluation du client soit lever ou ne rien faire.</span><span class="sxs-lookup"><span data-stu-id="e1698-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="e1698-118">Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1698-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
