---
title: Résilience des connexions - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 6d8cf117dfd94524a53e10bb4a23c2a44c4c8e7b
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459170"
---
# <a name="connection-resiliency"></a><span data-ttu-id="0cb6a-102">Résilience des connexions</span><span class="sxs-lookup"><span data-stu-id="0cb6a-102">Connection Resiliency</span></span>

<span data-ttu-id="0cb6a-103">Résilience des connexions retente automatiquement les commandes de base de données ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="0cb6a-104">La fonctionnalité peut être utilisée avec une base de données en fournissant une « stratégie d’exécution », qui encapsule la logique nécessaire pour détecter les erreurs et réessayez les commandes.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="0cb6a-105">Fournisseurs EF Core peuvent fournir des stratégies d’exécution adaptées à leurs conditions d’échec de la base de données spécifique et les stratégies de nouvelle tentative optimal.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="0cb6a-106">Par exemple, le fournisseur SQL Server inclut une stratégie d’exécution qui est conçu spécialement pour SQL Server (y compris SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="0cb6a-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="0cb6a-107">Il connaît les types d’exception qui peuvent être retentées et a des valeurs par défaut sensibles pour le nombre maximal de tentatives, délai entre les nouvelles tentatives, etc.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="0cb6a-108">Une stratégie d’exécution est spécifiée lorsque vous configurez les options pour votre contexte.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="0cb6a-109">Il s’agit généralement dans le `OnConfiguring` méthode de votre contexte dérivé :</span><span class="sxs-lookup"><span data-stu-id="0cb6a-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="0cb6a-110">ou dans `Startup.cs` pour une application ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="0cb6a-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="0cb6a-111">Stratégie d’exécution personnalisée</span><span class="sxs-lookup"><span data-stu-id="0cb6a-111">Custom execution strategy</span></span>

<span data-ttu-id="0cb6a-112">Il existe un mécanisme pour inscrire une stratégie d’exécution personnalisée de votre choix si vous souhaitez modifier les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="0cb6a-113">Transactions et les stratégies d’exécution</span><span class="sxs-lookup"><span data-stu-id="0cb6a-113">Execution strategies and transactions</span></span>

<span data-ttu-id="0cb6a-114">Une stratégie d’exécution qui réessaie automatiquement en cas d’échec doit être en mesure de lire chaque opération dans un bloc de nouvelle tentative échoue.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="0cb6a-115">Lorsque de nouvelles tentatives sont activées, chaque opération que vous effectuez par le biais de EF Core devient sa propre opération de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="0cb6a-116">Autrement dit, chaque requête et chaque appel à `SaveChanges()` va être retentée en tant qu’unité si une défaillance passagère se produit.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="0cb6a-117">Toutefois, si votre code lance une transaction à l’aide de `BeginTransaction()` vous définissez votre propre groupe d’opérations qui doivent être traités en tant qu’unité, et tous les éléments à l’intérieur de la transaction doit être lu une défaillance intervient.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="0cb6a-118">Vous recevrez une exception semblable à ce qui suit si vous essayez d’effectuer lors de l’utilisation d’une stratégie d’exécution :</span><span class="sxs-lookup"><span data-stu-id="0cb6a-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="0cb6a-119">InvalidOperationException: La stratégie d’exécution configurée « SqlServerRetryingExecutionStrategy » ne prend pas en charge les transactions lancées par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="0cb6a-120">Utilisez la stratégie d’exécution retournée par « DbContext.Database.CreateExecutionStrategy() » pour exécuter toutes les opérations de la transaction en tant qu’ensemble pouvant être retenté.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="0cb6a-121">La solution consiste à appeler manuellement la stratégie d’exécution avec un délégué représentant tout ce qui doit être exécutée.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="0cb6a-122">Si une défaillance passagère se produit, la stratégie d’exécution appelle à nouveau le délégué.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="0cb6a-123">Cette approche peut également être utilisée avec les transactions ambiantes.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="0cb6a-124">Échec de validation de transaction et le problème de l’idempotence</span><span class="sxs-lookup"><span data-stu-id="0cb6a-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="0cb6a-125">En règle générale, en cas d’échec de la connexion la transaction en cours est annulée.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="0cb6a-126">Toutefois, si la connexion est perdue pendant la transaction étant validées résultant état de la transaction est inconnu.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="0cb6a-127">Consultez ce [billet de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-127">See this [blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="0cb6a-128">Par défaut, la stratégie d’exécution va retenter l’opération comme si la transaction a été annulée, mais si elle n’est pas le cas cela entraîne une exception si le nouvel état de la base de données n’est pas compatible ou pourrait entraîner **une altération des données** si le opération ne repose pas sur un état particulier, par exemple lors de l’insertion d’une nouvelle ligne avec les valeurs de clé généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-128">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="0cb6a-129">Il existe plusieurs façons de gérer cela.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-129">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="0cb6a-130">Option 1 - (presque) nothing</span><span class="sxs-lookup"><span data-stu-id="0cb6a-130">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="0cb6a-131">La probabilité d’un échec de connexion lors de la validation de transaction est faible, elle peut être acceptable pour votre application de simplement échouer si cette condition se produit réellement.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-131">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="0cb6a-132">Toutefois, vous devez éviter d’utiliser des clés générées par le magasin afin de garantir qu’une exception est levée au lieu d’ajouter une ligne en double.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-132">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="0cb6a-133">Envisagez d’utiliser une valeur GUID générée par le client ou un générateur de valeur de côté client.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-133">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="0cb6a-134">Option 2 : état de l’application reconstruction</span><span class="sxs-lookup"><span data-stu-id="0cb6a-134">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="0cb6a-135">Ignorer actuel `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-135">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="0cb6a-136">Créer un nouveau `DbContext` et restaurer l’état de votre application à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-136">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="0cb6a-137">Informer l’utilisateur que la dernière opération ne peut-être pas terminée avec succès.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-137">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="0cb6a-138">Option 3 : ajouter la vérification de l’état</span><span class="sxs-lookup"><span data-stu-id="0cb6a-138">Option 3 - Add state verification</span></span>

<span data-ttu-id="0cb6a-139">Pour la plupart des opérations qui modifient l’état de la base de données, il est possible d’ajouter du code qui vérifie si elle a réussi.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-139">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="0cb6a-140">Entity Framework fournit une méthode d’extension pour simplifier ce processus - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-140">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="0cb6a-141">Cette méthode commence une transaction est validée et accepte également une fonction dans le `verifySucceeded` paramètre qui est appelé lorsqu’une erreur temporaire se produit pendant la validation de transaction.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-141">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="0cb6a-142">Ici `SaveChanges` est appelé avec `acceptAllChangesOnSuccess` définie sur `false` afin d’éviter la modification de l’état de la `Blog` entité à `Unchanged` si `SaveChanges` réussit.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-142">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="0cb6a-143">Cela permet de retenter l’opération même si la validation échoue et la transaction est restaurée.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-143">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="0cb6a-144">Option 4 : suivre manuellement la transaction</span><span class="sxs-lookup"><span data-stu-id="0cb6a-144">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="0cb6a-145">Si vous avez besoin d’utiliser des clés générées par le magasin ou besoin d’un moyen générique de gestion des échecs de validation qui ne dépend de l’opération effectuée chaque transaction peut être attribuée à un ID qui est vérifié lors de la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-145">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="0cb6a-146">Ajouter une table à la base de données utilisé pour suivre l’état des transactions.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-146">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="0cb6a-147">Insérer une ligne dans la table au début de chaque transaction.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-147">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="0cb6a-148">Si la connexion échoue lors de la validation, vérifier la présence de la ligne correspondante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-148">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="0cb6a-149">Si la validation est réussie, supprimez la ligne correspondante pour éviter la croissance de la table.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-149">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="0cb6a-150">Vérifiez que le contexte utilisé pour la vérification a une stratégie d’exécution définie comme la connexion est susceptible d’échouer lors de la vérification en cas d’échec lors de la validation de transaction.</span><span class="sxs-lookup"><span data-stu-id="0cb6a-150">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
