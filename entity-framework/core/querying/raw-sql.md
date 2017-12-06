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
# <a name="raw-sql-queries"></a>Requêtes SQL brut

Entity Framework Core permet de liste déroulante pour les requêtes SQL bruts lorsque vous travaillez avec une base de données relationnelle. Cela peut être utile si la requête que vous voulez effectuer ne peut pas être exprimée à l’aide de LINQ ou à l’aide d’une requête LINQ se traduit par inefficace SQL envoyée à la base de données.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="limitations"></a>Limitations

Il existe quelques limitations à connaître lors de l’utilisation des requêtes SQL bruts :
* Requêtes SQL peuvent uniquement servir à retourner des types d’entités qui font partie de votre modèle. Il existe une amélioration de notre backlog à [Activer retour des types d’ad hoc à partir des requêtes SQL brutes](https://github.com/aspnet/EntityFramework/issues/1862).

* La requête SQL doit retourner des données pour toutes les propriétés du type d’entité.

* Les noms de colonnes dans le jeu de résultats doivent correspondre aux noms de colonnes mappées aux propriétés. Notez que cela est différent d’EF6 où mappage de propriété/colonne a été ignorée pour les requêtes SQL bruts et noms devaient correspondre aux noms de propriété de colonne de jeu de résultats.

* La requête SQL ne peut pas contenir les données associées. Toutefois, dans de nombreux cas, vous pouvez composer au-dessus de la requête à l’aide de la `Include` opérateur pour retourner les données associées (consultez [, y compris les données associées](#including-related-data)).

## <a name="basic-raw-sql-queries"></a>Requêtes SQL brutes de base

Vous pouvez utiliser la *FromSql* méthode d’extension pour lancer une requête LINQ basée sur une requête SQL brute.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Les requêtes SQL bruts peuvent servir à exécuter une procédure stockée.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passage de paramètres

Comme avec toute API qui accepte SQL, il est important de paramétrer des entrées d’utilisateur pour protéger contre une attaque par injection de SQL. Vous pouvez inclure des espaces réservés de paramètre dans la chaîne de requête SQL et fournissez ensuite les valeurs de paramètre en tant qu’arguments supplémentaires. Les valeurs de paramètres que vous fournissez seront automatiquement converties en un `DbParameter`.

L’exemple suivant passe un paramètre unique à une procédure stockée. Alors que cela peut vous paraître comme `String.Format` syntaxe, la valeur fournie est encapsulée dans un paramètre et le nom de paramètre généré inséré où le `{0}` espace réservé a été spécifié.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Ceci est la même requête, mais à l’aide de la syntaxe d’interpolation de chaîne, qui est pris en charge dans EF Core 2.0 et versions ultérieures :

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Vous pouvez également construire un objet DbParameter et fournir en tant que valeur de paramètre. Cela vous permet d’utiliser les paramètres nommés dans la chaîne de requête SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composition avec LINQ

Si la requête SQL peut être composée de la base de données, vous pouvez composer au-dessus de la requête SQL brute initiale à l’aide des opérateurs LINQ. Les requêtes SQL qui peuvent être composées sur en cours avec le `SELECT` (mot clé).

L’exemple suivant utilise une requête SQL brute qui sélectionne à partir d’une fonction table (TVF) et les dessus à l’aide de LINQ pour effectuer le filtrage et le tri.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Y compris les données associées

Composition avec les opérateurs LINQ peut servir à inclure les données associées dans la requête.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Utilisez toujours le paramétrage pour les requêtes SQL brutes :** API acceptant une brute SQL de chaîne tel que `FromSql` et `ExecuteSqlCommand` autorise les valeurs à passer facilement en tant que paramètres. En plus de valider l’entrée d’utilisateur, vous devez toujours utiliser le paramétrage pour toutes les valeurs utilisées dans une requête SQL brut/commande. Si vous utilisez la concaténation de chaînes pour générer dynamiquement une partie de la chaîne de requête, vous êtes responsable de la validation d’entrée pour vous protéger contre les attaques par injection SQL.
