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
# <a name="raw-sql-queries"></a>Requêtes SQL brutes

Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle. Cela peut être utile si la requête que vous souhaitez exécuter ne peut pas être exprimée à l’aide de LINQ, ou si l’utilisation d’une requête LINQ aboutit à une requête SQL inefficace. Les requêtes SQL brutes peuvent retourner des types d’entité standard ou des [types d’entité sans clé](xref:core/modeling/keyless-entity-types) qui font partie de votre modèle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) sur GitHub.

## <a name="basic-raw-sql-queries"></a>Requêtes SQL brutes de base

Vous pouvez utiliser la `FromSqlRaw` méthode d’extension pour commencer une requête LINQ basée sur une requête SQL brute.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passage de paramètres

> [!WARNING]
> **Toujours utiliser le paramétrage pour les requêtes SQL brutes**
>
> Lorsque vous introduisez des valeurs fournies par l’utilisateur dans une requête SQL brute, vous devez veiller à éviter les attaques par injection SQL. En plus de valider le fait que ces valeurs ne contiennent pas de caractères non valides, utilisez toujours le paramétrage qui envoie les valeurs séparées du texte SQL.
>
> En particulier, ne transmettez jamais une chaîne concaténée ou interpolée (`$""`) avec des valeurs non validées fournies par l’utilisateur dans `FromSqlRaw` ou `ExecuteSqlRaw`. Les `FromSqlInterpolated` méthodes `ExecuteSqlInterpolated` et autorisent l’utilisation de la syntaxe d’interpolation de chaîne d’une manière qui protège contre les attaques par injection SQL.

L’exemple suivant passe un paramètre unique à une procédure stockée en incluant un espace réservé de paramètre dans la chaîne de requête SQL et en fournissant un argument supplémentaire. Bien que cela puisse être `String.Format` similaire à la syntaxe, la valeur fournie est `DbParameter` encapsulée dans un et le nom de `{0}` paramètre généré est inséré à l’endroit où l’espace réservé a été spécifié.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

En guise d' `FromSqlRaw`alternative à, vous `FromSqlInterpolated` pouvez utiliser qui autorise l’utilisation sécurisée de l’interpolation de chaîne. Comme dans l’exemple précédent, la valeur est convertie `DbParameter` en et n’est donc pas vulnérable à l’injection SQL :

> [!NOTE]
> Avant la version 3,0, `FromSqlRaw` `FromSqlInterpolated` deux surcharges étaient nommées `FromSql`. Pour plus d’informations, consultez la [section versions précédentes](#previous-versions) .

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre. Étant donné qu’un espace réservé de paramètre SQL standard est utilisé, plutôt qu' `FromSqlRaw` un espace réservé de chaîne, peut être utilisé en toute sécurité :

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

De cette façon, vous pouvez utiliser des paramètres nommés dans la chaîne de requête SQL, ce qui est utile quand une procédure stockée a des paramètres facultatifs :

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composition avec LINQ

Si la requête SQL peut être composée dans la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ. Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`.

L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et compose dessus à l’aide de LINQ pour effectuer le filtrage et le tri.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Cette opération génère la requête SQL suivante :

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Suivi des modifications

Les requêtes qui utilisent `FromSql` les méthodes suivent exactement les mêmes règles de suivi des modifications que toute autre requête LINQ dans EF Core. Par exemple, si la requête projette des types d’entités, les résultats sont suivis par défaut.

L’exemple suivant utilise une requête SQL brute qui effectue une sélection à partir d’une fonction table (TVF), puis désactive le suivi des modifications avec l' `AsNoTracking`appel à :

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Inclusion de données associées

La méthode `Include` peut être utilisée pour inclure des données associées, comme avec toute autre requête LINQ :

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Notez que cela nécessite que votre requête SQL brute soit composable ; en particulier, il ne fonctionne pas avec les appels de procédure stockée. Consultez les remarques sur la composition sous [limitations](#limitations)).

## <a name="limitations"></a>Limitations

Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :

* La requête SQL doit retourner des données pour toutes les propriétés du type d’entité.

* Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés. Notez que cela est différent à compter d’EF6, où le mappage de propriétés/colonnes était ignoré pour les requêtes SQL brutes et où les noms de colonne du jeu de résultats devaient correspondre aux noms des propriétés.

* La requête SQL ne peut pas contenir de données associées. Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).

* Les instructions `SELECT` passées à cette méthode doivent généralement être composables : Si EF Core doit évaluer des opérateurs de requête supplémentaires sur le serveur (par exemple, pour traduire des opérateurs LINQ `FromSql` appliqués après les méthodes), le SQL fourni est traité comme une sous-requête. Cela signifie que l’instruction SQL passée ne doit pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-requête, comme :
  * point-virgule de fin
  * Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)
  * Sur le serveur SQL Server, une clause `ORDER BY` n’est pas accompagnée de `OFFSET 0` OU `TOP 100 PERCENT` dans la clause `SELECT`

* Notez que SQL Server n’autorise pas la composition sur les appels de procédure stockée. par conséquent, toute tentative d’appliquer des opérateurs de requête supplémentaires à un tel appel entraîne l’invalidité de SQL. Les opérateurs de requête peuvent être `AsEnumerable()` introduits après pour l’évaluation du client.

## <a name="previous-versions"></a>Versions antérieures

EF Core version 2,2 et les versions antérieures comportaient deux `FromSql` surcharges nommées qui se présentaient de la `FromSqlRaw` même `FromSqlInterpolated`façon que les plus récents et. Il est ainsi très facile d’appeler par erreur la méthode de chaîne brute lorsque l’intention était d’appeler la méthode de chaîne interpolée, et d’inverse. De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.
