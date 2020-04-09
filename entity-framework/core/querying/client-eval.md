---
title: Évaluation client vs serveur - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417759"
---
# <a name="client-vs-server-evaluation"></a>Évaluation du client par rapport au serveur

En règle générale, Entity Framework Core tente d’évaluer autant que possible une requête sur le serveur. EF Core convertit certaines parties de la requête en paramètres qu’elle peut évaluer du côté du client. Le reste de la requête (ainsi que les paramètres générés) est remis au fournisseur de base de données pour déterminer la requête de base de données équivalente à évaluer sur le serveur. EF Core soutient l’évaluation partielle des clients dans la `Select()`projection de haut niveau (essentiellement, le dernier appel à ). Si la projection de haut niveau de la requête ne peut pas être traduite sur le serveur, EF Core récupérera toutes les données requises du serveur et évaluera les parties restantes de la requête sur le client. Si EF Core détecte une expression, dans n’importe quel endroit autre que la projection de haut niveau, qui ne peut pas être traduite sur le serveur, alors elle lance une exception de temps d’exécution. Découvrez [comment fonctionne la requête](xref:core/querying/how-query-works) pour comprendre comment EF Core détermine ce qui ne peut pas être traduit en serveur.

> [!NOTE]
> Avant la version 3.0, Entity Framework Core a soutenu l’évaluation des clients n’importe où dans la requête. Pour plus d’informations, voir la [section versions précédentes](#previous-versions).

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="client-evaluation-in-the-top-level-projection"></a>Évaluation des clients dans la projection de haut niveau

Dans l’exemple suivant, une méthode d’aide est utilisée pour normaliser les URL pour les blogs, qui sont retournés à partir d’une base de données SQL Server. Étant donné que le fournisseur de serveurs SQL n’a aucune idée de la façon dont cette méthode est implémentée, il n’est pas possible de la traduire en SQL. Tous les autres aspects de la requête sont évalués `URL` dans la base de données, mais le passage de la méthode retournée est fait sur le client.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Évaluation non étayée par les clients

Bien que l’évaluation des clients soit utile, elle peut parfois entraîner de mauvaises performances. Considérez la requête suivante, dans laquelle la méthode d’aide est maintenant utilisée dans un filtre où. Étant donné que le filtre ne peut pas être appliqué dans la base de données, toutes les données doivent être extraites dans la mémoire pour appliquer le filtre sur le client. Sur la base du filtre et de la quantité de données sur le serveur, l’évaluation des clients pourrait entraîner de mauvaises performances. Ainsi, Entity Framework Core bloque une telle évaluation des clients et jette une exception de temps d’exécution.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Évaluation explicite des clients

Vous devrez peut-être entrer en vigueur explicitement dans l’évaluation des clients dans certains cas comme

- La quantité de données est faible de sorte que l’évaluation sur le client n’entraîne pas une pénalité de performance énorme.
- L’opérateur LINQ utilisé n’a pas de traduction côté serveur.

Dans de tels cas, vous pouvez explicitement `AsEnumerable` opter pour l’évaluation des clients en appelant des méthodes comme ou `ToList` (ou`AsAsyncEnumerable` `ToListAsync` pour async). En `AsEnumerable` vous utilisant serait streaming les `ToList` résultats, mais en utilisant cause de tamponnage en créant une liste, qui prend également la mémoire supplémentaire. Bien que si vous énumérez plusieurs fois, puis stocker des résultats dans une liste aide plus car il n’y a qu’une seule requête à la base de données. Selon l’utilisation particulière, vous devez évaluer quelle méthode est la plus utile pour le cas.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Fuite de mémoire potentielle dans l’évaluation du client

Étant donné que la traduction et la compilation des requêtes sont coûteuses, EF Core cache le plan de requête compilé. Le délégué mis en cache peut utiliser le code client tout en effectuant l’évaluation des clients de la projection de haut niveau. EF Core génère des paramètres pour les parties évaluées par le client de l’arbre et réutilise le plan de requête en remplaçant les valeurs de paramètres. Mais certaines constantes dans l’arbre d’expression ne peuvent pas être converties en paramètres. Si le délégué mis en cache contient de telles constantes, ces objets ne peuvent pas être des ordures collectées puisqu’ils sont encore référencés. Si un tel objet contient un DbContext ou d’autres services en elle, alors il pourrait provoquer l’utilisation de la mémoire de l’application à croître au fil du temps. Ce comportement est généralement le signe d’une fuite de mémoire. EF Core lance une exception chaque fois qu’elle rencontre des constantes d’un type qui ne peut pas être cartographié à l’aide du fournisseur de base de données actuel. Les causes communes et leurs solutions sont les suivantes :

- **À l’aide d’une méthode d’instance**: Lorsque vous utilisez des méthodes d’instance dans une projection client, l’arbre d’expression contient une constante de l’instance. Si votre méthode n’utilise aucune donnée de l’instance, envisagez de rendre la méthode statique. Si vous avez besoin de données d’instance dans le corps de la méthode, puis passer les données spécifiques comme un argument à la méthode.
- **Passer des arguments constants à la** `this` méthode : Ce cas se pose généralement en utilisant dans un argument à la méthode client. Envisagez de diviser l’argument à plusieurs arguments scalaires, qui peuvent être cartographiés par le fournisseur de base de données.
- **Autres constantes**: Si une constante est à l’étude dans tous les autres cas, vous pouvez évaluer si la constante est nécessaire dans le traitement. S’il est nécessaire d’avoir la constante, ou si vous ne pouvez pas utiliser une solution à partir des cas ci-dessus, puis créer une variable locale pour stocker la valeur et utiliser la variable locale dans la requête. EF Core convertira la variable locale en paramètre.

## <a name="previous-versions"></a>Versions antérieures

La section suivante s’applique aux versions EF Core avant 3.0.

Les anciennes versions EF Core ont soutenu l’évaluation des clients dans n’importe quelle partie de la requête- et pas seulement la projection de haut niveau. C’est pourquoi les requêtes similaires à celles affichées dans la section [d’évaluation des clients non pris en compte](#unsupported-client-evaluation) ont fonctionné correctement. Étant donné que ce comportement pourrait causer des problèmes de performance inaperçus, EF Core a enregistré un avertissement d’évaluation client. Pour plus d’informations sur l’affichage de la sortie d’enregistrement, voir [Logging](xref:core/miscellaneous/logging).

Optionnellement EF Core vous a permis de modifier le comportement par défaut pour lancer une exception ou ne rien faire lors de l’évaluation du client (sauf dans la projection). Le comportement de lancer d’exception le rendrait semblable au comportement dans 3.0. Pour modifier le comportement, vous devez configurer des avertissements `DbContext.OnConfiguring`tout en `Startup.cs` configurant les options pour votre contexte - généralement en , ou si vous utilisez ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
