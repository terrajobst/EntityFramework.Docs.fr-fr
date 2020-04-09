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
# <a name="raw-sql-queries"></a>Requêtes SQL brutes

Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle. Les requêtes SQL brutes sont utiles si la requête que vous voulez ne peut pas être exprimée à l’aide de LINQ. Des requêtes SQL brutes sont également utilisées si l’utilisation d’une requête LINQ entraîne une requête SQL inefficace. Les requêtes SQL brutes peuvent retourner les types d’entités régulières ou [les types d’entités sans clé](xref:core/modeling/keyless-entity-types) qui font partie de votre modèle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) sur GitHub.

## <a name="basic-raw-sql-queries"></a>Requêtes SQL brutes de base

Vous pouvez `FromSqlRaw` utiliser la méthode d’extension pour commencer une requête LINQ basée sur une requête SQL brute. `FromSqlRaw`ne peut être utilisé que sur les `DbSet<>`racines de requête, qui est directement sur le .

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Transmission des paramètres

> [!WARNING]
> **Toujours utiliser la paramétrisation pour les requêtes SQL brutes**
>
> Lors de l’introduction de valeurs fournies par l’utilisateur dans une requête SQL brute, il faut faire attention pour éviter les attaques d’injection SQL. En plus de valider que ces valeurs ne contiennent pas de caractères invalides, utilisez toujours la paramétrisation qui envoie les valeurs séparées du texte SQL.
>
> En particulier, ne jamais passer une chaîne concatenated ou interpolée (`$""` `FromSqlRaw` ) `ExecuteSqlRaw`avec des valeurs non validées fournies par l’utilisateur dans ou . Les `FromSqlInterpolated` `ExecuteSqlInterpolated` méthodes et les méthodes permettent d’utiliser la syntaxe d’interpolation des cordes d’une manière qui protège contre les attaques d’injection SQL.

L’exemple suivant passe un paramètre unique à une procédure stockée en incluant un lieu de passage dans la chaîne de requête SQL et en fournissant un argument supplémentaire. Bien que cette syntaxe puisse ressembler `String.Format` à une `DbParameter` syntaxe, la valeur `{0}` fournie est enveloppée dans un nom de paramètre généré inséré là où le propriétaire de place a été spécifié.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`est similaire `FromSqlRaw` à mais vous permet d’utiliser la syntaxe d’interpolation des chaînes. Tout `FromSqlRaw`comme, `FromSqlInterpolated` ne peut être utilisé sur les racines de requête. Comme dans l’exemple précédent, la `DbParameter` valeur est convertie en un et n’est pas vulnérable à l’injection SQL.

> [!NOTE]
> Avant la version 3.0, `FromSqlRaw` et `FromSqlInterpolated` étaient `FromSql`deux surcharges nommées . Pour plus d’informations, voir la [section versions précédentes](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre. Étant donné qu’un détenteur régulier de paramètres SQL est utilisé, plutôt qu’un espace de réunion, `FromSqlRaw` peut être utilisé en toute sécurité :

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`vous permet d’utiliser des paramètres nommés dans la chaîne de requête SQL, ce qui est utile lorsqu’une procédure stockée a des paramètres facultatifs :

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Composition avec LINQ

Vous pouvez composer en plus de la requête SQL brute initiale en utilisant des opérateurs LINQ. EF Core le traitera comme une sous-fertilité et composera dessus dans la base de données. L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction de table(TVF). Et puis compose sur elle en utilisant LINQ pour faire le filtrage et le tri.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Ci-dessus requête génère suite SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Inclusion de données associées

La méthode `Include` peut être utilisée pour inclure des données associées, comme avec toute autre requête LINQ :

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

Composer avec LINQ exige que votre requête SQL brute soit composable puisque EF Core traitera la SQL fournie comme une sous-fertilité. Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`. De plus, SQL passé ne devrait pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-virerie, tels que:

- Un point-virgule à la traîne
- Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)
- Sur SQL `ORDER BY` Server, une clause qui `OFFSET 0` `TOP 100 PERCENT` n’est pas utilisée avec LA OU dans la `SELECT` clause

SQL Server n’autorise pas la composition sur les appels de procédure stockés, de sorte que toute tentative d’appliquer des opérateurs de requête supplémentaires à un tel appel se traduira par SQL invalide. Utiliser `AsEnumerable` `AsAsyncEnumerable` ou méthode `FromSqlRaw` `FromSqlInterpolated` juste après ou des méthodes pour s’assurer que EF Core n’essaie pas de composer sur une procédure stockée.

## <a name="change-tracking"></a>Suivi des modifications

Les requêtes `FromSqlRaw` qui `FromSqlInterpolated` utilisent le ou les méthodes suivent exactement les mêmes règles de suivi de changement que n’importe quelle autre requête LINQ dans EF Core. Par exemple, si la requête projette des types d’entités, les résultats sont suivis par défaut.

L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction de `AsNoTracking`table (TVF), puis désactive le suivi de changement avec l’appel à :

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Limites

Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :

- La requête SQL doit retourner les données pour toutes les propriétés du type d’entité.
- Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés. Notez que ce comportement est différent de EF6. EF6 a ignoré la propriété à la cartographie de colonne pour les requêtes brutes SQL et les noms de colonnes de jeu de résultat ont dû correspondre aux noms de propriété.
- La requête SQL ne peut pas contenir de données connexes. Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).

## <a name="previous-versions"></a>Versions antérieures

EF Core version 2.2 et plus tôt `FromSql`avait deux surcharges de méthode nommées , qui se sont comportés de la même manière que le plus récent `FromSqlRaw` et `FromSqlInterpolated`. Il était facile d’appeler accidentellement la méthode de la chaîne brute lorsque l’intention était d’appeler la méthode de la chaîne interpolée, et l’inverse. Appeler une surcharge incorrecte accidentellement pourrait entraîner des requêtes ne pas être paramétrées alors qu’elles auraient dû l’être.
