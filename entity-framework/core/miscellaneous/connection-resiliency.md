---
title: "Résilience des connexions - EF Core"
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
---
# <a name="connection-resiliency"></a><span data-ttu-id="fe967-102">Résilience des connexions</span><span class="sxs-lookup"><span data-stu-id="fe967-102">Connection Resiliency</span></span>

<span data-ttu-id="fe967-103">Résilience des connexions tente automatiquement les commandes de base de données a échoué.</span><span class="sxs-lookup"><span data-stu-id="fe967-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="fe967-104">La fonctionnalité peut être utilisée avec une base de données, vous devez fournir une « stratégie d’exécution », qui encapsule la logique nécessaire pour détecter les erreurs, puis réessayez de commandes.</span><span class="sxs-lookup"><span data-stu-id="fe967-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="fe967-105">Les fournisseurs de base EF peuvent fournir des stratégies d’exécution adaptées à leurs conditions d’échec spécifique de la base de données et les stratégies de nouvelle tentative optimal.</span><span class="sxs-lookup"><span data-stu-id="fe967-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="fe967-106">Par exemple, le fournisseur SQL Server inclut une stratégie d’exécution qui est conçu spécialement pour SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="fe967-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="fe967-107">Il connaît les types d’exception qui peuvent être retentées et a des valeurs par défaut pour le nombre maximal de tentatives, délai entre les nouvelles tentatives, etc.</span><span class="sxs-lookup"><span data-stu-id="fe967-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="fe967-108">Stratégie d’exécution est spécifiée lorsque vous configurez les options de votre contexte.</span><span class="sxs-lookup"><span data-stu-id="fe967-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="fe967-109">Il s’agit généralement dans le `OnConfiguring` méthode de votre contexte dérivée ou dans `Startup.cs` pour une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe967-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="fe967-110">Stratégie d’exécution personnalisés</span><span class="sxs-lookup"><span data-stu-id="fe967-110">Custom execution strategy</span></span>

<span data-ttu-id="fe967-111">Il existe un mécanisme d’enregistrement d’une stratégie d’exécution personnalisée de votre choix si vous souhaitez modifier les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="fe967-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="fe967-112">Les transactions et les stratégies d’exécution</span><span class="sxs-lookup"><span data-stu-id="fe967-112">Execution strategies and transactions</span></span>

<span data-ttu-id="fe967-113">Stratégie d’exécution tente automatiquement de défaillances doit être en mesure de lire chaque opération dans un bloc de nouvelle tentative échoue.</span><span class="sxs-lookup"><span data-stu-id="fe967-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="fe967-114">Lorsque de nouvelles tentatives sont activées, chaque opération que vous effectuez via EF Core devient son propre opération reproductible, autrement dit, chaque requête et chaque appel à `SaveChanges()` sera tentée à nouveau en tant qu’unité si une défaillance temporaire se produit.</span><span class="sxs-lookup"><span data-stu-id="fe967-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="fe967-115">Toutefois, si votre code initie une transaction à l’aide de `BeginTransaction()` vous définissez votre propre groupe d’opérations qui doivent être traités en tant qu’unité, autrement dit, tous les éléments à l’intérieur de la transaction devra être lu une défaillance intervient.</span><span class="sxs-lookup"><span data-stu-id="fe967-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="fe967-116">Vous recevez une exception semblable à ce qui suit si vous essayez d’effectuer lors de l’utilisation d’une stratégie d’exécution.</span><span class="sxs-lookup"><span data-stu-id="fe967-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="fe967-117">InvalidOperationException : La stratégie d’exécution configurée 'SqlServerRetryingExecutionStrategy' ne prend pas en charge les transactions démarrées par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fe967-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="fe967-118">Utilisez la stratégie d’exécution retournée par 'DbContext.Database.CreateExecutionStrategy()' pour exécuter toutes les opérations dans la transaction comme une unité reproductible.</span><span class="sxs-lookup"><span data-stu-id="fe967-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="fe967-119">La solution consiste à appeler manuellement la stratégie d’exécution avec un délégué qui représente tous les éléments qui doit être exécutée.</span><span class="sxs-lookup"><span data-stu-id="fe967-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="fe967-120">Si une défaillance temporaire se produit, la stratégie d’exécution appelle le délégué à nouveau.</span><span class="sxs-lookup"><span data-stu-id="fe967-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="fe967-121">Échec de validation de transaction et le problème idempotence</span><span class="sxs-lookup"><span data-stu-id="fe967-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="fe967-122">En général, lorsqu’un échec de connexion la transaction en cours est annulée.</span><span class="sxs-lookup"><span data-stu-id="fe967-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="fe967-123">Toutefois, si la connexion est perdue pendant la transaction étant validées résultant de la transaction de l’état est inconnu.</span><span class="sxs-lookup"><span data-stu-id="fe967-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="fe967-124">Consultez ce [billet de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="fe967-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="fe967-125">Par défaut, la stratégie d’exécution va retenter l’opération comme si la transaction a été restaurée, mais si elle n’est pas le cas cela entraîne une exception si le nouvel état de la base de données n’est pas compatible ou peut entraîner des **une altération des données** si le opération ne repose pas sur un état particulier, par exemple lors de l’insertion d’une nouvelle ligne avec les valeurs de clés générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="fe967-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="fe967-126">Il existe plusieurs façons de gérer cela.</span><span class="sxs-lookup"><span data-stu-id="fe967-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="fe967-127">Option 1 - (presque) nothing</span><span class="sxs-lookup"><span data-stu-id="fe967-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="fe967-128">La probabilité d’un échec de connexion lors de la validation de transaction est faible, il peut être acceptable pour votre application pour simplement de faire échouer si cette condition se produit réellement.</span><span class="sxs-lookup"><span data-stu-id="fe967-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="fe967-129">Toutefois, vous devez éviter d’utiliser des clés générées par le magasin afin de garantir qu’une exception est levée au lieu d’ajouter une ligne en double.</span><span class="sxs-lookup"><span data-stu-id="fe967-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="fe967-130">Envisagez d’utiliser une valeur GUID générée par le client ou un générateur de valeur du côté client.</span><span class="sxs-lookup"><span data-stu-id="fe967-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="fe967-131">Option 2 - état de l’application reconstruction</span><span class="sxs-lookup"><span data-stu-id="fe967-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="fe967-132">Ignorer l’actuel `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fe967-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="fe967-133">Créer un nouveau `DbContext` et restaurer l’état de votre application à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="fe967-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="fe967-134">Informer l’utilisateur que la dernière opération ne peut-être pas été correctement déroulée.</span><span class="sxs-lookup"><span data-stu-id="fe967-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="fe967-135">Option 3 - Vérification de l’état de l’ajout</span><span class="sxs-lookup"><span data-stu-id="fe967-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="fe967-136">Pour la plupart des opérations qui modifient l’état de la base de données, il est possible d’ajouter du code qui vérifie si elle a réussi.</span><span class="sxs-lookup"><span data-stu-id="fe967-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="fe967-137">EF fournit une méthode d’extension pour simplifier cette utilisation - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="fe967-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="fe967-138">Cette méthode commence et valide une transaction et accepte également une fonction dans le `verifySucceeded` paramètre qui est appelé lorsqu’une erreur temporaire se produit pendant la validation de transaction.</span><span class="sxs-lookup"><span data-stu-id="fe967-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="fe967-139">Ici `SaveChanges` est appelé avec `acceptAllChangesOnSuccess` la valeur `false` pour éviter de modifier l’état de la `Blog` entité `Unchanged` si `SaveChanges` réussit.</span><span class="sxs-lookup"><span data-stu-id="fe967-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="fe967-140">Cela permet de recommencer l’opération de même si la validation échoue et que la transaction est restaurée.</span><span class="sxs-lookup"><span data-stu-id="fe967-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="fe967-141">Option 4 : suivre manuellement la transaction</span><span class="sxs-lookup"><span data-stu-id="fe967-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="fe967-142">Si vous devez utiliser des clés générées par le magasin ou générique afin de gérer les erreurs de validation ne dépend de l’opération effectuée chaque transaction peut être attribuée à un ID qui est vérifié lors de la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="fe967-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="fe967-143">Ajouter une table à la base de données utilisé pour suivre l’état des transactions.</span><span class="sxs-lookup"><span data-stu-id="fe967-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="fe967-144">Insérer une ligne dans la table au début de chaque transaction.</span><span class="sxs-lookup"><span data-stu-id="fe967-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="fe967-145">Si la connexion échoue lors de la validation, vérifiez la présence de la ligne correspondante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="fe967-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="fe967-146">Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.</span><span class="sxs-lookup"><span data-stu-id="fe967-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="fe967-147">Assurez-vous que le contexte utilisé pour la vérification a une stratégie d’exécution définie comme la connexion est susceptible d’échouer lors de la vérification en cas d’échec lors de la validation de la transaction.</span><span class="sxs-lookup"><span data-stu-id="fe967-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
