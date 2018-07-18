---
title: Utilisation de Transactions - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4238c88cc149458ed11b96a0bf9aaed9aac40b2d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121483"
---
# <a name="working-with-transactions"></a><span data-ttu-id="46fc0-102">Utilisation des Transactions</span><span class="sxs-lookup"><span data-stu-id="46fc0-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="46fc0-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="46fc0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="46fc0-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="46fc0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="46fc0-105">Ce document décrit l’utilisation de transactions dans EF6, y compris les améliorations que nous avons ajouté depuis EF5 afin de faciliter l’utilisation de transactions.</span><span class="sxs-lookup"><span data-stu-id="46fc0-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="46fc0-106">Ce que fait EF par défaut</span><span class="sxs-lookup"><span data-stu-id="46fc0-106">What EF does by default</span></span>  

<span data-ttu-id="46fc0-107">Dans toutes les versions d’Entity Framework, chaque fois que vous exécutez **SaveChanges()** à insérer, mettre à jour ou supprimer la base de données, le framework encapsule cette opération dans une transaction.</span><span class="sxs-lookup"><span data-stu-id="46fc0-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="46fc0-108">Cette opération dure uniquement suffisamment longtemps pour exécuter l’opération, puis termine.</span><span class="sxs-lookup"><span data-stu-id="46fc0-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="46fc0-109">Lorsque vous exécutez une autre opération de ce type, une nouvelle transaction est démarrée.</span><span class="sxs-lookup"><span data-stu-id="46fc0-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="46fc0-110">En commençant par EF6 **Database.ExecuteSqlCommand()** par défaut encapsule la commande dans une transaction si une n’était pas présente.</span><span class="sxs-lookup"><span data-stu-id="46fc0-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="46fc0-111">Il existe des surcharges de cette méthode qui vous permettent de remplacer ce comportement si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="46fc0-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="46fc0-112">Également dans l’exécution d’EF6 de procédures stockées incluses dans le modèle via des API telles que **ObjectContext.ExecuteFunction()** fait de même (sauf le comportement par défaut ne peut pas actuellement être substitué).</span><span class="sxs-lookup"><span data-stu-id="46fc0-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="46fc0-113">Dans les deux cas, le niveau d’isolation de la transaction est le niveau d’isolation le fournisseur de base de données considère que sa valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="46fc0-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="46fc0-114">Par défaut, par exemple, sur SQL Server CE est READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="46fc0-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="46fc0-115">Entity Framework n’encapsule pas les requêtes dans une transaction.</span><span class="sxs-lookup"><span data-stu-id="46fc0-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="46fc0-116">Cette fonctionnalité par défaut convient pour un grand nombre d’utilisateurs et s’il est donc pas nécessaire d’agir différemment dans EF6 ; écrivez simplement le code comme vous l’avez toujours fait.</span><span class="sxs-lookup"><span data-stu-id="46fc0-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="46fc0-117">Toutefois, certains utilisateurs ont besoin de mieux contrôler leurs transactions – ce sujet est traité dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="46fc0-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="46fc0-118">Fonctionnement de l’API</span><span class="sxs-lookup"><span data-stu-id="46fc0-118">How the APIs work</span></span>  

<span data-ttu-id="46fc0-119">Avant d’EF6 Entity Framework insisté sur l’ouverture de la connexion de base de données elle-même (il a levé une exception si elle a été passée à une connexion qui est déjà ouverte).</span><span class="sxs-lookup"><span data-stu-id="46fc0-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="46fc0-120">Dans la mesure où une transaction peut être démarrée uniquement sur une connexion ouverte, cela signifiait que la seule façon d’un utilisateur peut encapsuler plusieurs opérations en une seule transaction était d’utiliser un [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou utiliser le  **ObjectContext.Connection** propriété et l’appel de start **Open()** et **BeginTransaction()** directement sur retourné **EntityConnection** objet.</span><span class="sxs-lookup"><span data-stu-id="46fc0-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="46fc0-121">En outre, les appels d’API qui contacter la base de données échoue si vous aviez commencé une transaction sur la connexion de base de données sous-jacente à votre rythme.</span><span class="sxs-lookup"><span data-stu-id="46fc0-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="46fc0-122">La limitation d’accepter uniquement les connexions fermées a été supprimée dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="46fc0-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="46fc0-123">Pour plus d’informations, consultez [gestion des connexions](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="46fc0-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="46fc0-124">À partir de EF6 le framework maintenant fournit :</span><span class="sxs-lookup"><span data-stu-id="46fc0-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="46fc0-125">**Database.BeginTransaction()** : une méthode plus facile pour un utilisateur effectuer les transactions proprement dites au sein d’un DbContext existant – autorisant plusieurs opérations à être combinés au sein de la même transaction et par conséquent, soit toutes validées, soit toutes les restaurée en tant qu’un.</span><span class="sxs-lookup"><span data-stu-id="46fc0-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="46fc0-126">Il permet également de l’utilisateur de spécifier plus facilement le niveau d’isolation de la transaction.</span><span class="sxs-lookup"><span data-stu-id="46fc0-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="46fc0-127">**Database.UseTransaction()** : ce qui permet d’utiliser une transaction qui a été démarrée en dehors d’Entity Framework DbContext.</span><span class="sxs-lookup"><span data-stu-id="46fc0-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="46fc0-128">Combinaison de plusieurs opérations en une seule transaction dans le même contexte</span><span class="sxs-lookup"><span data-stu-id="46fc0-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="46fc0-129">**Database.BeginTransaction()** a deux remplacements : une qui prend une explicite [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) et l’autre qui n’accepte aucun argument et utilise la valeur par défaut IsolationLevel à partir du fournisseur de base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="46fc0-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="46fc0-130">Les deux remplacements retournent un **DbContextTransaction** objet qui fournit des **Commit()** et **Rollback()** méthodes qui effectuent la validation et restauration sur le magasin sous-jacent transaction.</span><span class="sxs-lookup"><span data-stu-id="46fc0-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="46fc0-131">Le **DbContextTransaction** est destiné à être supprimé une fois qu’il a été validée ou restaurée.</span><span class="sxs-lookup"><span data-stu-id="46fc0-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="46fc0-132">Un moyen facile de parvenir est la **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="46fc0-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="46fc0-133">syntaxe qui appelle automatiquement **Dispose()** lorsque du bloc est terminée :</span><span class="sxs-lookup"><span data-stu-id="46fc0-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="46fc0-134">Commencer une transaction requiert que la connexion à la banque sous-jacente soit ouverte.</span><span class="sxs-lookup"><span data-stu-id="46fc0-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="46fc0-135">Ainsi, l’appel Database.BeginTransaction() ouvrira la connexion si elle n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="46fc0-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="46fc0-136">Si DbContextTransaction ouvert la connexion puis il fermera il lorsque Dispose() est appelée.</span><span class="sxs-lookup"><span data-stu-id="46fc0-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="46fc0-137">En passant d’une transaction existante au contexte</span><span class="sxs-lookup"><span data-stu-id="46fc0-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="46fc0-138">Vous avez parfois besoin une transaction qui est encore plus large dans la portée et qui inclut des opérations sur la même base de données, mais en dehors d’Entity Framework complètement.</span><span class="sxs-lookup"><span data-stu-id="46fc0-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="46fc0-139">Pour ce faire, vous devez ouvrir la connexion et démarrer la transaction vous-même et puis demander à EF a) pour utiliser la connexion de base de données déjà ouvert et (b) utiliser la transaction existante sur cette connexion.</span><span class="sxs-lookup"><span data-stu-id="46fc0-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="46fc0-140">Pour ce faire, vous devez définir et utiliser un constructeur sur votre classe de contexte qui hérite d’un des constructeurs DbContext qui prennent i) d’un paramètre de connexion existant et ii) la contextOwnsConnection booléenne.</span><span class="sxs-lookup"><span data-stu-id="46fc0-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="46fc0-141">L’indicateur contextOwnsConnection doit être définie sur false lorsqu’elle est appelée dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="46fc0-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="46fc0-142">Ceci est important car il informe Entity Framework qu’il ne doit pas fermer la connexion lorsqu’il n’est plus (par exemple, voir la ligne 4 ci-dessous) :</span><span class="sxs-lookup"><span data-stu-id="46fc0-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="46fc0-143">En outre, vous devez démarrer la transaction vous-même (y compris la propriété IsolationLevel si vous souhaitez éviter le paramètre par défaut) et indiquer à Entity Framework qu’il existe une transaction existante déjà démarrée sur la connexion (voir la ligne 33 ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="46fc0-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="46fc0-144">Ensuite, vous êtes libre exécuter des opérations de base de données directement sur l’objet SqlConnection lui-même, soit sur le DbContext.</span><span class="sxs-lookup"><span data-stu-id="46fc0-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="46fc0-145">Toutes ces opérations sont exécutées au sein d’une seule transaction.</span><span class="sxs-lookup"><span data-stu-id="46fc0-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="46fc0-146">Vous prenez la responsabilité pour valider ou restaurer la transaction et pour appeler Dispose() sur celui-ci, ainsi que pour la fermeture et la suppression de la connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="46fc0-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="46fc0-147">Exemple :</span><span class="sxs-lookup"><span data-stu-id="46fc0-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="46fc0-148">Effacement de la transaction</span><span class="sxs-lookup"><span data-stu-id="46fc0-148">Clearing up the transaction</span></span>

<span data-ttu-id="46fc0-149">Vous pouvez passer null pour Database.UseTransaction() pour effacer les connaissances d’Entity Framework de la transaction actuelle.</span><span class="sxs-lookup"><span data-stu-id="46fc0-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="46fc0-150">Entity Framework sera ni validation ni restaurer la transaction existante lorsque vous effectuez cette opération, par conséquent, vous utilisez avec précaution et uniquement si vous ne savez Voici ce que vous voulez faire.</span><span class="sxs-lookup"><span data-stu-id="46fc0-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="46fc0-151">Erreurs dans UseTransaction</span><span class="sxs-lookup"><span data-stu-id="46fc0-151">Errors in UseTransaction</span></span>

<span data-ttu-id="46fc0-152">Vous verrez une exception à partir de Database.UseTransaction() si vous passez d’une transaction lorsque :</span><span class="sxs-lookup"><span data-stu-id="46fc0-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="46fc0-153">Entity Framework a déjà une transaction existante</span><span class="sxs-lookup"><span data-stu-id="46fc0-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="46fc0-154">Entity Framework est déjà opérant dans un TransactionScope</span><span class="sxs-lookup"><span data-stu-id="46fc0-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="46fc0-155">L’objet de connexion dans la transaction passée est null.</span><span class="sxs-lookup"><span data-stu-id="46fc0-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="46fc0-156">Autrement dit, la transaction n’est pas associée à une connexion : c’est généralement le signe que cette transaction est déjà terminée.</span><span class="sxs-lookup"><span data-stu-id="46fc0-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="46fc0-157">L’objet de connexion dans la transaction passée ne correspond pas à la connexion de d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="46fc0-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="46fc0-158">À l’aide de transactions avec d’autres fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="46fc0-158">Using transactions with other features</span></span>  

<span data-ttu-id="46fc0-159">Cette section détaille la façon dont les transactions ci-dessus interagissent avec :</span><span class="sxs-lookup"><span data-stu-id="46fc0-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="46fc0-160">Résilience de la connexion</span><span class="sxs-lookup"><span data-stu-id="46fc0-160">Connection resiliency</span></span>  
- <span data-ttu-id="46fc0-161">Méthodes asynchrones</span><span class="sxs-lookup"><span data-stu-id="46fc0-161">Asynchronous methods</span></span>  
- <span data-ttu-id="46fc0-162">Transactions de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="46fc0-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="46fc0-163">Résilience des connexions</span><span class="sxs-lookup"><span data-stu-id="46fc0-163">Connection Resiliency</span></span>  

<span data-ttu-id="46fc0-164">La nouvelle fonctionnalité de résilience de connexion ne fonctionne pas avec les transactions lancées par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="46fc0-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="46fc0-165">Pour plus d’informations, consultez [Limitations avec les stratégies d’exécution une nouvelle tentative](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span><span class="sxs-lookup"><span data-stu-id="46fc0-165">For details, see [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="46fc0-166">Programmation asynchrone</span><span class="sxs-lookup"><span data-stu-id="46fc0-166">Asynchronous Programming</span></span>  

<span data-ttu-id="46fc0-167">L’approche décrite dans les sections précédentes a besoin d’aucune options ou des paramètres supplémentaires pour travailler avec le [asynchrone interroge et enregistre les méthodes](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="46fc0-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="46fc0-168">Sachez toutefois que, selon que vous fassiez dans les méthodes asynchrones, cela peut entraîner la dans des transactions à long terme, ce qui peuvent à son tour entraîner des interblocages ou bloque le c'est-à-dire dégrade les performances de l’application globale.</span><span class="sxs-lookup"><span data-stu-id="46fc0-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="46fc0-169">Transactions de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="46fc0-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="46fc0-170">Avant EF6 la méthode recommandée consistant à fournir les transactions plus grande portée était d’utiliser un objet TransactionScope :</span><span class="sxs-lookup"><span data-stu-id="46fc0-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="46fc0-171">Le SqlConnection Entity Framework voulez-vous utiliser la transaction ambiante de TransactionScope et par conséquent, être validées ensemble.</span><span class="sxs-lookup"><span data-stu-id="46fc0-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="46fc0-172">En commençant par .NET 4.5.1 TransactionScope a été mis à jour pour fonctionner avec des méthodes asynchrones via l’utilisation de la [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) énumération :</span><span class="sxs-lookup"><span data-stu-id="46fc0-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="46fc0-173">Il existe encore quelques limitations à l’approche de TransactionScope :</span><span class="sxs-lookup"><span data-stu-id="46fc0-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="46fc0-174">Requiert .NET 4.5.1 ou version ultérieure pour travailler avec les méthodes asynchrones.</span><span class="sxs-lookup"><span data-stu-id="46fc0-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="46fc0-175">Il ne peut pas être utilisé dans les scénarios de cloud, sauf si vous êtes sûr que la connexion d’un seul et unique (scénarios de cloud ne prennent pas en charge les transactions distribuées).</span><span class="sxs-lookup"><span data-stu-id="46fc0-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="46fc0-176">Il ne peut pas être combiné avec l’approche Database.UseTransaction() des sections précédentes.</span><span class="sxs-lookup"><span data-stu-id="46fc0-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="46fc0-177">Elle lève des exceptions si vous émettez tout DDL et que vous n’avez pas activé les transactions distribuées via le Service MSDTC.</span><span class="sxs-lookup"><span data-stu-id="46fc0-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="46fc0-178">Avantages de l’approche de TransactionScope :</span><span class="sxs-lookup"><span data-stu-id="46fc0-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="46fc0-179">Il sera automatiquement mise à niveau une transaction locale en une transaction distribuée si vous rendre plus d’une connexion à une base de données ou combinez une connexion à une base de données avec une connexion à une autre base de données dans la même transaction (Remarque : vous devez avoir le service MSDTC configuré pour autoriser les transactions distribuées pour que cela fonctionne).</span><span class="sxs-lookup"><span data-stu-id="46fc0-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="46fc0-180">Facilité de codage.</span><span class="sxs-lookup"><span data-stu-id="46fc0-180">Ease of coding.</span></span> <span data-ttu-id="46fc0-181">Si vous préférez la transaction ambiante et traitées implicitement en arrière-plan au lieu de vous contrôlez explicitement sous ensuite l’approche de TransactionScope peut vous conviennent mieux.</span><span class="sxs-lookup"><span data-stu-id="46fc0-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="46fc0-182">En résumé, avec le nouveau Database.BeginTransaction() Database.UseTransaction() API ci-dessus, l’approche de TransactionScope n’est plus nécessaire pour la plupart des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="46fc0-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="46fc0-183">Si vous ne passez pas à utiliser TransactionScope être conscient des limitations ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="46fc0-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="46fc0-184">Nous vous recommandons d’utiliser l’approche décrite dans les sections précédentes, au lieu de cela lorsque cela est possible.</span><span class="sxs-lookup"><span data-stu-id="46fc0-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
