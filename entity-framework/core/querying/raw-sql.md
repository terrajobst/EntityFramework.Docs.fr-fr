---
title: "Requêtes SQL brut - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="10109-102">Requêtes SQL brut</span><span class="sxs-lookup"><span data-stu-id="10109-102">Raw SQL Queries</span></span>

<span data-ttu-id="10109-103">Entity Framework Core permet de liste déroulante pour les requêtes SQL bruts lorsque vous travaillez avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="10109-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="10109-104">Cela peut être utile si la requête que vous voulez effectuer ne peut pas être exprimée à l’aide de LINQ ou à l’aide d’une requête LINQ se traduit par inefficace SQL envoyée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="10109-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="10109-105">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="10109-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="10109-106">Limitations</span><span class="sxs-lookup"><span data-stu-id="10109-106">Limitations</span></span>

<span data-ttu-id="10109-107">Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL bruts :</span><span class="sxs-lookup"><span data-stu-id="10109-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="10109-108">Requêtes SQL peuvent uniquement servir à retourner des types d’entités qui font partie de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="10109-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="10109-109">Il existe une amélioration de notre backlog à [Activer retour des types d’ad hoc à partir des requêtes SQL brutes](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="10109-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="10109-110">La requête SQL doit retourner des données pour toutes les propriétés du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="10109-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="10109-111">Les noms de colonnes dans le jeu de résultats doivent correspondre aux noms de colonnes mappées aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="10109-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="10109-112">Notez que cela est différent d’EF6 où mappage de propriété/colonne a été ignorée pour les requêtes SQL bruts et noms devaient correspondre aux noms de propriété de colonne de jeu de résultats.</span><span class="sxs-lookup"><span data-stu-id="10109-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="10109-113">La requête SQL ne peut pas contenir les données associées.</span><span class="sxs-lookup"><span data-stu-id="10109-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="10109-114">Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de la `Include` opérateur pour retourner les données associées (consultez [, y compris les données associées](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="10109-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="10109-115">Requêtes SQL brutes de base</span><span class="sxs-lookup"><span data-stu-id="10109-115">Basic raw SQL queries</span></span>

<span data-ttu-id="10109-116">Vous pouvez utiliser la *FromSql* méthode d’extension pour lancer une requête LINQ basée sur une requête SQL brute.</span><span class="sxs-lookup"><span data-stu-id="10109-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="10109-117">Les requêtes SQL bruts peuvent servir à exécuter une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="10109-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="10109-118">Passage de paramètres</span><span class="sxs-lookup"><span data-stu-id="10109-118">Passing parameters</span></span>

<span data-ttu-id="10109-119">Comme avec toute API qui accepte SQL, il est important de paramétrer des entrées d’utilisateur pour protéger contre une attaque par injection de SQL.</span><span class="sxs-lookup"><span data-stu-id="10109-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="10109-120">Vous pouvez inclure des espaces réservés de paramètre dans la chaîne de requête SQL et fournissez ensuite les valeurs de paramètre en tant qu’arguments supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="10109-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="10109-121">Les valeurs de paramètres que vous fournissez seront automatiquement converties en un `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="10109-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="10109-122">L’exemple suivant passe un paramètre unique à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="10109-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="10109-123">Alors que cela peut vous paraître comme `String.Format` syntaxe, la valeur fournie est encapsulée dans un paramètre et le nom de paramètre généré inséré où le `{0}` espace réservé a été spécifié.</span><span class="sxs-lookup"><span data-stu-id="10109-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="10109-124">Ceci est la même requête, mais à l’aide de la syntaxe d’interpolation de chaîne, qui est pris en charge dans EF Core 2.0 et versions ultérieures :</span><span class="sxs-lookup"><span data-stu-id="10109-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="10109-125">Vous pouvez également construire un objet DbParameter et fournir en tant que valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="10109-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="10109-126">Cela vous permet d’utiliser les paramètres nommés dans la chaîne de requête SQL</span><span class="sxs-lookup"><span data-stu-id="10109-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="10109-127">Composition avec LINQ</span><span class="sxs-lookup"><span data-stu-id="10109-127">Composing with LINQ</span></span>

<span data-ttu-id="10109-128">Si la requête SQL peut être composée de la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="10109-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="10109-129">Les requêtes SQL qui peuvent être composées sur en cours avec le `SELECT` (mot clé).</span><span class="sxs-lookup"><span data-stu-id="10109-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="10109-130">L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et les dessus à l’aide de LINQ pour effectuer le filtrage et le tri.</span><span class="sxs-lookup"><span data-stu-id="10109-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="10109-131">Y compris les données associées</span><span class="sxs-lookup"><span data-stu-id="10109-131">Including related data</span></span>

<span data-ttu-id="10109-132">Composition avec les opérateurs LINQ peut servir à inclure les données associées dans la requête.</span><span class="sxs-lookup"><span data-stu-id="10109-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="10109-133">**Utilisez toujours le paramétrage pour les requêtes SQL brutes :** API acceptant une brute SQL de chaîne tel que `FromSql` et `ExecuteSqlCommand` autorise les valeurs à passer facilement en tant que paramètres.</span><span class="sxs-lookup"><span data-stu-id="10109-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="10109-134">En plus de valider l’entrée d’utilisateur, vous devez toujours utiliser le paramétrage pour toutes les valeurs utilisées dans une requête SQL brut/commande.</span><span class="sxs-lookup"><span data-stu-id="10109-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="10109-135">Si vous utilisez la concaténation de chaînes pour générer dynamiquement une partie de la chaîne de requête, vous êtes responsable de la validation d’entrée pour vous protéger contre les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="10109-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
