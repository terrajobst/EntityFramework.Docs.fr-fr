---
title: Résilience des connexions - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053529"
---
# <a name="connection-resiliency"></a>Résilience des connexions

Résilience des connexions tente automatiquement les commandes de base de données a échoué. La fonctionnalité peut être utilisée avec une base de données, vous devez fournir une « stratégie d’exécution », qui encapsule la logique nécessaire pour détecter les erreurs, puis réessayez de commandes. Les fournisseurs de base EF peuvent fournir des stratégies d’exécution adaptées à leurs conditions d’échec spécifique de la base de données et les stratégies de nouvelle tentative optimal.

Par exemple, le fournisseur SQL Server inclut une stratégie d’exécution qui est conçu spécialement pour SQL Server (y compris SQL Azure). Il connaît les types d’exception qui peuvent être retentées et a des valeurs par défaut pour le nombre maximal de tentatives, délai entre les nouvelles tentatives, etc.

Stratégie d’exécution est spécifiée lorsque vous configurez les options de votre contexte. Il s’agit généralement dans le `OnConfiguring` méthode de votre contexte dérivée ou dans `Startup.cs` pour une application ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Stratégie d’exécution personnalisés

Il existe un mécanisme d’enregistrement d’une stratégie d’exécution personnalisée de votre choix si vous souhaitez modifier les valeurs par défaut.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Les transactions et les stratégies d’exécution

Stratégie d’exécution tente automatiquement de défaillances doit être en mesure de lire chaque opération dans un bloc de nouvelle tentative échoue. Lorsque de nouvelles tentatives sont activées, chaque opération que vous effectuez via EF Core devient son propre opération reproductible, autrement dit, chaque requête et chaque appel à `SaveChanges()` sera tentée à nouveau en tant qu’unité si une défaillance temporaire se produit.

Toutefois, si votre code initie une transaction à l’aide de `BeginTransaction()` vous définissez votre propre groupe d’opérations qui doivent être traités en tant qu’unité, autrement dit, tous les éléments à l’intérieur de la transaction devra être lu une défaillance intervient. Vous recevez une exception semblable à ce qui suit si vous essayez d’effectuer lors de l’utilisation d’une stratégie d’exécution.

> InvalidOperationException : La stratégie d’exécution configurée 'SqlServerRetryingExecutionStrategy' ne prend pas en charge les transactions démarrées par l’utilisateur. Utilisez la stratégie d’exécution retournée par 'DbContext.Database.CreateExecutionStrategy()' pour exécuter toutes les opérations dans la transaction comme une unité reproductible.

La solution consiste à appeler manuellement la stratégie d’exécution avec un délégué qui représente tous les éléments qui doit être exécutée. Si une défaillance temporaire se produit, la stratégie d’exécution appelle le délégué à nouveau.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Échec de validation de transaction et le problème idempotence

En général, lorsqu’un échec de connexion la transaction en cours est annulée. Toutefois, si la connexion est perdue pendant la transaction étant validées résultant de la transaction de l’état est inconnu. Consultez ce [billet de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) pour plus d’informations.

Par défaut, la stratégie d’exécution va retenter l’opération comme si la transaction a été restaurée, mais si elle n’est pas le cas cela entraîne une exception si le nouvel état de la base de données n’est pas compatible ou peut entraîner des **une altération des données** si le opération ne repose pas sur un état particulier, par exemple lors de l’insertion d’une nouvelle ligne avec les valeurs de clés générées automatiquement.

Il existe plusieurs façons de gérer cela.

### <a name="option-1---do-almost-nothing"></a>Option 1 - (presque) nothing

La probabilité d’un échec de connexion lors de la validation de transaction est faible, il peut être acceptable pour votre application pour simplement de faire échouer si cette condition se produit réellement.

Toutefois, vous devez éviter d’utiliser des clés générées par le magasin afin de garantir qu’une exception est levée au lieu d’ajouter une ligne en double. Envisagez d’utiliser une valeur GUID générée par le client ou un générateur de valeur du côté client.

### <a name="option-2---rebuild-application-state"></a>Option 2 - état de l’application reconstruction

1. Ignorer l’actuel `DbContext`.
2. Créer un nouveau `DbContext` et restaurer l’état de votre application à partir de la base de données.
3. Informer l’utilisateur que la dernière opération ne peut-être pas été correctement déroulée.

### <a name="option-3---add-state-verification"></a>Option 3 - Vérification de l’état de l’ajout

Pour la plupart des opérations qui modifient l’état de la base de données, il est possible d’ajouter du code qui vérifie si elle a réussi. EF fournit une méthode d’extension pour simplifier cette utilisation - `IExecutionStrategy.ExecuteInTransaction`.

Cette méthode commence et valide une transaction et accepte également une fonction dans le `verifySucceeded` paramètre qui est appelé lorsqu’une erreur temporaire se produit pendant la validation de transaction.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Ici `SaveChanges` est appelé avec `acceptAllChangesOnSuccess` la valeur `false` pour éviter de modifier l’état de la `Blog` entité `Unchanged` si `SaveChanges` réussit. Cela permet de recommencer l’opération de même si la validation échoue et que la transaction est restaurée.

### <a name="option-4---manually-track-the-transaction"></a>Option 4 : suivre manuellement la transaction

Si vous devez utiliser des clés générées par le magasin ou générique afin de gérer les erreurs de validation ne dépend de l’opération effectuée chaque transaction peut être attribuée à un ID qui est vérifié lors de la validation échoue.

1. Ajouter une table à la base de données utilisé pour suivre l’état des transactions.
2. Insérer une ligne dans la table au début de chaque transaction.
3. Si la connexion échoue lors de la validation, vérifiez la présence de la ligne correspondante dans la base de données.
4. Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Assurez-vous que le contexte utilisé pour la vérification a une stratégie d’exécution définie comme la connexion est susceptible d’échouer lors de la vérification en cas d’échec lors de la validation de la transaction.
