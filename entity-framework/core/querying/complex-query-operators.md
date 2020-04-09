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
# <a name="complex-query-operators"></a>Opérateurs de requête complexes

Language Integrated Query (LINQ) contient de nombreux opérateurs complexes, qui combinent plusieurs sources de données ou effectuent un traitement complexe. Tous les opérateurs de LINQ n’ont pas de traductions appropriées du côté du serveur. Parfois, une requête sous une forme se traduit sur le serveur, mais si elle est écrite sous une forme différente ne se traduit pas même si le résultat est le même. Cette page décrit certains des opérateurs complexes et leurs variations prises en charge. Dans les versions futures, nous pouvons reconnaître plus de modèles et ajouter leurs traductions correspondantes. Il est également important de garder à l’esprit que le support de traduction varie d’un fournisseur à l’autre. Une requête particulière, qui est traduite dans SqlServer, peut ne pas fonctionner pour les bases de données SQLite.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="join"></a>Join

L’opérateur LINQ Join vous permet de connecter deux sources de données en fonction du sélecteur clé pour chaque source, générant un réglage de valeurs lorsque la clé correspond. Il se traduit `INNER JOIN` naturellement par des bases de données relationnelles. Bien que la jointure LINQ dispose de sélecteurs de clés externes et internes, la base de données nécessite une seule condition de jointure. Ef Core génère donc une condition de jointure en comparant le sélecteur de clé externe au sélecteur de clé interne pour l’égalité. En outre, si les sélecteurs clés sont des types anonymes, EF Core génère une condition de jointure pour comparer la composante d’égalité sage.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

L’opérateur LINQ GroupJoin vous permet de connecter deux sources de données similaires à Join, mais il crée un groupe de valeurs intérieures pour faire correspondre les éléments extérieurs. L’exécution d’une requête comme l’exemple suivant génère un résultat de `Blog`  &  `IEnumerable<Post>`. Étant donné que les bases de données (en particulier les bases de données relationnelles) n’ont pas de moyen de représenter une collection d’objets côté client, GroupJoin ne se traduit pas sur le serveur dans de nombreux cas. Il vous oblige à obtenir toutes les données du serveur pour faire GroupJoin sans un sélecteur spécial (première requête ci-dessous). Mais si le sélecteur limite les données sélectionnées, puis récupérer toutes les données du serveur peut causer des problèmes de performances (deuxième requête ci-dessous). C’est pourquoi EF Core ne traduit pas GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

L’opérateur LINQ SelectMany vous permet d’énumérer un sélecteur de collection pour chaque élément externe et de générer des réglages de valeurs de chaque source de données. D’une certaine manière, c’est une jointure mais sans aucune condition de sorte que chaque élément extérieur est connecté à un élément de la source de collecte. Selon la façon dont le sélecteur de collection est lié à la source de données externe, SelectMany peut se traduire par différentes requêtes du côté serveur.

### <a name="collection-selector-doesnt-reference-outer"></a>Le sélecteur de collection ne fait pas référence à l’extérieur

Lorsque le sélecteur de collection ne fait référence à rien de la source extérieure, le résultat est un produit cartésien des deux sources de données. Il se `CROSS JOIN` traduit par des bases de données relationnelles.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Le sélecteur de collection fait référence à l’extérieur dans une clause où

Lorsque le sélecteur de collection a une clause où, qui fait référence à l’élément externe, puis EF Core le traduit en une base de données joindre et utilise le prédicat comme condition de jointure. Normalement, ce cas se pose lors de l’utilisation de la navigation de collecte sur l’élément externe comme sélecteur de collection. Si la collecte est vide pour un élément externe, aucun résultat ne serait généré pour cet élément externe. Mais `DefaultIfEmpty` si elle est appliquée sur le sélecteur de collection, l’élément externe sera relié à une valeur par défaut de l’élément intérieur. En raison de cette distinction, ce `INNER JOIN` genre de `DefaultIfEmpty` requêtes se traduit par l’absence et `LEFT JOIN` le moment où `DefaultIfEmpty` est appliqué.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Le sélecteur de collection fait référence à l’extérieur dans un cas non-où

Lorsque le sélecteur de collection fait référence à l’élément externe, qui n’est pas dans une clause où (comme le cas ci-dessus), il ne se traduit pas par une jointure de base de données. C’est pourquoi nous devons évaluer le sélecteur de collection pour chaque élément externe. Il se `APPLY` traduit par des opérations dans de nombreuses bases de données relationnelles. Si la collecte est vide pour un élément externe, aucun résultat ne serait généré pour cet élément externe. Mais `DefaultIfEmpty` si elle est appliquée sur le sélecteur de collection, l’élément externe sera relié à une valeur par défaut de l’élément intérieur. En raison de cette distinction, ce `CROSS APPLY` genre de `DefaultIfEmpty` requêtes se traduit par l’absence et `OUTER APPLY` le moment où `DefaultIfEmpty` est appliqué. Certaines bases de données comme `APPLY` SQLite ne prennent pas en charge les opérateurs, de sorte que ce genre de requête peut ne pas être traduit.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

LINQ GroupBy opérateurs créent `IGrouping<TKey, TElement>` un `TKey` `TElement` résultat de type où et pourrait être n’importe quel type arbitraire. En `IGrouping` outre, `IEnumerable<TElement>`les implémentations , ce qui signifie que vous pouvez composer sur elle en utilisant n’importe quel opérateur LINQ après le regroupement. Étant donné qu’aucune `IGrouping`structure de base de données ne peut représenter une , les opérateurs GroupBy n’ont pas de traduction dans la plupart des cas. Lorsqu’un opérateur agrégé est appliqué à chaque groupe, qui renvoie un `GROUP BY` échaudé, il peut être traduit en SQL dans des bases de données relationnelles. Le SQL `GROUP BY` est également restrictif. Il vous oblige à regrouper uniquement par des valeurs scalaires. La projection ne peut contenir que des colonnes clés de regroupement ou tout agrégat appliqué sur une colonne. EF Core identifie ce modèle et le traduit au serveur, comme dans l’exemple suivant :

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core traduit également les requêtes lorsqu’un opérateur agrégé sur le groupement apparaît dans un opérateur DE LINQ Where or OrderBy (ou autre commande). Il `HAVING` utilise la clause dans SQL pour la clause où. La partie de la requête avant d’appliquer l’opérateur GroupBy peut être n’importe quelle requête complexe tant qu’elle peut être traduite sur le serveur. En outre, une fois que vous appliquez des opérateurs agrégés sur une requête de groupement pour supprimer les groupes de la source résultante, vous pouvez composer sur le dessus de celui-ci comme n’importe quelle autre requête.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Les supports EF Core des opérateurs agrégés sont les suivants

- Average
- Count
- LongCount
- Max
- Min
- SUM

## <a name="left-join"></a>Rejoindre à gauche

Bien que Left Join ne soit pas un opérateur de LINQ, les bases de données relationnelles ont le concept d’une jointure de gauche qui est fréquemment utilisée dans les requêtes. Un modèle particulier dans les requêtes LINQ donne le même résultat qu’un `LEFT JOIN` sur le serveur. EF Core identifie ces modèles et `LEFT JOIN` génère l’équivalent du côté serveur. Le modèle consiste à créer un GroupJoin entre les deux sources de données, puis aplatir le groupement en utilisant l’opérateur SelectMany avec DefaultIfEmpty sur la source de groupement pour correspondre nul lorsque l’intérieur n’a pas d’élément connexe. L’exemple suivant montre à quoi ressemble ce modèle et à quoi il génère.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Le motif ci-dessus crée une structure complexe dans l’arbre d’expression. Pour cette raison, EF Core vous oblige à aplatir les résultats de regroupement de l’opérateur GroupJoin dans une étape immédiatement après l’opérateur. Même si le GroupJoin-DefaultIfEmpty-SelectMany est utilisé mais dans un modèle différent, nous ne pouvons pas l’identifier comme une adhésion de gauche.
