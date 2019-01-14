---
title: 'Requêtes SQL brutes : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 5bddddfbc2fe8d0ba99914f03b28bde4076fae42
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058708"
---
# <a name="raw-sql-queries"></a>Requêtes SQL brutes

Entity Framework Core vous permet d’examiner les requêtes SQL brutes lorsque vous travaillez avec une base de données relationnelle. Cela peut être utile si la requête que vous voulez effectuer ne peut pas être exprimée à l’aide de LINQ ou que l’utilisation d’une requête LINQ se traduit par des requêtes SQL inefficaces. Les requêtes SQL brutes peuvent retourner des types d’entités ou, à partir d’EF Core 2.1, des [types de requête](xref:core/modeling/query-types) qui font partie de votre modèle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="limitations"></a>Limitations

Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL brutes :

* La requête SQL doit retourner des données pour toutes les propriétés du type d’entité ou de requête.

* Les noms de colonne dans le jeu de résultats doivent correspondre aux noms de colonne mappés aux propriétés. Notez que cela est différent à compter d’EF6, où le mappage de propriétés/colonnes était ignoré pour les requêtes SQL brutes et où les noms de colonne du jeu de résultats devaient correspondre aux noms des propriétés.

* La requête SQL ne peut pas contenir de données associées. Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de l’opérateur `Include` pour retourner des données associées (consultez [Inclusion de données associées](#including-related-data)).

* Les instructions `SELECT` passées à cette méthode doivent généralement être composables : si EF Core a besoin évaluer des opérateurs de requête supplémentaires sur le serveur (par exemple, pour traduire les opérateurs LINQ appliqués après `FromSql`), le SQL fourni sera considéré comme une sous-requête. Cela signifie que l’instruction SQL passée ne doit pas contenir de caractères ou d’options qui ne sont pas valides sur une sous-requête, comme :
  * un point-virgule de fin
  * Sur le serveur SQL Server, une indication de niveau de requête en fin (par exemple, `OPTION (HASH JOIN)`)
  * Sur le serveur SQL Server, une clause `ORDER BY` n’est pas accompagnée de `TOP 100 PERCENT` dans la clause `SELECT`

* Les instructions SQL autres que `SELECT` sont reconnues automatiquement en tant que non composables. Par conséquent, les résultats complets des procédures stockées sont toujours retournés au client et tous les opérateurs LINQ appliqués après `FromSql` sont évalués en mémoire.

## <a name="basic-raw-sql-queries"></a>Requêtes SQL brutes de base

Vous pouvez utiliser la méthode d’extension *FromSql* pour lancer une requête LINQ basée sur une requête SQL brute.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Les requêtes SQL brutes peuvent servir à exécuter une procédure stockée.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passage de paramètres

Comme avec toute API qui accepte SQL, il est important de paramétrer toutes les entrées d’utilisateur pour protéger contre les attaques par injection SQL. Vous pouvez inclure des espaces réservés de paramètre dans la chaîne de requête SQL et fournir ensuite les valeurs de paramètre en tant qu’arguments supplémentaires. Les valeurs de paramètre que vous fournissez seront automatiquement converties en `DbParameter`.

L’exemple suivant passe un paramètre unique à une procédure stockée. Si cela ressemble à la syntaxe de `String.Format`, la valeur fournie est encapsulée dans un paramètre et le nom de paramètre généré est inséré là où l’espace réservé `{0}` a été spécifié.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Il s’agit de la même requête, mais avec la syntaxe d’interpolation de chaîne, qui est prise en charge dans EF Core 2.0 et versions ultérieures :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Vous pouvez également construire un objet DbParameter et le fournir en tant que valeur de paramètre. Cela vous permet d’utiliser des paramètres nommés dans la chaîne de requête SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composition avec LINQ

Si la requête SQL peut être composée dans la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ. Les requêtes SQL qui peuvent être composées commencent par le mot clé `SELECT`.

L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et compose dessus à l’aide de LINQ pour effectuer le filtrage et le tri.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Inclusion de données associées

La composition avec les opérateurs LINQ peut servir à inclure les données associées dans la requête.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Utilisez toujours le paramétrage pour les requêtes SQL brutes :** les API acceptant une chaîne SQL brute comme `FromSql` et `ExecuteSqlCommand` autorisent les valeurs à passer facilement en tant que paramètres. En plus de valider l’entrée utilisateur, vous devez toujours utiliser le paramétrage pour toutes les valeurs utilisées dans une commande/requête en SQL brut. Si vous utilisez la concaténation de chaînes pour générer dynamiquement une partie de la chaîne de requête, vous êtes responsable de la validation de l’entrée pour vous protéger contre les attaques par injection SQL.
