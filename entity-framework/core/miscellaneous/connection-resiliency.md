---
title: Résilience des connexions - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d5101d0622ddc2c90ddded16b9ec6cc4eb814c36
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283834"
---
# <a name="connection-resiliency"></a>Résilience des connexions

Résilience des connexions retente automatiquement les commandes de base de données ayant échoué. La fonctionnalité peut être utilisée avec une base de données en fournissant une « stratégie d’exécution », qui encapsule la logique nécessaire pour détecter les erreurs et réessayez les commandes. Fournisseurs EF Core peuvent fournir des stratégies d’exécution adaptées à leurs conditions d’échec de la base de données spécifique et les stratégies de nouvelle tentative optimal.

Par exemple, le fournisseur SQL Server inclut une stratégie d’exécution qui est conçu spécialement pour SQL Server (y compris SQL Azure). Il connaît les types d’exception qui peuvent être retentées et a des valeurs par défaut sensibles pour le nombre maximal de tentatives, délai entre les nouvelles tentatives, etc.

Une stratégie d’exécution est spécifiée lorsque vous configurez les options pour votre contexte. Il s’agit généralement dans le `OnConfiguring` méthode de votre contexte dérivé, ou dans `Startup.cs` pour une application ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Stratégie d’exécution personnalisée

Il existe un mécanisme pour inscrire une stratégie d’exécution personnalisée de votre choix si vous souhaitez modifier les valeurs par défaut.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Transactions et les stratégies d’exécution

Une stratégie d’exécution qui réessaie automatiquement en cas d’échec doit être en mesure de lire chaque opération dans un bloc de nouvelle tentative échoue. Lorsque de nouvelles tentatives sont activées, chaque opération que vous effectuez par le biais de EF Core devient sa propre opération de nouvelle tentative. Autrement dit, chaque requête et chaque appel à `SaveChanges()` va être retentée en tant qu’unité si une défaillance passagère se produit.

Toutefois, si votre code lance une transaction à l’aide de `BeginTransaction()` vous définissez votre propre groupe d’opérations qui doivent être traités en tant qu’unité, et tous les éléments à l’intérieur de la transaction doit être lu une défaillance intervient. Vous recevrez une exception semblable à ce qui suit si vous essayez d’effectuer lors de l’utilisation d’une stratégie d’exécution :

> InvalidOperationException : La stratégie d’exécution configurée « SqlServerRetryingExecutionStrategy » ne prend pas en charge des transactions lancée par l’utilisateur. Utilisez la stratégie d’exécution retournée par « DbContext.Database.CreateExecutionStrategy() » pour exécuter toutes les opérations de la transaction en tant qu’ensemble pouvant être retenté.

La solution consiste à appeler manuellement la stratégie d’exécution avec un délégué représentant tout ce qui doit être exécutée. Si une défaillance passagère se produit, la stratégie d’exécution appelle à nouveau le délégué.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Échec de validation de transaction et le problème de l’idempotence

En règle générale, en cas d’échec de la connexion la transaction en cours est annulée. Toutefois, si la connexion est perdue pendant la transaction étant validées résultant état de la transaction est inconnu. Consultez ce [billet de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) pour plus d’informations.

Par défaut, la stratégie d’exécution va retenter l’opération comme si la transaction a été annulée, mais si elle n’est pas le cas cela entraîne une exception si le nouvel état de la base de données n’est pas compatible ou pourrait entraîner **une altération des données** si le opération ne repose pas sur un état particulier, par exemple lors de l’insertion d’une nouvelle ligne avec les valeurs de clé généré automatiquement.

Il existe plusieurs façons de gérer cela.

### <a name="option-1---do-almost-nothing"></a>Option 1 - (presque) nothing

La probabilité d’un échec de connexion lors de la validation de transaction est faible, elle peut être acceptable pour votre application de simplement échouer si cette condition se produit réellement.

Toutefois, vous devez éviter d’utiliser des clés générées par le magasin afin de garantir qu’une exception est levée au lieu d’ajouter une ligne en double. Envisagez d’utiliser une valeur GUID générée par le client ou un générateur de valeur de côté client.

### <a name="option-2---rebuild-application-state"></a>Option 2 : état de l’application reconstruction

1. Ignorer actuel `DbContext`.
2. Créer un nouveau `DbContext` et restaurer l’état de votre application à partir de la base de données.
3. Informer l’utilisateur que la dernière opération ne peut-être pas terminée avec succès.

### <a name="option-3---add-state-verification"></a>Option 3 : ajouter la vérification de l’état

Pour la plupart des opérations qui modifient l’état de la base de données, il est possible d’ajouter du code qui vérifie si elle a réussi. Entity Framework fournit une méthode d’extension pour simplifier ce processus - `IExecutionStrategy.ExecuteInTransaction`.

Cette méthode commence une transaction est validée et accepte également une fonction dans le `verifySucceeded` paramètre qui est appelé lorsqu’une erreur temporaire se produit pendant la validation de transaction.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Ici `SaveChanges` est appelé avec `acceptAllChangesOnSuccess` définie sur `false` afin d’éviter la modification de l’état de la `Blog` entité à `Unchanged` si `SaveChanges` réussit. Cela permet de retenter l’opération même si la validation échoue et la transaction est restaurée.

### <a name="option-4---manually-track-the-transaction"></a>Option 4 : suivre manuellement la transaction

Si vous avez besoin d’utiliser des clés générées par le magasin ou besoin d’un moyen générique de gestion des échecs de validation qui ne dépend de l’opération effectuée chaque transaction peut être attribuée à un ID qui est vérifié lors de la validation échoue.

1. Ajouter une table à la base de données utilisé pour suivre l’état des transactions.
2. Insérer une ligne dans la table au début de chaque transaction.
3. Si la connexion échoue lors de la validation, vérifier la présence de la ligne correspondante dans la base de données.
4. Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Vérifiez que le contexte utilisé pour la vérification a une stratégie d’exécution définie comme la connexion est susceptible d’échouer lors de la vérification en cas d’échec lors de la validation de transaction.
