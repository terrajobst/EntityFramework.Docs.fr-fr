---
title: Gestion des échecs de validation de transaction - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
caps.latest.revision: 3
ms.openlocfilehash: 95ac69a005a70f31086bd4d3c0088bd3e82d28e2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121331"
---
# <a name="handling-transaction-commit-failures"></a>Gestion des échecs de validation de transaction
> [!NOTE]
> **EF6.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 6.1. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Dans le cadre de 6.1, nous vous présentons une nouvelle fonctionnalité de résilience de connexion pour Entity Framework : la possibilité de détecter et de récupérer automatiquement lorsque des échecs de connexion temporaires affectent l’accusé de réception de la transaction est validée. Des informations complètes sur le scénario sont mieux décrits dans le billet de blog [connectivité de base de données SQL et le problème de l’idempotence](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  En résumé, le scénario est que lorsqu’une exception est levée pendant une validation de transaction il existe deux causes possibles :  

1. Échec de la validation de transaction sur le serveur
2. La validation de transaction a réussi sur le serveur, mais un problème de connectivité a empêché la notification de réussite d’atteindre le client  

Quand le premier cas produit, l’application ou l’utilisateur pouvez retenter l’opération, mais lorsque le deuxième cas se produit nouvelles tentatives doivent être évitées et que l’application peut récupérer automatiquement. Le défi qui consiste sans la capacité à détecter quelle était la raison réelle, une exception a été signalée lors de la validation, l’application ne peut pas choisir le bon plan d’action. La nouvelle fonctionnalité dans EF 6.1 permet EF devrais vérifier de nouveau avec la base de données si la transaction a réussi et prendre en toute transparence de la ligne de conduite appropriée.  

## <a name="using-the-feature"></a>À l’aide de la fonctionnalité  

Afin d’activer la fonctionnalité, vous devez inclure un appel à [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) dans le constructeur de votre **DbConfiguration**. Si vous n’êtes pas familiarisé avec **DbConfiguration**, consultez [Configuration en fonction du Code](~/ef6/fundamentals/configuring/code-based.md). Cette fonctionnalité peut être utilisée en association avec les nouvelles tentatives automatiques qu'ont été introduits dans EF6, ce qui facilite la situation dans laquelle la transaction en fait la validation a échoué sur le serveur en raison d’une défaillance passagère :  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>Mode de suivi des transactions  

Lorsque la fonctionnalité est activée, EF ajoute automatiquement une nouvelle table à la base de données appelée **__Transactions**. Une nouvelle ligne est insérée dans cette table chaque fois qu’une transaction est créée par Entity Framework et l’existence de cette ligne est vérification si l’échec de la transaction se produit lors de la validation.  

Bien que EF fera de son mieux pour nettoyer les lignes de la table lorsqu’ils ne sont plus nécessaires, la table peut atteindre si l’application se termine prématurément et c’est pourquoi que vous devrez peut-être vider la table manuellement dans certains cas.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Comment gérer les échecs de validation avec les Versions précédentes

Avant d’EF 6.1 s’est pas mécanisme permettant de gérer les échecs de validation dans le produit d’EF. Il existe plusieurs façons de gérer cette situation peut être appliquée aux versions précédentes d’EF6 :  

### <a name="option-1---do-nothing"></a>Option 1 : ne rien faire  

La probabilité d’un échec de connexion lors de la validation de transaction est faible, elle peut être acceptable pour votre application de simplement échouer si cette condition se produit réellement.  

## <a name="option-2---use-the-database-to-reset-state"></a>Option 2 : utiliser la base de données pour réinitialiser l’état  

1. Ignorer le DbContext actuel  
2. Créer un nouveau DbContext et restaurer l’état de votre application à partir de la base de données  
3. Informer l’utilisateur que la dernière opération ne peut-être pas terminée avec succès  

## <a name="option-3---manually-track-the-transaction"></a>Option 3 - suivre manuellement la transaction  

1. Ajouter une table non suivies à la base de données utilisé pour suivre l’état des transactions.  
2. Insérer une ligne dans la table au début de chaque transaction.  
3. Si la connexion échoue lors de la validation, vérifier la présence de la ligne correspondante dans la base de données.  
    - Si la ligne est présente, se poursuivent normalement, car la transaction a été validée avec succès  
    - Si la ligne est absente, utilisez une stratégie d’exécution pour retenter l’opération actuelle.  
4. Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.  

[Ce billet de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contient des exemples de code pour accomplir cela sur SQL Azure.  
