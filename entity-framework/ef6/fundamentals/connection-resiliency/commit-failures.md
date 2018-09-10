---
title: Gestion des échecs de validation de transaction - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250854"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="744f2-102">Gestion des échecs de validation de transaction</span><span class="sxs-lookup"><span data-stu-id="744f2-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="744f2-103">**EF6.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="744f2-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="744f2-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="744f2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="744f2-105">Dans le cadre de 6.1, nous vous présentons une nouvelle fonctionnalité de résilience de connexion pour Entity Framework : la possibilité de détecter et de récupérer automatiquement lorsque des échecs de connexion temporaires affectent l’accusé de réception de la transaction est validée.</span><span class="sxs-lookup"><span data-stu-id="744f2-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="744f2-106">Des informations complètes sur le scénario sont mieux décrits dans le billet de blog [connectivité de base de données SQL et le problème de l’idempotence](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="744f2-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="744f2-107">En résumé, le scénario est que lorsqu’une exception est levée pendant une validation de transaction il existe deux causes possibles :</span><span class="sxs-lookup"><span data-stu-id="744f2-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="744f2-108">Échec de la validation de transaction sur le serveur</span><span class="sxs-lookup"><span data-stu-id="744f2-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="744f2-109">La validation de transaction a réussi sur le serveur, mais un problème de connectivité a empêché la notification de réussite d’atteindre le client</span><span class="sxs-lookup"><span data-stu-id="744f2-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="744f2-110">Quand le premier cas produit, l’application ou l’utilisateur pouvez retenter l’opération, mais lorsque le deuxième cas se produit nouvelles tentatives doivent être évitées et que l’application peut récupérer automatiquement.</span><span class="sxs-lookup"><span data-stu-id="744f2-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="744f2-111">Le défi qui consiste sans la capacité à détecter quelle était la raison réelle, une exception a été signalée lors de la validation, l’application ne peut pas choisir le bon plan d’action.</span><span class="sxs-lookup"><span data-stu-id="744f2-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="744f2-112">La nouvelle fonctionnalité dans EF 6.1 permet EF devrais vérifier de nouveau avec la base de données si la transaction a réussi et prendre en toute transparence de la ligne de conduite appropriée.</span><span class="sxs-lookup"><span data-stu-id="744f2-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="744f2-113">À l’aide de la fonctionnalité</span><span class="sxs-lookup"><span data-stu-id="744f2-113">Using the feature</span></span>  

<span data-ttu-id="744f2-114">Afin d’activer la fonctionnalité, vous devez inclure un appel à [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) dans le constructeur de votre **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="744f2-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="744f2-115">Si vous n’êtes pas familiarisé avec **DbConfiguration**, consultez [Configuration en fonction du Code](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="744f2-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="744f2-116">Cette fonctionnalité peut être utilisée en association avec les nouvelles tentatives automatiques qu'ont été introduits dans EF6, ce qui facilite la situation dans laquelle la transaction en fait la validation a échoué sur le serveur en raison d’une défaillance passagère :</span><span class="sxs-lookup"><span data-stu-id="744f2-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="744f2-117">Mode de suivi des transactions</span><span class="sxs-lookup"><span data-stu-id="744f2-117">How transactions are tracked</span></span>  

<span data-ttu-id="744f2-118">Lorsque la fonctionnalité est activée, EF ajoute automatiquement une nouvelle table à la base de données appelée **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="744f2-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="744f2-119">Une nouvelle ligne est insérée dans cette table chaque fois qu’une transaction est créée par Entity Framework et l’existence de cette ligne est vérification si l’échec de la transaction se produit lors de la validation.</span><span class="sxs-lookup"><span data-stu-id="744f2-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="744f2-120">Bien que EF fera de son mieux pour nettoyer les lignes de la table lorsqu’ils ne sont plus nécessaires, la table peut atteindre si l’application se termine prématurément et c’est pourquoi que vous devrez peut-être vider la table manuellement dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="744f2-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="744f2-121">Comment gérer les échecs de validation avec les Versions précédentes</span><span class="sxs-lookup"><span data-stu-id="744f2-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="744f2-122">Avant d’EF 6.1 s’est pas mécanisme permettant de gérer les échecs de validation dans le produit d’EF.</span><span class="sxs-lookup"><span data-stu-id="744f2-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="744f2-123">Il existe plusieurs façons de gérer cette situation peut être appliquée aux versions précédentes d’EF6 :</span><span class="sxs-lookup"><span data-stu-id="744f2-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="744f2-124">Option 1 : ne rien faire</span><span class="sxs-lookup"><span data-stu-id="744f2-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="744f2-125">La probabilité d’un échec de connexion lors de la validation de transaction est faible, elle peut être acceptable pour votre application de simplement échouer si cette condition se produit réellement.</span><span class="sxs-lookup"><span data-stu-id="744f2-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="744f2-126">Option 2 : utiliser la base de données pour réinitialiser l’état</span><span class="sxs-lookup"><span data-stu-id="744f2-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="744f2-127">Ignorer le DbContext actuel</span><span class="sxs-lookup"><span data-stu-id="744f2-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="744f2-128">Créer un nouveau DbContext et restaurer l’état de votre application à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="744f2-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="744f2-129">Informer l’utilisateur que la dernière opération ne peut-être pas terminée avec succès</span><span class="sxs-lookup"><span data-stu-id="744f2-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="744f2-130">Option 3 - suivre manuellement la transaction</span><span class="sxs-lookup"><span data-stu-id="744f2-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="744f2-131">Ajouter une table non suivies à la base de données utilisé pour suivre l’état des transactions.</span><span class="sxs-lookup"><span data-stu-id="744f2-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="744f2-132">Insérer une ligne dans la table au début de chaque transaction.</span><span class="sxs-lookup"><span data-stu-id="744f2-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="744f2-133">Si la connexion échoue lors de la validation, vérifier la présence de la ligne correspondante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="744f2-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="744f2-134">Si la ligne est présente, se poursuivent normalement, car la transaction a été validée avec succès</span><span class="sxs-lookup"><span data-stu-id="744f2-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="744f2-135">Si la ligne est absente, utilisez une stratégie d’exécution pour retenter l’opération actuelle.</span><span class="sxs-lookup"><span data-stu-id="744f2-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="744f2-136">Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.</span><span class="sxs-lookup"><span data-stu-id="744f2-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="744f2-137">[Ce billet de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contient des exemples de code pour accomplir cela sur SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="744f2-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
