---
title: "Chargement associées données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a>Chargement de données connexes

Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées. Il existe trois modèles d’O/RM communs utilisés pour charger les données associées.
* **Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.
* **Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à une date ultérieure.
* **Chargement différé** signifie que les données associées en toute transparence sont chargées à partir de la base de données que lorsque la propriété de navigation est accessible. Chargement différé n’est pas encore possible avec EF de base.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="eager-loading"></a>Chargement hâtif

Vous pouvez utiliser la `Include` méthode pour spécifier les données connexes à inclure dans les résultats de la requête. Dans l’exemple suivant, les blogs sont retournés dans les résultats auront leurs `Posts` propriété remplie avec les messages liés.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core va corriger automatiquement des propriétés de navigation à toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte. Par conséquent, même si vous n’incluez pas explicitement les données pour une propriété de navigation, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.


Vous pouvez inclure des données liées provenant de plusieurs relations dans une requête unique.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Y compris plusieurs niveaux

Vous pouvez descendre, par le biais des relations à inclure plusieurs niveaux de données connexes à l’aide du `ThenInclude` (méthode). L’exemple suivant charge tous les blogs, leurs messages liés et l’auteur de chaque publication.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrect et peut provoquer des expressions correctes marquage avec des erreurs de syntaxe lorsque vous utilisez la `ThenInclude` méthode après une propriété de navigation de collection. Ceci est le symptôme d’un bogue IntelliSense suivi à https://github.com/dotnet/roslyn/issues/8237. Il est possible d’ignorer ces erreurs de syntaxe fausses tant que le code est correct et qu’il peut être compilé avec succès. 

Vous pouvez chaîner plusieurs appels à `ThenInclude` pour continuer, notamment les prochaines niveaux de données associées.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Vous pouvez combiner tous de cette option pour inclure les données associées à partir de plusieurs niveaux et plusieurs racines dans la même requête.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Voulez-vous inclure plusieurs entités associées pour l’une des entités qui est incluse. Par exemple, lors de l’interrogation `Blog`s, vous incluez `Posts` , puis à inclure à la fois le `Author` et `Tags` de la `Posts`. Pour ce faire, vous devez spécifier chaque inclure le chemin d’accès à partir de la racine. Par exemple, `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`. Cela ne signifie pas que vous obtiendrez des jointures redondantes, dans la plupart des cas QU'EF consolide les jointures lors de la génération SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignoré inclut

Si vous modifiez la requête afin qu’elle retourne ne sont plus des instances de la requête a commencé avec le type d’entité, les opérateurs include sont ignorés.

Dans l’exemple suivant, les opérateurs include sont basées sur les `Blog`, puis le `Select` opérateur est utilisé pour modifier la requête pour retourner un type anonyme. Dans ce cas, les opérateurs include n’ont aucun effet.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Par défaut, EF Core consigne un avertissement lorsque incluent les opérateurs sont ignorés. Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation. Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré lever ou ne rien faire. Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Chargement explicite

> [!NOTE]  
> Cette fonctionnalité a été introduite dans EF Core 1.1.

Vous pouvez charger explicitement une propriété de navigation via la `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées. Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core automatiquement définit les propriétés de navigation de l’entité qui vient d’être chargé pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargé pour faire référence à la entité qui vient d’être chargé.

### <a name="querying-related-entities"></a>Interrogation des entités connexes

Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.

Cela vous permet de vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Vous pouvez également filtrer les entités associées sont chargées en mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Chargement différé

Chargement différé n’est pas encore pris en charge par EF Core. Vous pouvez afficher le [élément chargement différé notre backlog](https://github.com/aspnet/EntityFramework/issues/3797) pour effectuer le suivi de cette fonctionnalité.

## <a name="related-data-and-serialization"></a>Sérialisation et les données associées

Étant donné que les propriétés de EF Core sera automatiquement correctives navigation, vous pouvez retrouver avec des cycles dans votre graphique d’objets. Par exemple, le chargement d’un blog et il est lié billets aboutit à un objet de blog qui fait référence à une collection de publications. Chacune de ces publications aura une référence vers le blog.

Certaines infrastructures de sérialisation n’autorisent pas ces cycles. Par exemple, Json.NET lève l’exception suivante si un cycle est prématurément.

> Newtonsoft.Json.JsonSerializationException : Self faisant référence à la boucle détectée pour la propriété « Blog » avec le type 'MyApplication.Models.Blog'.

Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il se trouve dans le graphique d’objets. Cette opération est effectuée le `ConfigureServices(...)` méthode dans `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
