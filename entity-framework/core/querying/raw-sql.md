---
title: 'Requêtes SQL brutes : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 21cb688d6775039def3b0be12768da71b5d96531
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997142"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="d8596-102">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="d8596-102">Raw SQL Queries</span></span>

<span data-ttu-id="d8596-103">Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="d8596-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="d8596-104">Cela peut être utile si la requête que vous voulez effectuer ne peut pas être exprimée à l’aide de LINQ ou que l’utilisation d’une requête LINQ se traduit du SQL inefficace envoyé à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d8596-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span> <span data-ttu-id="d8596-105">Les requêtes SQL brutes peuvent retourner des types d’entités ou, à partir d’EF Core 2.1, des [types de requête](xref:core/modeling/query-types) qui font partie de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="d8596-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="d8596-106">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8596-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="d8596-107">Limitations</span><span class="sxs-lookup"><span data-stu-id="d8596-107">Limitations</span></span>

<span data-ttu-id="d8596-108">Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :</span><span class="sxs-lookup"><span data-stu-id="d8596-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="d8596-109">La requête SQL doit retourner des données pour toutes les propriétés du type d’entité ou de requête.</span><span class="sxs-lookup"><span data-stu-id="d8596-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="d8596-110">Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="d8596-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="d8596-111">Notez que cela est différent à compter d’EF6, où le mappage de propriétés/colonnes était ignoré pour les requêtes SQL brutes et où les noms de colonne du jeu de résultats devaient correspondre aux noms des propriétés.</span><span class="sxs-lookup"><span data-stu-id="d8596-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="d8596-112">La requête SQL ne peut pas contenir de données associées.</span><span class="sxs-lookup"><span data-stu-id="d8596-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="d8596-113">Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="d8596-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="d8596-114">Les instructions `SELECT` passées à cette méthode doivent généralement être composables : si EF Core a besoin évaluer des opérateurs de requête supplémentaires sur le serveur (par exemple, pour traduire les opérateurs LINQ appliqués après `FromSql`), le SQL fourni sera considéré comme une sous-requête.</span><span class="sxs-lookup"><span data-stu-id="d8596-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="d8596-115">Cela signifie que l’instruction SQL passée ne doit pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-requête, comme :</span><span class="sxs-lookup"><span data-stu-id="d8596-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="d8596-116">un point-virgule de fin</span><span class="sxs-lookup"><span data-stu-id="d8596-116">a trailing semicolon</span></span>
  * <span data-ttu-id="d8596-117">Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="d8596-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="d8596-118">Sur le serveur SQL Server, une clause `ORDER BY` n’est pas accompagnée de `TOP 100 PERCENT` dans la clause `SELECT`</span><span class="sxs-lookup"><span data-stu-id="d8596-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="d8596-119">Les instructions SQL autres que `SELECT` sont reconnues automatiquement en tant que non composables.</span><span class="sxs-lookup"><span data-stu-id="d8596-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="d8596-120">Par conséquent, les résultats complets des procédures stockées sont toujours retournés au client et tous les opérateurs LINQ appliqués après `FromSql` sont évalués en mémoire.</span><span class="sxs-lookup"><span data-stu-id="d8596-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="d8596-121">Requêtes SQL brutes de base</span><span class="sxs-lookup"><span data-stu-id="d8596-121">Basic raw SQL queries</span></span>

<span data-ttu-id="d8596-122">Vous pouvez utiliser la méthode d’extension *FromSql* pour lancer une requête LINQ basée sur une requête SQL brute.</span><span class="sxs-lookup"><span data-stu-id="d8596-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="d8596-123">Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="d8596-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="d8596-124">Passage de paramètres</span><span class="sxs-lookup"><span data-stu-id="d8596-124">Passing parameters</span></span>

<span data-ttu-id="d8596-125">Comme avec toute API qui accepte SQL, il est important de paramétrer toutes les entrées d’utilisateur pour protéger contre les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="d8596-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="d8596-126">Vous pouvez inclure des espaces réservés de paramètre dans la chaîne de requête SQL et fournir ensuite les valeurs de paramètre en tant qu’arguments supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="d8596-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="d8596-127">Les valeurs de paramètre que vous fournissez seront automatiquement converties en `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="d8596-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="d8596-128">L’exemple suivant passe un paramètre unique à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="d8596-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="d8596-129">Si cela ressemble à la syntaxe de `String.Format`, la valeur fournie est encapsulée dans un paramètre et le nom de paramètre généré est inséré là où l’espace réservé `{0}` a été spécifié.</span><span class="sxs-lookup"><span data-stu-id="d8596-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="d8596-130">Il s’agit de la même requête, mais avec la syntaxe d’interpolation de chaîne, qui est prise en charge dans EF Core 2.0 et versions ultérieures :</span><span class="sxs-lookup"><span data-stu-id="d8596-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="d8596-131">Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="d8596-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="d8596-132">Cela vous permet d’utiliser des paramètres nommés dans la chaîne de requête SQL</span><span class="sxs-lookup"><span data-stu-id="d8596-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="d8596-133">Composition avec LINQ</span><span class="sxs-lookup"><span data-stu-id="d8596-133">Composing with LINQ</span></span>

<span data-ttu-id="d8596-134">Si la requête SQL peut être composée dans la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="d8596-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="d8596-135">Les requêtes SQL qui peuvent être composées ont le mot-clé `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="d8596-135">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="d8596-136">L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et compose dessus à l’aide de LINQ pour effectuer le filtrage et le tri.</span><span class="sxs-lookup"><span data-stu-id="d8596-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="d8596-137">Inclusion de données associées</span><span class="sxs-lookup"><span data-stu-id="d8596-137">Including related data</span></span>

<span data-ttu-id="d8596-138">La composition avec les opérateurs LINQ peut servir à inclure les données associées dans la requête.</span><span class="sxs-lookup"><span data-stu-id="d8596-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="d8596-139">**Utilisez toujours le paramétrage pour les requêtes SQL brutes : les API**  acceptant une chaîne SQL brute comme `FromSql` et `ExecuteSqlCommand` autorisent les valeurs à passer facilement en tant que paramètres.</span><span class="sxs-lookup"><span data-stu-id="d8596-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="d8596-140">En plus de valider l’entrée utilisateur, vous devez toujours utiliser le paramétrage pour toutes les valeurs utilisées dans une commande/requête en SQL brut.</span><span class="sxs-lookup"><span data-stu-id="d8596-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="d8596-141">Si vous utilisez la concaténation de chaînes pour générer dynamiquement une partie de la chaîne de requête, vous êtes responsable de la validation de l’entrée pour vous protéger contre les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="d8596-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
