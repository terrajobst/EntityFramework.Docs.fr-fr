---
title: "La gestion des conflits d’accès concurrentiel - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Gestion de conflits d'accès concurrentiel

> [!NOTE]
> Cette page décrit le fonctionne de la concurrence dans EF Core et comment gérer les conflits d’accès concurrentiel dans votre application. Consultez [jetons d’accès concurrentiel](xref:core/modeling/concurrency) pour plus d’informations sur la configuration des jetons d’accès concurrentiel dans votre modèle.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) sur GitHub.

_Concurrence de la base de données_ fait référence aux situations dans lesquelles plusieurs processus ou les utilisateurs accéder ou modifier les mêmes données dans une base de données en même temps. _Contrôle d’accès concurrentiel_ fait référence à des mécanismes spécifiques permettant de garantir la cohérence des données en présence de modifications simultanées.

EF Core implémente _contrôle d’accès concurrentiel optimiste_, ce qui signifie qu’il vous permet de plusieurs processus ou utilisateurs apportent des modifications indépendamment sans la surcharge de synchronisation ou de verrouillage. Dans l’idéal, ces modifications n’interfèrent pas avec eux et seront donc en mesure de réussir. Dans le pire des cas, deux ou plusieurs processus va tenter d’apporter des modifications en conflit et seulement un d’eux doit réussir.

## <a name="how-concurrency-control-works-in-ef-core"></a>Fonctionne du contrôle d’accès concurrentiel dans EF Core

Propriétés configurées en tant que jetons d’accès concurrentiel sont utilisés pour implémenter un contrôle d’accès concurrentiel optimiste : chaque fois qu’une opération update ou delete est effectuée pendant `SaveChanges`, la valeur du jeton d’accès concurrentiel sur la base de données est comparée à la version d’origine valeur lue EF Core.

- Si les valeurs correspondent, l’opération peut s’effectuer.
- Si les valeurs ne correspondent pas, EF Core suppose qu’un autre utilisateur a effectué une opération conflictuelle et abandonne la transaction en cours.

Le cas où un autre utilisateur a effectué une opération qui est en conflit avec l’opération en cours est appelé _conflit d’accès concurrentiel_.

Les fournisseurs de base de données sont responsables de l’implémentation de la comparaison des valeurs de jeton d’accès concurrentiel.

Sur les bases de données relationnelles EF Core inclut une vérification de la valeur du jeton d’accès concurrentiel dans le `WHERE` clause de n’importe quel `UPDATE` ou `DELETE` instructions. Après l’exécution des instructions, EF Core lit le nombre de lignes qui ont été affectés.

Si aucune ligne n’est affectée, un conflit d’accès concurrentiel est détecté, et EF Core lève `DbUpdateConcurrencyException`.

Par exemple, nous pouvons choisir de configurer `LastName` sur `Person` un jeton d’accès concurrentiel. Toute opération de mise à jour sur la personne qui inclut le contrôle d’accès concurrentiel dans le `WHERE` clause :

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Résolution des conflits d’accès concurrentiel

Poursuivre l’exemple précédent, si un utilisateur tente d’enregistrer certaines modifications apportées à un `Person`, mais un autre utilisateur a déjà modifié le `LastName` l’une exception sera levée.

À ce stade, l’application peut simplement indiquer à l’utilisateur que la mise à jour a échoué en raison de modifications en conflit et déplacer sur. Mais il peut être souhaitable pour inviter l’utilisateur pour s’assurer de que cet enregistrement représente toujours la même personne réelle et recommencez l’opération.

Ce processus est un exemple de _résoudre un conflit d’accès concurrentiel_.

Résolution d’un conflit d’accès concurrentiel consiste à fusionner les modifications en attente à partir du `DbContext` avec les valeurs dans la base de données. Les valeurs de fusion varie en fonction de l’application et peut être dirigé par l’utilisateur.

**Il existe trois ensembles de valeurs disponibles pour aider à résoudre un conflit d’accès concurrentiel :**

* **Valeurs actuelles** sont les valeurs que l’application a tenté d’écrire dans la base de données.

* **Les valeurs d’origine** sont les valeurs qui ont été récupérées à l’origine à partir de la base de données, avant que toutes les modifications ont été apportées.

* **Valeurs de base de données** sont les valeurs actuellement stockées dans la base de données.

L’approche générale pour gérer les conflits d’accès concurrentiel est la suivante :

1. Catch `DbUpdateConcurrencyException` pendant `SaveChanges`.
2. Utilisez `DbUpdateConcurrencyException.Entries` pour préparer un nouvel ensemble de modifications pour les entités concernées.
3. Actualiser les valeurs d’origine du jeton d’accès concurrentiel pour refléter les valeurs actuelles dans la base de données.
4. Recommencez le processus jusqu'à ce qu’aucun conflit se produire.

Dans l’exemple suivant, `Person.FirstName` et `Person.LastName` sont configurés en tant que jetons d’accès concurrentiel. Il existe un `// TODO:` commentaire à l’emplacement où vous inclure la logique spécifique de l’application pour choisir la valeur doit être enregistré.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
