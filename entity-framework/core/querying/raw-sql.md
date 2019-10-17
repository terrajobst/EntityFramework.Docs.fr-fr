---
title: 'Requêtes SQL brutes : EF Core'
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b7087771f1a9e8ee5e044cfea367d74a0b1c1d35
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445919"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="0ba15-102">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="0ba15-102">Raw SQL Queries</span></span>

<span data-ttu-id="0ba15-103">Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="0ba15-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="0ba15-104">Les requêtes SQL brutes sont utiles si la requête que vous souhaitez ne peut pas être exprimée à l’aide de LINQ.</span><span class="sxs-lookup"><span data-stu-id="0ba15-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="0ba15-105">Les requêtes SQL brutes sont également utilisées si une requête LINQ aboutit à une requête SQL inefficace.</span><span class="sxs-lookup"><span data-stu-id="0ba15-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="0ba15-106">Les requêtes SQL brutes peuvent retourner des types d’entité standard ou des [types d’entité sans clé](xref:core/modeling/keyless-entity-types) qui font partie de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="0ba15-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="0ba15-107">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="0ba15-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="0ba15-108">Requêtes SQL brutes de base</span><span class="sxs-lookup"><span data-stu-id="0ba15-108">Basic raw SQL queries</span></span>

<span data-ttu-id="0ba15-109">Vous pouvez utiliser la méthode d’extension `FromSqlRaw` pour commencer une requête LINQ basée sur une requête SQL brute.</span><span class="sxs-lookup"><span data-stu-id="0ba15-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="0ba15-110">`FromSqlRaw` ne peut être utilisé que sur les racines de requête, qui se trouvent directement sur la `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="0ba15-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="0ba15-111">Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="0ba15-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="0ba15-112">Passage de paramètres</span><span class="sxs-lookup"><span data-stu-id="0ba15-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="0ba15-113">**Toujours utiliser le paramétrage pour les requêtes SQL brutes**</span><span class="sxs-lookup"><span data-stu-id="0ba15-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="0ba15-114">Lorsque vous introduisez des valeurs fournies par l’utilisateur dans une requête SQL brute, vous devez veiller à éviter les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="0ba15-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="0ba15-115">En plus de valider le fait que ces valeurs ne contiennent pas de caractères non valides, utilisez toujours le paramétrage qui envoie les valeurs séparées du texte SQL.</span><span class="sxs-lookup"><span data-stu-id="0ba15-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="0ba15-116">En particulier, ne transmettez jamais une chaîne concaténée ou interpolée (`$""`) avec des valeurs non validées fournies par l’utilisateur dans `FromSqlRaw` ou `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="0ba15-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="0ba15-117">Les méthodes `FromSqlInterpolated` et `ExecuteSqlInterpolated` permettent d’utiliser la syntaxe d’interpolation de chaîne d’une manière qui protège contre les attaques par injection SQL.</span><span class="sxs-lookup"><span data-stu-id="0ba15-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="0ba15-118">L’exemple suivant passe un paramètre unique à une procédure stockée en incluant un espace réservé de paramètre dans la chaîne de requête SQL et en fournissant un argument supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="0ba15-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="0ba15-119">Alors que cette syntaxe peut ressembler à la syntaxe `String.Format`, la valeur fournie est encapsulée dans une `DbParameter` et le nom de paramètre généré est inséré à l’endroit où l’espace réservé `{0}` a été spécifié.</span><span class="sxs-lookup"><span data-stu-id="0ba15-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="0ba15-120">`FromSqlInterpolated` est semblable à `FromSqlRaw`, mais vous permet d’utiliser la syntaxe d’interpolation de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0ba15-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="0ba15-121">Tout comme `FromSqlRaw`, `FromSqlInterpolated` ne peut être utilisé que sur les racines de requête.</span><span class="sxs-lookup"><span data-stu-id="0ba15-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="0ba15-122">Comme dans l’exemple précédent, la valeur est convertie en `DbParameter` et n’est pas vulnérable à l’injection SQL.</span><span class="sxs-lookup"><span data-stu-id="0ba15-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="0ba15-123">Avant la version 3,0, les `FromSqlRaw` et `FromSqlInterpolated` étaient deux surcharges nommées `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="0ba15-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="0ba15-124">Pour plus d’informations, consultez la [section versions précédentes](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="0ba15-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="0ba15-125">Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="0ba15-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="0ba15-126">Étant donné qu’un espace réservé de paramètre SQL standard est utilisé, plutôt qu’un espace réservé de chaîne, `FromSqlRaw` peut être utilisé en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="0ba15-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="0ba15-127">`FromSqlRaw` vous permet d’utiliser des paramètres nommés dans la chaîne de requête SQL, ce qui est utile lorsqu’une procédure stockée a des paramètres facultatifs :</span><span class="sxs-lookup"><span data-stu-id="0ba15-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="0ba15-128">Composition avec LINQ</span><span class="sxs-lookup"><span data-stu-id="0ba15-128">Composing with LINQ</span></span>

<span data-ttu-id="0ba15-129">Vous pouvez composer en haut de la requête SQL brute initiale à l’aide des opérateurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="0ba15-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="0ba15-130">EF Core le traitera comme sous-requête et composera dessus dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0ba15-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="0ba15-131">L’exemple suivant utilise une requête SQL brute qui effectue une sélection à partir d’une fonction table (TVF).</span><span class="sxs-lookup"><span data-stu-id="0ba15-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="0ba15-132">Puis le compose à l’aide de LINQ pour effectuer un filtrage et un tri.</span><span class="sxs-lookup"><span data-stu-id="0ba15-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="0ba15-133">La requête ci-dessus génère le code SQL suivant :</span><span class="sxs-lookup"><span data-stu-id="0ba15-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="0ba15-134">Inclusion de données associées</span><span class="sxs-lookup"><span data-stu-id="0ba15-134">Including related data</span></span>

<span data-ttu-id="0ba15-135">La méthode `Include` peut être utilisée pour inclure des données associées, comme avec toute autre requête LINQ :</span><span class="sxs-lookup"><span data-stu-id="0ba15-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="0ba15-136">La composition avec LINQ nécessite que votre requête SQL brute soit composable, car EF Core traite le SQL fourni comme sous-requête.</span><span class="sxs-lookup"><span data-stu-id="0ba15-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="0ba15-137">Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="0ba15-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="0ba15-138">En outre, SQL passé ne doit pas contenir de caractères ou d’options qui ne sont pas valides dans une sous-requête, par exemple :</span><span class="sxs-lookup"><span data-stu-id="0ba15-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="0ba15-139">Point-virgule de fin</span><span class="sxs-lookup"><span data-stu-id="0ba15-139">A trailing semicolon</span></span>
- <span data-ttu-id="0ba15-140">Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="0ba15-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="0ba15-141">Sur SQL Server, une clause `ORDER BY` qui n’est pas utilisée avec `OFFSET 0` ou `TOP 100 PERCENT` dans la clause `SELECT`</span><span class="sxs-lookup"><span data-stu-id="0ba15-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="0ba15-142">SQL Server n’autorise pas la composition sur les appels de procédure stockée, toute tentative d’appliquer des opérateurs de requête supplémentaires à un tel appel aura pour résultat un SQL non valide.</span><span class="sxs-lookup"><span data-stu-id="0ba15-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="0ba15-143">Utilisez la méthode `AsEnumerable` ou `AsAsyncEnumerable` juste après les méthodes `FromSqlRaw` ou `FromSqlInterpolated` pour vous assurer que EF Core n’essaie pas de composer une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="0ba15-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="0ba15-144">Suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="0ba15-144">Change Tracking</span></span>

<span data-ttu-id="0ba15-145">Les requêtes qui utilisent les méthodes `FromSqlRaw` ou `FromSqlInterpolated` suivent exactement les mêmes règles de suivi des modifications que toute autre requête LINQ dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="0ba15-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="0ba15-146">Par exemple, si la requête projette des types d’entités, les résultats sont suivis par défaut.</span><span class="sxs-lookup"><span data-stu-id="0ba15-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="0ba15-147">L’exemple suivant utilise une requête SQL brute qui effectue une sélection à partir d’une fonction table (TVF), puis désactive le suivi des modifications avec l’appel à `AsNoTracking` :</span><span class="sxs-lookup"><span data-stu-id="0ba15-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="0ba15-148">Limitations</span><span class="sxs-lookup"><span data-stu-id="0ba15-148">Limitations</span></span>

<span data-ttu-id="0ba15-149">Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :</span><span class="sxs-lookup"><span data-stu-id="0ba15-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="0ba15-150">La requête SQL doit retourner des données pour toutes les propriétés du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="0ba15-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="0ba15-151">Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="0ba15-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="0ba15-152">Notez que ce comportement est différent de EF6.</span><span class="sxs-lookup"><span data-stu-id="0ba15-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="0ba15-153">EF6 ignore la propriété pour le mappage de colonnes pour les requêtes SQL brutes et les noms de colonnes du jeu de résultats devaient correspondre aux noms de propriété.</span><span class="sxs-lookup"><span data-stu-id="0ba15-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="0ba15-154">La requête SQL ne peut pas contenir de données associées.</span><span class="sxs-lookup"><span data-stu-id="0ba15-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="0ba15-155">Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="0ba15-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="0ba15-156">Versions antérieures</span><span class="sxs-lookup"><span data-stu-id="0ba15-156">Previous versions</span></span>

<span data-ttu-id="0ba15-157">EF Core version 2,2 et les versions antérieures comportaient deux surcharges de méthode nommées `FromSql`, qui se présentaient de la même façon que la plus récente `FromSqlRaw` et `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="0ba15-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="0ba15-158">Il était facile d’appeler par erreur la méthode de chaîne brute lorsque l’intention était d’appeler la méthode de chaîne interpolée, et d’inverse.</span><span class="sxs-lookup"><span data-stu-id="0ba15-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="0ba15-159">L’appel accidentel d’une surcharge incorrecte peut entraîner des requêtes qui ne sont pas paramétrables quand elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="0ba15-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
