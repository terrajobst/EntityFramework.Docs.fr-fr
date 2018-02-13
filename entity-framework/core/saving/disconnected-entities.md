---
title: "Entités déconnectées - EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a>Entités déconnectées

Une instance de DbContext suivre automatiquement les entités retournées à partir de la base de données. Modifications apportées à ces entités puis seront détectées lorsque SaveChanges est appelée et la base de données sera mise à jour en fonction des besoins. Consultez [enregistrer base](basic.md) et [les données associées](related-data.md) pour plus d’informations.

Toutefois, parfois, les entités sont interrogées à l’aide d’une instance de contexte et puis enregistré à l’aide d’une autre instance. Cela se produit souvent dans les scénarios « déconnectés » telle qu’une application web où les entités sont interrogées, envoyées au client, modifiées, envoyées sur le serveur dans une demande et puis enregistrées. Dans ce cas, le deuxième contexte d’instance doit savoir si les entités sont nouvelle (doit être inséré) existant (doit être mis à jour).

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) sur GitHub.

> [!TIP]
> EF Core peut suivre seulement une seule instance d’une entité avec une valeur de clé primaire donnée. La meilleure façon d’éviter ce problème consiste à utiliser un contexte de courte durée de vie pour chaque unité de travail tels que le contexte est vide, au démarrage en cours a entités associées, enregistre ces entités, puis le contexte est supprimé et ignorée.

## <a name="identifying-new-entities"></a>Identification des entités

### <a name="client-identifies-new-entities"></a>Client identifie de nouvelles entités

Le cas le plus simple à gérer est lorsque le client informe le serveur si l’entité est nouveau ou existant. Par exemple, souvent la demande d’insertion d’une nouvelle entité diffère de la demande pour mettre à jour une entité existante.

Le reste de cette section couvre les cas où il est nécessaire de déterminer, d’une autre manière, s’il faut insérer ou mettre à jour.

### <a name="with-auto-generated-keys"></a>Avec les clés générées automatiquement

La valeur d’une clé générée automatiquement peut souvent être utilisée pour déterminer si une entité doit être insérée ou mise à jour. Si la clé n’a pas été défini (autrement dit il toujours a la valeur CLR par défaut null, zéro, etc.), puis l’entité doit être de nouveau et a besoin d’insertion. En revanche, si la valeur de clé a été définie, puis il doit avoir déjà été précédemment enregistré et doit mettre à jour. En d’autres termes, si la clé a une valeur, puis d’entité a été interrogée, envoyées au client et a maintenant revenir à mettre à jour.

Il est facile de rechercher une clé non définie lorsque le type d’entité est connu :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Toutefois, EF a également un moyen intégré pour n’importe quel type d’entité et le type de clé :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Les clés sont définies dès que les entités sont suivies par le contexte, même si l’entité est dans l’état Added. Ainsi, lors du parcours d’un graphique d’entités et de décider quelles actions effectuer avec chaque, telles que lors de l’utilisation de l’API TrackGraph. La valeur de clé doit uniquement être utilisée de la manière illustrée ici _avant_ n’importe quel appel est effectué pour effectuer le suivi de l’entité.

### <a name="with-other-keys"></a>Avec les autres clés

Un autre mécanisme est nécessaire pour identifier les nouvelles entités lorsque les valeurs de clés ne sont pas générés automatiquement. Il existe deux approches générales pour cela :
 * Requête pour l’entité
 * Passez un indicateur à partir du client

Pour rechercher l’entité, utiliser simplement la méthode de recherche :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

N’entre pas dans le cadre de ce document pour afficher le code complet pour le passage d’un indicateur d’un client. Dans une application web, cela signifie généralement effectuant des demandes différentes pour différentes actions, ou en passant un état dans la demande, puis extraire dans le contrôleur.

## <a name="saving-single-entities"></a>L’enregistrement des entités uniques

S’il est connu ou non une insertion ou une mise à jour est nécessaire, puis ajouter ou mise à jour peut être utilisé de manière appropriée :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Toutefois, si l’entité utilise les valeurs de clés générées automatiquement, la méthode de mise à jour peut être utilisée pour les deux cas :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Normalement, la méthode de mise à jour marque l’entité pour la mise à jour, insertion pas. Toutefois, si l’entité a une clé générée automatiquement, et aucune valeur de clé n’a été défini, puis l’entité est automatiquement marquée pour insérer.

> [!TIP]  
> Ce comportement a été introduit dans EF Core 2.0. Pour des versions antérieures, il est toujours nécessaire de choisir explicitement ajouter ou mise à jour.

Si l’entité n’utilise pas les clés générées automatiquement, puis l’application doit décider si l’entité doit être insérée ou mise à jour : par exemple :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Les étapes ici sont :
* Si Find retourne null, la base de données ne contient pas déjà le blog avec cet ID, afin de nous appeler, puis ajoutez le marquer pour l’insertion.
* Si la recherche retourne une entité, puis il existe dans la base de données et le contexte suit maintenant l’entité existante
  * Ensuite, nous utilisons SetValues pour définir les valeurs de toutes les propriétés de cette entité à celles fournies par le client.
  * L’appel de SetValues marque l’entité à mettre à jour en fonction des besoins.

> [!TIP]  
> SetValues uniquement marque a modifié les propriétés qui ont des valeurs différentes à ceux de l’entité suivie. Cela signifie que lorsque la mise à jour est envoyée, uniquement les colonnes qui ont été modifiées seront mise à jour. (Et si rien n’a changé, alors aucune mise à jour ne sera envoyé à tout).

## <a name="working-with-graphs"></a>Utilisation des graphiques

### <a name="identity-resolution"></a>Résolution d’identité

Comme indiqué ci-dessus, EF Core peut suivre uniquement une seule instance d’une entité avec une valeur de clé primaire donnée. Lorsque vous travaillez avec des graphiques le graphique doit être créé dans l’idéal, telle que cet invariant est géré, et le contexte doit être utilisé pour qu’une seule unité de travail. Si le graphique ne contient pas les doublons, il sera nécessaire traiter le graphique avant de l’envoyer à EF de consolider plusieurs instances en un seul. Cela peut être non trivial où les instances ont des valeurs en conflit et les relations, donc les doublons de consolidation doivent être effectuées dès que possible dans le pipeline de votre application afin d’éviter la résolution des conflits.

### <a name="all-newall-existing-entities"></a>Toutes les entités de nouveau ou l’ensemble existantes

Un exemple de l’utilisation de graphiques est insertion ou mise à jour d’un blog ainsi que sa collection de messages associés. Si toutes les entités dans le graphique doivent être insérées, ou tous doivent être mis à jour, le processus est identique à celle décrite ci-dessus pour les entités uniques. Par exemple, un graphique des blogs et publications créées comme suit :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

peut être inséré comme suit :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

L’appel Add marque le blog et toutes les publications à insérer.

De même, si toutes les entités dans un graphique doivent être mis à jour, puis Update peut être utilisé :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Le blog et toutes ses publications seront marquées pour être mis à jour.

### <a name="mix-of-new-and-existing-entities"></a>Combinaison d’entités nouvelle et existantes

Avec les clés générées automatiquement, mise à jour de nouveau utilisable pour les insertions et mises à jour, même si le graphique contient un mélange d’entités qui nécessitent l’insertion et ceux qui nécessitent la mise à jour :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Mise à jour marque une entité dans le graphique, le blog ou post, d’insertion si elle ne dispose pas d’un ensemble de la valeur de clé, tandis que toutes les autres entités sont marquées pour la mise à jour.

Comme précédemment, lorsque vous n’utilisez ne pas les clés générées automatiquement, une requête et un traitement peuvent être utilisés :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Gestion des suppressions

DELETE peut être difficile à gérer depuis souvent l’absence d’une entité signifie qu’il doit être supprimé. Une façon de gérer cela consiste à utiliser des « suppressions récupérables » telles que l’entité est marquée comme supprimée au lieu d’être supprimées. Supprime, puis devient le même que les mises à jour. Suppressions récupérables qui peuvent être implémentées à l’aide de [filtres de requête](xref:core/querying/filters).

Pour les suppressions de valeur est trues, il est courant d’utiliser une extension du modèle de requête pour effectuer ce qui est essentiellement une comparaison de graphique. Exemple :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

En interne, ajouter, attacher et mise à jour utilisent traversée de graphique avec une détermination effectuée pour chaque entité que si elle doit être marquée comme Added (pour insérer), Modified (mettre à jour), Unchanged (ne rien faire), ou supprimés (à supprimer). Ce mécanisme est exposé via l’API TrackGraph. Par exemple, supposons que lorsque le client envoie un graphique d’entités qu’il définit certains indicateur sur chaque entité indiquant comment il doit être géré. TrackGraph peut ensuite être utilisée pour traiter cet indicateur :

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Les indicateurs sont uniquement affichés dans le cadre de l’entité pour simplifier l’exemple. En général, les indicateurs serait partie d’un DTO ou d’un autre état inclus dans la demande.
