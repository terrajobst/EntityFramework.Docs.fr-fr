---
title: 'Requêtes SQL brutes : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813595"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="468a5-102">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="468a5-102">Raw SQL Queries</span></span>

<span data-ttu-id="468a5-103">Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="468a5-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="468a5-104">Cela peut être utile si la requête que vous souhaitez exécuter ne peut pas être exprimée à l’aide de LINQ, ou si l’utilisation d’une requête LINQ aboutit à une requête SQL inefficace.</span><span class="sxs-lookup"><span data-stu-id="468a5-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="468a5-105">Les requêtes SQL brutes peuvent retourner des types d’entité standard ou des [types d’entité sans clé](xref:core/modeling/keyless-entity-types) qui font partie de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="468a5-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="468a5-106">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="468a5-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="468a5-107">Requêtes SQL brutes de base</span><span class="sxs-lookup"><span data-stu-id="468a5-107">Basic raw SQL queries</span></span>

<span data-ttu-id="468a5-108">Vous pouvez utiliser la `FromSqlRaw` méthode d’extension pour commencer une requête LINQ basée sur une requête SQL brute.</span><span class="sxs-lookup"><span data-stu-id="468a5-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="468a5-109">Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="468a5-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="468a5-110">Passage de paramètres</span><span class="sxs-lookup"><span data-stu-id="468a5-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="468a5-111">**Toujours utiliser le paramétrage pour les requêtes SQL brutes**</span><span class="sxs-lookup"><span data-stu-id="468a5-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="468a5-112">Lorsque vous introduisez des valeurs fournies par l’utilisateur dans une requête SQL brute, vous devez veiller à éviter les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="468a5-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="468a5-113">En plus de valider le fait que ces valeurs ne contiennent pas de caractères non valides, utilisez toujours le paramétrage qui envoie les valeurs séparées du texte SQL.</span><span class="sxs-lookup"><span data-stu-id="468a5-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="468a5-114">En particulier, ne transmettez jamais une chaîne concaténée ou interpolée (`$""`) avec des valeurs non validées fournies par l’utilisateur dans `FromSqlRaw` ou `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="468a5-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="468a5-115">Les `FromSqlInterpolated` méthodes `ExecuteSqlInterpolated` et autorisent l’utilisation de la syntaxe d’interpolation de chaîne d’une manière qui protège contre les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="468a5-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="468a5-116">L’exemple suivant passe un paramètre unique à une procédure stockée en incluant un espace réservé de paramètre dans la chaîne de requête SQL et en fournissant un argument supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="468a5-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="468a5-117">Bien que cela puisse être `String.Format` similaire à la syntaxe, la valeur fournie est `DbParameter` encapsulée dans un et le nom de `{0}` paramètre généré est inséré à l’endroit où l’espace réservé a été spécifié.</span><span class="sxs-lookup"><span data-stu-id="468a5-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="468a5-118">En guise d' `FromSqlRaw`alternative à, vous `FromSqlInterpolated` pouvez utiliser qui autorise l’utilisation sécurisée de l’interpolation de chaîne.</span><span class="sxs-lookup"><span data-stu-id="468a5-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="468a5-119">Comme dans l’exemple précédent, la valeur est convertie `DbParameter` en et n’est donc pas vulnérable à l’injection SQL :</span><span class="sxs-lookup"><span data-stu-id="468a5-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="468a5-120">Avant la version 3,0, `FromSqlRaw` `FromSqlInterpolated` deux surcharges étaient nommées `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="468a5-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="468a5-121">Pour plus d’informations, consultez la [section versions précédentes](#previous-versions) .</span><span class="sxs-lookup"><span data-stu-id="468a5-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="468a5-122">Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="468a5-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="468a5-123">Étant donné qu’un espace réservé de paramètre SQL standard est utilisé, plutôt qu' `FromSqlRaw` un espace réservé de chaîne, peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="468a5-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="468a5-124">De cette façon, vous pouvez utiliser des paramètres nommés dans la chaîne de requête SQL, ce qui est utile quand une procédure stockée a des paramètres facultatifs :</span><span class="sxs-lookup"><span data-stu-id="468a5-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="468a5-125">Composition avec LINQ</span><span class="sxs-lookup"><span data-stu-id="468a5-125">Composing with LINQ</span></span>

<span data-ttu-id="468a5-126">Si la requête SQL peut être composée dans la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="468a5-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="468a5-127">Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="468a5-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="468a5-128">L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et compose dessus à l’aide de LINQ pour effectuer le filtrage et le tri.</span><span class="sxs-lookup"><span data-stu-id="468a5-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="468a5-129">Cette opération génère la requête SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="468a5-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="468a5-130">Suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="468a5-130">Change Tracking</span></span>

<span data-ttu-id="468a5-131">Les requêtes qui utilisent `FromSql` les méthodes suivent exactement les mêmes règles de suivi des modifications que toute autre requête LINQ dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="468a5-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="468a5-132">Par exemple, si la requête projette des types d’entités, les résultats sont suivis par défaut.</span><span class="sxs-lookup"><span data-stu-id="468a5-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="468a5-133">L’exemple suivant utilise une requête SQL brute qui effectue une sélection à partir d’une fonction table (TVF), puis désactive le suivi des modifications avec l' `AsNoTracking`appel à :</span><span class="sxs-lookup"><span data-stu-id="468a5-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="468a5-134">Inclusion de données associées</span><span class="sxs-lookup"><span data-stu-id="468a5-134">Including related data</span></span>

<span data-ttu-id="468a5-135">La méthode `Include` peut être utilisée pour inclure des données associées, comme avec toute autre requête LINQ :</span><span class="sxs-lookup"><span data-stu-id="468a5-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="468a5-136">Notez que cela nécessite que votre requête SQL brute soit composable ; en particulier, il ne fonctionne pas avec les appels de procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="468a5-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="468a5-137">Consultez les remarques sur la composition sous [limitations](#limitations)).</span><span class="sxs-lookup"><span data-stu-id="468a5-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="468a5-138">Limitations</span><span class="sxs-lookup"><span data-stu-id="468a5-138">Limitations</span></span>

<span data-ttu-id="468a5-139">Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :</span><span class="sxs-lookup"><span data-stu-id="468a5-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="468a5-140">La requête SQL doit retourner des données pour toutes les propriétés du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="468a5-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="468a5-141">Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="468a5-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="468a5-142">Notez que cela est différent à compter d’EF6, où le mappage de propriétés/colonnes était ignoré pour les requêtes SQL brutes et où les noms de colonne du jeu de résultats devaient correspondre aux noms des propriétés.</span><span class="sxs-lookup"><span data-stu-id="468a5-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="468a5-143">La requête SQL ne peut pas contenir de données associées.</span><span class="sxs-lookup"><span data-stu-id="468a5-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="468a5-144">Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="468a5-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="468a5-145">Les instructions `SELECT` passées à cette méthode doivent généralement être composables : Si EF Core doit évaluer des opérateurs de requête supplémentaires sur le serveur (par exemple, pour traduire des opérateurs LINQ `FromSql` appliqués après les méthodes), le SQL fourni est traité comme une sous-requête.</span><span class="sxs-lookup"><span data-stu-id="468a5-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="468a5-146">Cela signifie que l’instruction SQL passée ne doit pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-requête, comme :</span><span class="sxs-lookup"><span data-stu-id="468a5-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="468a5-147">point-virgule de fin</span><span class="sxs-lookup"><span data-stu-id="468a5-147">A trailing semicolon</span></span>
  * <span data-ttu-id="468a5-148">Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="468a5-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="468a5-149">Sur le serveur SQL Server, une clause `ORDER BY` n’est pas accompagnée de `OFFSET 0` OU `TOP 100 PERCENT` dans la clause `SELECT`</span><span class="sxs-lookup"><span data-stu-id="468a5-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="468a5-150">Notez que SQL Server n’autorise pas la composition sur les appels de procédure stockée. par conséquent, toute tentative d’appliquer des opérateurs de requête supplémentaires à un tel appel entraîne l’invalidité de SQL.</span><span class="sxs-lookup"><span data-stu-id="468a5-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="468a5-151">Les opérateurs de requête peuvent être `AsEnumerable()` introduits après pour l’évaluation du client.</span><span class="sxs-lookup"><span data-stu-id="468a5-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="468a5-152">Versions antérieures</span><span class="sxs-lookup"><span data-stu-id="468a5-152">Previous versions</span></span>

<span data-ttu-id="468a5-153">EF Core version 2,2 et les versions antérieures comportaient deux `FromSql` surcharges nommées qui se présentaient de la `FromSqlRaw` même `FromSqlInterpolated`façon que les plus récents et.</span><span class="sxs-lookup"><span data-stu-id="468a5-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="468a5-154">Il est ainsi très facile d’appeler par erreur la méthode de chaîne brute lorsque l’intention était d’appeler la méthode de chaîne interpolée, et d’inverse.</span><span class="sxs-lookup"><span data-stu-id="468a5-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="468a5-155">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="468a5-155">This could result in queries not being parameterized when they should have been.</span></span>
