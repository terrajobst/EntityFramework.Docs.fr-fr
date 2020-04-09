---
title: Opérateurs de requêtes complexes - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417742"
---
# <a name="complex-query-operators"></a><span data-ttu-id="59031-102">Opérateurs de requête complexes</span><span class="sxs-lookup"><span data-stu-id="59031-102">Complex Query Operators</span></span>

<span data-ttu-id="59031-103">Language Integrated Query (LINQ) contient de nombreux opérateurs complexes, qui combinent plusieurs sources de données ou effectuent un traitement complexe.</span><span class="sxs-lookup"><span data-stu-id="59031-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="59031-104">Tous les opérateurs de LINQ n’ont pas de traductions appropriées du côté du serveur.</span><span class="sxs-lookup"><span data-stu-id="59031-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="59031-105">Parfois, une requête sous une forme se traduit sur le serveur, mais si elle est écrite sous une forme différente ne se traduit pas même si le résultat est le même.</span><span class="sxs-lookup"><span data-stu-id="59031-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="59031-106">Cette page décrit certains des opérateurs complexes et leurs variations prises en charge.</span><span class="sxs-lookup"><span data-stu-id="59031-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="59031-107">Dans les versions futures, nous pouvons reconnaître plus de modèles et ajouter leurs traductions correspondantes.</span><span class="sxs-lookup"><span data-stu-id="59031-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="59031-108">Il est également important de garder à l’esprit que le support de traduction varie d’un fournisseur à l’autre.</span><span class="sxs-lookup"><span data-stu-id="59031-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="59031-109">Une requête particulière, qui est traduite dans SqlServer, peut ne pas fonctionner pour les bases de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="59031-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="59031-110">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="59031-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="59031-111">Join</span><span class="sxs-lookup"><span data-stu-id="59031-111">Join</span></span>

<span data-ttu-id="59031-112">L’opérateur LINQ Join vous permet de connecter deux sources de données en fonction du sélecteur clé pour chaque source, générant un réglage de valeurs lorsque la clé correspond.</span><span class="sxs-lookup"><span data-stu-id="59031-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="59031-113">Il se traduit `INNER JOIN` naturellement par des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="59031-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="59031-114">Bien que la jointure LINQ dispose de sélecteurs de clés externes et internes, la base de données nécessite une seule condition de jointure.</span><span class="sxs-lookup"><span data-stu-id="59031-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="59031-115">Ef Core génère donc une condition de jointure en comparant le sélecteur de clé externe au sélecteur de clé interne pour l’égalité.</span><span class="sxs-lookup"><span data-stu-id="59031-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="59031-116">En outre, si les sélecteurs clés sont des types anonymes, EF Core génère une condition de jointure pour comparer la composante d’égalité sage.</span><span class="sxs-lookup"><span data-stu-id="59031-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="59031-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="59031-117">GroupJoin</span></span>

<span data-ttu-id="59031-118">L’opérateur LINQ GroupJoin vous permet de connecter deux sources de données similaires à Join, mais il crée un groupe de valeurs intérieures pour faire correspondre les éléments extérieurs.</span><span class="sxs-lookup"><span data-stu-id="59031-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="59031-119">L’exécution d’une requête comme l’exemple suivant génère un résultat de `Blog`  &  `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="59031-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="59031-120">Étant donné que les bases de données (en particulier les bases de données relationnelles) n’ont pas de moyen de représenter une collection d’objets côté client, GroupJoin ne se traduit pas sur le serveur dans de nombreux cas.</span><span class="sxs-lookup"><span data-stu-id="59031-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="59031-121">Il vous oblige à obtenir toutes les données du serveur pour faire GroupJoin sans un sélecteur spécial (première requête ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="59031-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="59031-122">Mais si le sélecteur limite les données sélectionnées, puis récupérer toutes les données du serveur peut causer des problèmes de performances (deuxième requête ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="59031-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="59031-123">C’est pourquoi EF Core ne traduit pas GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="59031-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="59031-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="59031-124">SelectMany</span></span>

<span data-ttu-id="59031-125">L’opérateur LINQ SelectMany vous permet d’énumérer un sélecteur de collection pour chaque élément externe et de générer des réglages de valeurs de chaque source de données.</span><span class="sxs-lookup"><span data-stu-id="59031-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="59031-126">D’une certaine manière, c’est une jointure mais sans aucune condition de sorte que chaque élément extérieur est connecté à un élément de la source de collecte.</span><span class="sxs-lookup"><span data-stu-id="59031-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="59031-127">Selon la façon dont le sélecteur de collection est lié à la source de données externe, SelectMany peut se traduire par différentes requêtes du côté serveur.</span><span class="sxs-lookup"><span data-stu-id="59031-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="59031-128">Le sélecteur de collection ne fait pas référence à l’extérieur</span><span class="sxs-lookup"><span data-stu-id="59031-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="59031-129">Lorsque le sélecteur de collection ne fait référence à rien de la source extérieure, le résultat est un produit cartésien des deux sources de données.</span><span class="sxs-lookup"><span data-stu-id="59031-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="59031-130">Il se `CROSS JOIN` traduit par des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="59031-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="59031-131">Le sélecteur de collection fait référence à l’extérieur dans une clause où</span><span class="sxs-lookup"><span data-stu-id="59031-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="59031-132">Lorsque le sélecteur de collection a une clause où, qui fait référence à l’élément externe, puis EF Core le traduit en une base de données joindre et utilise le prédicat comme condition de jointure.</span><span class="sxs-lookup"><span data-stu-id="59031-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="59031-133">Normalement, ce cas se pose lors de l’utilisation de la navigation de collecte sur l’élément externe comme sélecteur de collection.</span><span class="sxs-lookup"><span data-stu-id="59031-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="59031-134">Si la collecte est vide pour un élément externe, aucun résultat ne serait généré pour cet élément externe.</span><span class="sxs-lookup"><span data-stu-id="59031-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="59031-135">Mais `DefaultIfEmpty` si elle est appliquée sur le sélecteur de collection, l’élément externe sera relié à une valeur par défaut de l’élément intérieur.</span><span class="sxs-lookup"><span data-stu-id="59031-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="59031-136">En raison de cette distinction, ce `INNER JOIN` genre de `DefaultIfEmpty` requêtes se traduit par l’absence et `LEFT JOIN` le moment où `DefaultIfEmpty` est appliqué.</span><span class="sxs-lookup"><span data-stu-id="59031-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="59031-137">Le sélecteur de collection fait référence à l’extérieur dans un cas non-où</span><span class="sxs-lookup"><span data-stu-id="59031-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="59031-138">Lorsque le sélecteur de collection fait référence à l’élément externe, qui n’est pas dans une clause où (comme le cas ci-dessus), il ne se traduit pas par une jointure de base de données.</span><span class="sxs-lookup"><span data-stu-id="59031-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="59031-139">C’est pourquoi nous devons évaluer le sélecteur de collection pour chaque élément externe.</span><span class="sxs-lookup"><span data-stu-id="59031-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="59031-140">Il se `APPLY` traduit par des opérations dans de nombreuses bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="59031-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="59031-141">Si la collecte est vide pour un élément externe, aucun résultat ne serait généré pour cet élément externe.</span><span class="sxs-lookup"><span data-stu-id="59031-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="59031-142">Mais `DefaultIfEmpty` si elle est appliquée sur le sélecteur de collection, l’élément externe sera relié à une valeur par défaut de l’élément intérieur.</span><span class="sxs-lookup"><span data-stu-id="59031-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="59031-143">En raison de cette distinction, ce `CROSS APPLY` genre de `DefaultIfEmpty` requêtes se traduit par l’absence et `OUTER APPLY` le moment où `DefaultIfEmpty` est appliqué.</span><span class="sxs-lookup"><span data-stu-id="59031-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="59031-144">Certaines bases de données comme `APPLY` SQLite ne prennent pas en charge les opérateurs, de sorte que ce genre de requête peut ne pas être traduit.</span><span class="sxs-lookup"><span data-stu-id="59031-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="59031-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="59031-145">GroupBy</span></span>

<span data-ttu-id="59031-146">LINQ GroupBy opérateurs créent `IGrouping<TKey, TElement>` un `TKey` `TElement` résultat de type où et pourrait être n’importe quel type arbitraire.</span><span class="sxs-lookup"><span data-stu-id="59031-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="59031-147">En `IGrouping` outre, `IEnumerable<TElement>`les implémentations , ce qui signifie que vous pouvez composer sur elle en utilisant n’importe quel opérateur LINQ après le regroupement.</span><span class="sxs-lookup"><span data-stu-id="59031-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="59031-148">Étant donné qu’aucune `IGrouping`structure de base de données ne peut représenter une , les opérateurs GroupBy n’ont pas de traduction dans la plupart des cas.</span><span class="sxs-lookup"><span data-stu-id="59031-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="59031-149">Lorsqu’un opérateur agrégé est appliqué à chaque groupe, qui renvoie un `GROUP BY` échaudé, il peut être traduit en SQL dans des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="59031-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="59031-150">Le SQL `GROUP BY` est également restrictif.</span><span class="sxs-lookup"><span data-stu-id="59031-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="59031-151">Il vous oblige à regrouper uniquement par des valeurs scalaires.</span><span class="sxs-lookup"><span data-stu-id="59031-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="59031-152">La projection ne peut contenir que des colonnes clés de regroupement ou tout agrégat appliqué sur une colonne.</span><span class="sxs-lookup"><span data-stu-id="59031-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="59031-153">EF Core identifie ce modèle et le traduit au serveur, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="59031-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="59031-154">EF Core traduit également les requêtes lorsqu’un opérateur agrégé sur le groupement apparaît dans un opérateur DE LINQ Where or OrderBy (ou autre commande).</span><span class="sxs-lookup"><span data-stu-id="59031-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="59031-155">Il `HAVING` utilise la clause dans SQL pour la clause où.</span><span class="sxs-lookup"><span data-stu-id="59031-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="59031-156">La partie de la requête avant d’appliquer l’opérateur GroupBy peut être n’importe quelle requête complexe tant qu’elle peut être traduite sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="59031-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="59031-157">En outre, une fois que vous appliquez des opérateurs agrégés sur une requête de groupement pour supprimer les groupes de la source résultante, vous pouvez composer sur le dessus de celui-ci comme n’importe quelle autre requête.</span><span class="sxs-lookup"><span data-stu-id="59031-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="59031-158">Les supports EF Core des opérateurs agrégés sont les suivants</span><span class="sxs-lookup"><span data-stu-id="59031-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="59031-159">Average</span><span class="sxs-lookup"><span data-stu-id="59031-159">Average</span></span>
- <span data-ttu-id="59031-160">Count</span><span class="sxs-lookup"><span data-stu-id="59031-160">Count</span></span>
- <span data-ttu-id="59031-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="59031-161">LongCount</span></span>
- <span data-ttu-id="59031-162">Max</span><span class="sxs-lookup"><span data-stu-id="59031-162">Max</span></span>
- <span data-ttu-id="59031-163">Min</span><span class="sxs-lookup"><span data-stu-id="59031-163">Min</span></span>
- <span data-ttu-id="59031-164">SUM</span><span class="sxs-lookup"><span data-stu-id="59031-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="59031-165">Rejoindre à gauche</span><span class="sxs-lookup"><span data-stu-id="59031-165">Left Join</span></span>

<span data-ttu-id="59031-166">Bien que Left Join ne soit pas un opérateur de LINQ, les bases de données relationnelles ont le concept d’une jointure de gauche qui est fréquemment utilisée dans les requêtes.</span><span class="sxs-lookup"><span data-stu-id="59031-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="59031-167">Un modèle particulier dans les requêtes LINQ donne le même résultat qu’un `LEFT JOIN` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="59031-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="59031-168">EF Core identifie ces modèles et `LEFT JOIN` génère l’équivalent du côté serveur.</span><span class="sxs-lookup"><span data-stu-id="59031-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="59031-169">Le modèle consiste à créer un GroupJoin entre les deux sources de données, puis aplatir le groupement en utilisant l’opérateur SelectMany avec DefaultIfEmpty sur la source de groupement pour correspondre nul lorsque l’intérieur n’a pas d’élément connexe.</span><span class="sxs-lookup"><span data-stu-id="59031-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="59031-170">L’exemple suivant montre à quoi ressemble ce modèle et à quoi il génère.</span><span class="sxs-lookup"><span data-stu-id="59031-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="59031-171">Le motif ci-dessus crée une structure complexe dans l’arbre d’expression.</span><span class="sxs-lookup"><span data-stu-id="59031-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="59031-172">Pour cette raison, EF Core vous oblige à aplatir les résultats de regroupement de l’opérateur GroupJoin dans une étape immédiatement après l’opérateur.</span><span class="sxs-lookup"><span data-stu-id="59031-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="59031-173">Même si le GroupJoin-DefaultIfEmpty-SelectMany est utilisé mais dans un modèle différent, nous ne pouvons pas l’identifier comme une adhésion de gauche.</span><span class="sxs-lookup"><span data-stu-id="59031-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
