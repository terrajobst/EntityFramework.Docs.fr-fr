---
title: La résilience et nouvelle tentative logique de connexion - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997483"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="c48ff-102">Logique de résilience et de nouvelle tentative de connexion</span><span class="sxs-lookup"><span data-stu-id="c48ff-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="c48ff-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c48ff-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c48ff-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="c48ff-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c48ff-105">Applications se connectant à un serveur de base de données ont toujours été vulnérables aux sauts de connexion en raison de défaillances back-end et une instabilité du réseau.</span><span class="sxs-lookup"><span data-stu-id="c48ff-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="c48ff-106">Toutefois, dans un environnement de réseau local fonctionne avec les serveurs de base de données dédiée, ces erreurs sont assez rares pour une logique supplémentaire pour gérer ces échecs n’est pas souvent nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c48ff-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="c48ff-107">Avec l’essor du cloud en fonction des serveurs de base de données, tels que Windows Azure SQL Database et les connexions sur des réseaux moins fiables, qu'il est désormais plus courant pour les sauts de connexion.</span><span class="sxs-lookup"><span data-stu-id="c48ff-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="c48ff-108">Cela peut être dû à des techniques défensives qui permet de garantir l’équité de service, tels que les limitations de connexion ou d’instabilité du réseau qui provoque des délais d’attente intermittentes et autres erreurs temporaires de bases de données en nuage.</span><span class="sxs-lookup"><span data-stu-id="c48ff-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="c48ff-109">La résilience de connexion fait référence à la possibilité pour EF de réessayer automatiquement toutes les commandes qui échouent en raison de ces interruptions de connexion.</span><span class="sxs-lookup"><span data-stu-id="c48ff-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="c48ff-110">Stratégies d’exécution</span><span class="sxs-lookup"><span data-stu-id="c48ff-110">Execution Strategies</span></span>  

<span data-ttu-id="c48ff-111">Nouvelle tentative de connexion est pris en charge par une implémentation de l’interface IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="c48ff-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="c48ff-112">Les implémentations de la IDbExecutionStrategy sera responsables de l’acceptation d’une opération et, si une exception se produit, déterminer si une nouvelle tentative est appropriée et une nouvelle tentative s’il s’agit.</span><span class="sxs-lookup"><span data-stu-id="c48ff-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="c48ff-113">Il existe quatre stratégies d’exécution fournis avec Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="c48ff-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="c48ff-114">**DefaultExecutionStrategy**: cette stratégie d’exécution ne retente pas toutes les opérations, il est la valeur par défaut pour les bases de données autres que sql server.</span><span class="sxs-lookup"><span data-stu-id="c48ff-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="c48ff-115">**DefaultSqlExecutionStrategy**: il s’agit d’une stratégie d’exécution interne qui est utilisée par défaut.</span><span class="sxs-lookup"><span data-stu-id="c48ff-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="c48ff-116">Cette stratégie ne retente pas du tout, toutefois, il encapsule toutes les exceptions qui peut être transitoires à informer les utilisateurs qu’ils veulent peuvent activer la résilience des connexions.</span><span class="sxs-lookup"><span data-stu-id="c48ff-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="c48ff-117">**DbExecutionStrategy**: cette classe est appropriée comme classe de base pour les autres stratégies d’exécution, y compris vos propres modèles personnalisés.</span><span class="sxs-lookup"><span data-stu-id="c48ff-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="c48ff-118">Il implémente une stratégie de nouvelle tentative exponentielle, où la nouvelle tentative initiale produit avec zéro délai et le délai augmente exponentiellement jusqu'à ce que le nombre maximal de tentatives est atteint.</span><span class="sxs-lookup"><span data-stu-id="c48ff-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="c48ff-119">Cette classe possède une méthode abstraite ShouldRetryOn qui peut être implémentée dans les stratégies d’exécution dérivée pour contrôler les exceptions qui doivent être retentées.</span><span class="sxs-lookup"><span data-stu-id="c48ff-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="c48ff-120">**SqlAzureExecutionStrategy**: cette stratégie d’exécution hérite DbExecutionStrategy et réessaie sur les exceptions qui sont connues pour être éventuellement temporaires lorsque vous travaillez avec la base de données SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c48ff-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="c48ff-121">Stratégies d’exécution 2 et 4 sont inclus dans le fournisseur Sql Server qui est fourni avec Entity Framework, ce qui se trouve dans l’assembly EntityFramework.SqlServer et sont conçus pour fonctionner avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c48ff-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="c48ff-122">L’activation d’une stratégie d’exécution</span><span class="sxs-lookup"><span data-stu-id="c48ff-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="c48ff-123">Pour demander à EF d’utiliser une stratégie d’exécution le plus simple est de la méthode SetExecutionStrategy de la [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe :</span><span class="sxs-lookup"><span data-stu-id="c48ff-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="c48ff-124">Ce code indique à EF d’utiliser le SqlAzureExecutionStrategy lors de la connexion à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c48ff-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="c48ff-125">Configuration de la stratégie d’exécution</span><span class="sxs-lookup"><span data-stu-id="c48ff-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="c48ff-126">Le constructeur de SqlAzureExecutionStrategy peut accepter deux paramètres, MaxRetryCount et MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="c48ff-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="c48ff-127">Nombre de la valeur MaxRetry est le nombre maximal de tentatives de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="c48ff-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="c48ff-128">Le MaxDelay est un TimeSpan représentant le délai maximal entre les nouvelles tentatives qui utilise la stratégie d’exécution.</span><span class="sxs-lookup"><span data-stu-id="c48ff-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="c48ff-129">Pour définir le nombre maximal de nouvelles tentatives à 1 et le délai maximal de 30 secondes, vous devez execue les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c48ff-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="c48ff-130">Le SqlAzureExecutionStrategy réessaiera instantanément la première fois, une défaillance passagère se produit, mais plus retardera entre chaque tentative, jusqu'à ce que soit le nombre maximal de nombre limite de tentatives est dépassée ou le temps total atteint le délai maximal.</span><span class="sxs-lookup"><span data-stu-id="c48ff-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="c48ff-131">Les stratégies d’exécution va réessayer uniquement un nombre limité d’exceptions qui sont généralement tansient, vous devez toujours gérer les autres erreurs ainsi intercepter l’exception RetryLimitExceeded pour le cas où une erreur n’est pas temporaire ou prend trop de temps à résoudre lui-même.</span><span class="sxs-lookup"><span data-stu-id="c48ff-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="c48ff-132">Limitations</span><span class="sxs-lookup"><span data-stu-id="c48ff-132">Limitations</span></span>  

<span data-ttu-id="c48ff-133">Il existe certaines connus des limitations lorsque vous utilisez une stratégie d’exécution de nouvelle tentative :</span><span class="sxs-lookup"><span data-stu-id="c48ff-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="c48ff-134">Requêtes de diffusion en continu ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="c48ff-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="c48ff-135">Par défaut, EF6 et version ultérieure met en mémoire tampon les résultats de requête, plutôt que de diffusion en continu les.</span><span class="sxs-lookup"><span data-stu-id="c48ff-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="c48ff-136">Si vous souhaitez avoir des résultats transmis en continu vous pouvez utiliser la méthode AsStreaming pour modifier un LINQ pour interroger des entités à la diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="c48ff-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="c48ff-137">Diffusion en continu n’est pas pris en charge lorsqu’une stratégie d’exécution de nouvelle tentative est inscrit.</span><span class="sxs-lookup"><span data-stu-id="c48ff-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="c48ff-138">Cette limitation existe parce que la connexion peut supprimer la partie dans les résultats retournés.</span><span class="sxs-lookup"><span data-stu-id="c48ff-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="c48ff-139">Lorsque cela se produit, EF doit exécuter à nouveau la requête entière, mais n’a aucun moyen fiable de savoir ce qui se traduit ont déjà été retournés (données peuvent avoir changé depuis la requête initiale a été envoyée, résultats peuvent revenir dans un ordre différent, résultats ne peuvent pas avoir un identificateur unique etc..).</span><span class="sxs-lookup"><span data-stu-id="c48ff-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="c48ff-140">Transactions non prises en charge initiée par l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="c48ff-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="c48ff-141">Lorsque vous avez configuré une stratégie d’exécution qui résulte de nouvelles tentatives, il existe certaines limitations concernant l’utilisation de transactions.</span><span class="sxs-lookup"><span data-stu-id="c48ff-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="c48ff-142">Ce qui est pris en charge : comportement de transaction par défaut le d’Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c48ff-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="c48ff-143">Par défaut, Entity Framework effectue des mises à jour de la base de données dans une transaction.</span><span class="sxs-lookup"><span data-stu-id="c48ff-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="c48ff-144">Vous n’avez pas besoin de faire quelque chose pour ce faire, EF toujours fait automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c48ff-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="c48ff-145">Par exemple, dans le code suivant SaveChanges est effectuée automatiquement dans une transaction.</span><span class="sxs-lookup"><span data-stu-id="c48ff-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="c48ff-146">Si SaveChanges tombe en panne après insertion parmi le nouveau Site puis la transaction est restaurée et aucune modification appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c48ff-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="c48ff-147">Le contexte est également laissé dans un état qui permet de SaveChanges à appeler à nouveau pour appliquer les modifications de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="c48ff-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="c48ff-148">Ce qui n’est pas pris en charge : transactions initiée par l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="c48ff-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="c48ff-149">Lorsque vous N'utilisez pas une stratégie d’exécution de nouvelle tentative, vous pouvez encapsuler plusieurs opérations dans une transaction unique.</span><span class="sxs-lookup"><span data-stu-id="c48ff-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="c48ff-150">Par exemple, le code suivant encapsule les deux appels à SaveChanges dans une transaction unique.</span><span class="sxs-lookup"><span data-stu-id="c48ff-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="c48ff-151">Si n’importe quelle partie de l’opération échoue, aucune des modifications sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="c48ff-151">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="c48ff-152">Cela n’est pas pris en charge lors de l’utilisation d’une stratégie d’exécution de nouvelle tentative, car EF ne tient pas compte de toutes les opérations précédentes et comment les retenter.</span><span class="sxs-lookup"><span data-stu-id="c48ff-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="c48ff-153">Par exemple, si le deuxième SaveChanges a échoué puis EF n’est plus contient les informations requises pour le premier appel de SaveChanges de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="c48ff-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="c48ff-154">Solutions possibles</span><span class="sxs-lookup"><span data-stu-id="c48ff-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="c48ff-155">Suspendre la stratégie d’exécution</span><span class="sxs-lookup"><span data-stu-id="c48ff-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="c48ff-156">Une solution de contournement possible est de suspendre la stratégie d’exécution de nouvelle tentative pour la partie du code qui a besoin d’utiliser un utilisateur a lancé la transaction.</span><span class="sxs-lookup"><span data-stu-id="c48ff-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="c48ff-157">Pour ce faire, le plus simple consiste à ajouter un indicateur SuspendExecutionStrategy à votre code basé sur la classe de configuration et modifier l’expression lambda à la stratégie d’exécution pour retourner la stratégie d’exécution par défaut (non retying) lorsque l’indicateur est défini.</span><span class="sxs-lookup"><span data-stu-id="c48ff-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="c48ff-158">Notez que nous utilisons CallContext pour stocker la valeur d’indicateur.</span><span class="sxs-lookup"><span data-stu-id="c48ff-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="c48ff-159">Cela fournit une fonctionnalité similaire à un stockage local des threads, mais est déconseillé d’utiliser avec le code asynchrone - y compris la requête asynchrone et l’enregistrer avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c48ff-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="c48ff-160">Nous pouvons désormais suspendre la stratégie d’exécution pour la section de code qui utilise une transaction lancée par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c48ff-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="c48ff-161">Appeler manuellement la stratégie d’exécution</span><span class="sxs-lookup"><span data-stu-id="c48ff-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="c48ff-162">Une autre option consiste à utiliser la stratégie d’exécution manuellement et de lui donner l’ensemble de la logique à exécuter, afin qu’elle peut réessayer tout ce que si une des opérations échoue.</span><span class="sxs-lookup"><span data-stu-id="c48ff-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="c48ff-163">Nous avons besoin de suspendre la stratégie d’exécution - à l’aide de la technique ci-dessus - afin que les contextes utilisées à l’intérieur du bloc de code renouvelable n’essayez pas de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="c48ff-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="c48ff-164">Notez que les contextes doivent être construites dans le bloc de code, une nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="c48ff-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="c48ff-165">Cela garantit que nous avons commencé avec un état propre pour chaque nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="c48ff-165">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
