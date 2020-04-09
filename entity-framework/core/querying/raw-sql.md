---
title: 'Requêtes SQL brutes : EF Core'
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417669"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="70f3a-102">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="70f3a-102">Raw SQL Queries</span></span>

<span data-ttu-id="70f3a-103">Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="70f3a-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="70f3a-104">Les requêtes SQL brutes sont utiles si la requête que vous voulez ne peut pas être exprimée à l’aide de LINQ.</span><span class="sxs-lookup"><span data-stu-id="70f3a-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="70f3a-105">Des requêtes SQL brutes sont également utilisées si l’utilisation d’une requête LINQ entraîne une requête SQL inefficace.</span><span class="sxs-lookup"><span data-stu-id="70f3a-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="70f3a-106">Les requêtes SQL brutes peuvent retourner les types d’entités régulières ou [les types d’entités sans clé](xref:core/modeling/keyless-entity-types) qui font partie de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="70f3a-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="70f3a-107">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="70f3a-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="70f3a-108">Requêtes SQL brutes de base</span><span class="sxs-lookup"><span data-stu-id="70f3a-108">Basic raw SQL queries</span></span>

<span data-ttu-id="70f3a-109">Vous pouvez `FromSqlRaw` utiliser la méthode d’extension pour commencer une requête LINQ basée sur une requête SQL brute.</span><span class="sxs-lookup"><span data-stu-id="70f3a-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="70f3a-110">`FromSqlRaw`ne peut être utilisé que sur les `DbSet<>`racines de requête, qui est directement sur le .</span><span class="sxs-lookup"><span data-stu-id="70f3a-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="70f3a-111">Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="70f3a-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="70f3a-112">Transmission des paramètres</span><span class="sxs-lookup"><span data-stu-id="70f3a-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="70f3a-113">**Toujours utiliser la paramétrisation pour les requêtes SQL brutes**</span><span class="sxs-lookup"><span data-stu-id="70f3a-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="70f3a-114">Lors de l’introduction de valeurs fournies par l’utilisateur dans une requête SQL brute, il faut faire attention pour éviter les attaques d’injection SQL.</span><span class="sxs-lookup"><span data-stu-id="70f3a-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="70f3a-115">En plus de valider que ces valeurs ne contiennent pas de caractères invalides, utilisez toujours la paramétrisation qui envoie les valeurs séparées du texte SQL.</span><span class="sxs-lookup"><span data-stu-id="70f3a-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="70f3a-116">En particulier, ne jamais passer une chaîne concatenated ou interpolée (`$""` `FromSqlRaw` ) `ExecuteSqlRaw`avec des valeurs non validées fournies par l’utilisateur dans ou .</span><span class="sxs-lookup"><span data-stu-id="70f3a-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="70f3a-117">Les `FromSqlInterpolated` `ExecuteSqlInterpolated` méthodes et les méthodes permettent d’utiliser la syntaxe d’interpolation des cordes d’une manière qui protège contre les attaques d’injection SQL.</span><span class="sxs-lookup"><span data-stu-id="70f3a-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="70f3a-118">L’exemple suivant passe un paramètre unique à une procédure stockée en incluant un lieu de passage dans la chaîne de requête SQL et en fournissant un argument supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="70f3a-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="70f3a-119">Bien que cette syntaxe puisse ressembler `String.Format` à une `DbParameter` syntaxe, la valeur `{0}` fournie est enveloppée dans un nom de paramètre généré inséré là où le propriétaire de place a été spécifié.</span><span class="sxs-lookup"><span data-stu-id="70f3a-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="70f3a-120">`FromSqlInterpolated`est similaire `FromSqlRaw` à mais vous permet d’utiliser la syntaxe d’interpolation des chaînes.</span><span class="sxs-lookup"><span data-stu-id="70f3a-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="70f3a-121">Tout `FromSqlRaw`comme, `FromSqlInterpolated` ne peut être utilisé sur les racines de requête.</span><span class="sxs-lookup"><span data-stu-id="70f3a-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="70f3a-122">Comme dans l’exemple précédent, la `DbParameter` valeur est convertie en un et n’est pas vulnérable à l’injection SQL.</span><span class="sxs-lookup"><span data-stu-id="70f3a-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="70f3a-123">Avant la version 3.0, `FromSqlRaw` et `FromSqlInterpolated` étaient `FromSql`deux surcharges nommées .</span><span class="sxs-lookup"><span data-stu-id="70f3a-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="70f3a-124">Pour plus d’informations, voir la [section versions précédentes](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="70f3a-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="70f3a-125">Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="70f3a-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="70f3a-126">Étant donné qu’un détenteur régulier de paramètres SQL est utilisé, plutôt qu’un espace de réunion, `FromSqlRaw` peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="70f3a-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="70f3a-127">`FromSqlRaw`vous permet d’utiliser des paramètres nommés dans la chaîne de requête SQL, ce qui est utile lorsqu’une procédure stockée a des paramètres facultatifs :</span><span class="sxs-lookup"><span data-stu-id="70f3a-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="70f3a-128">Composition avec LINQ</span><span class="sxs-lookup"><span data-stu-id="70f3a-128">Composing with LINQ</span></span>

<span data-ttu-id="70f3a-129">Vous pouvez composer en plus de la requête SQL brute initiale en utilisant des opérateurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="70f3a-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="70f3a-130">EF Core le traitera comme une sous-fertilité et composera dessus dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="70f3a-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="70f3a-131">L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction de table(TVF).</span><span class="sxs-lookup"><span data-stu-id="70f3a-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="70f3a-132">Et puis compose sur elle en utilisant LINQ pour faire le filtrage et le tri.</span><span class="sxs-lookup"><span data-stu-id="70f3a-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="70f3a-133">Ci-dessus requête génère suite SQL:</span><span class="sxs-lookup"><span data-stu-id="70f3a-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="70f3a-134">Inclusion de données associées</span><span class="sxs-lookup"><span data-stu-id="70f3a-134">Including related data</span></span>

<span data-ttu-id="70f3a-135">La méthode `Include` peut être utilisée pour inclure des données associées, comme avec toute autre requête LINQ :</span><span class="sxs-lookup"><span data-stu-id="70f3a-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="70f3a-136">Composer avec LINQ exige que votre requête SQL brute soit composable puisque EF Core traitera la SQL fournie comme une sous-fertilité.</span><span class="sxs-lookup"><span data-stu-id="70f3a-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="70f3a-137">Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="70f3a-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="70f3a-138">De plus, SQL passé ne devrait pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-virerie, tels que:</span><span class="sxs-lookup"><span data-stu-id="70f3a-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="70f3a-139">Un point-virgule à la traîne</span><span class="sxs-lookup"><span data-stu-id="70f3a-139">A trailing semicolon</span></span>
- <span data-ttu-id="70f3a-140">Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="70f3a-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="70f3a-141">Sur SQL `ORDER BY` Server, une clause qui `OFFSET 0` `TOP 100 PERCENT` n’est pas utilisée avec LA OU dans la `SELECT` clause</span><span class="sxs-lookup"><span data-stu-id="70f3a-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="70f3a-142">SQL Server n’autorise pas la composition sur les appels de procédure stockés, de sorte que toute tentative d’appliquer des opérateurs de requête supplémentaires à un tel appel se traduira par SQL invalide.</span><span class="sxs-lookup"><span data-stu-id="70f3a-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="70f3a-143">Utiliser `AsEnumerable` `AsAsyncEnumerable` ou méthode `FromSqlRaw` `FromSqlInterpolated` juste après ou des méthodes pour s’assurer que EF Core n’essaie pas de composer sur une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="70f3a-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="70f3a-144">Suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="70f3a-144">Change Tracking</span></span>

<span data-ttu-id="70f3a-145">Les requêtes `FromSqlRaw` qui `FromSqlInterpolated` utilisent le ou les méthodes suivent exactement les mêmes règles de suivi de changement que n’importe quelle autre requête LINQ dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="70f3a-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="70f3a-146">Par exemple, si la requête projette des types d’entités, les résultats sont suivis par défaut.</span><span class="sxs-lookup"><span data-stu-id="70f3a-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="70f3a-147">L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction de `AsNoTracking`table (TVF), puis désactive le suivi de changement avec l’appel à :</span><span class="sxs-lookup"><span data-stu-id="70f3a-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="70f3a-148">Limites</span><span class="sxs-lookup"><span data-stu-id="70f3a-148">Limitations</span></span>

<span data-ttu-id="70f3a-149">Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :</span><span class="sxs-lookup"><span data-stu-id="70f3a-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="70f3a-150">La requête SQL doit retourner les données pour toutes les propriétés du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="70f3a-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="70f3a-151">Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="70f3a-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="70f3a-152">Notez que ce comportement est différent de EF6.</span><span class="sxs-lookup"><span data-stu-id="70f3a-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="70f3a-153">EF6 a ignoré la propriété à la cartographie de colonne pour les requêtes brutes SQL et les noms de colonnes de jeu de résultat ont dû correspondre aux noms de propriété.</span><span class="sxs-lookup"><span data-stu-id="70f3a-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="70f3a-154">La requête SQL ne peut pas contenir de données connexes.</span><span class="sxs-lookup"><span data-stu-id="70f3a-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="70f3a-155">Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="70f3a-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="70f3a-156">Versions antérieures</span><span class="sxs-lookup"><span data-stu-id="70f3a-156">Previous versions</span></span>

<span data-ttu-id="70f3a-157">EF Core version 2.2 et plus tôt `FromSql`avait deux surcharges de méthode nommées , qui se sont comportés de la même manière que le plus récent `FromSqlRaw` et `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="70f3a-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="70f3a-158">Il était facile d’appeler accidentellement la méthode de la chaîne brute lorsque l’intention était d’appeler la méthode de la chaîne interpolée, et l’inverse.</span><span class="sxs-lookup"><span data-stu-id="70f3a-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="70f3a-159">Appeler une surcharge incorrecte accidentellement pourrait entraîner des requêtes ne pas être paramétrées alors qu’elles auraient dû l’être.</span><span class="sxs-lookup"><span data-stu-id="70f3a-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
