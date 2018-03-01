---
title: Transactions - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="6e8e5-102">L’utilisation de Transactions</span><span class="sxs-lookup"><span data-stu-id="6e8e5-102">Using Transactions</span></span>

<span data-ttu-id="6e8e5-103">Les transactions permettent à plusieurs opérations de base de données à traiter de manière atomique.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="6e8e5-104">Si la transaction est validée, toutes les opérations sont appliquées avec succès à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="6e8e5-105">Si la transaction est restaurée, aucune des opérations sont appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="6e8e5-106">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="6e8e5-107">Comportement de transaction par défaut</span><span class="sxs-lookup"><span data-stu-id="6e8e5-107">Default transaction behavior</span></span>

<span data-ttu-id="6e8e5-108">Par défaut, si le fournisseur de base de données prend en charge les transactions, toutes les modifications dans un seul appel à `SaveChanges()` sont appliquées dans une transaction.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="6e8e5-109">Si des modifications échouent, puis la transaction est annulée et aucune des modifications sont appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="6e8e5-110">Cela signifie que `SaveChanges()` est garanti complètement réussissent, ou laisser la base de données non modifiée si une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="6e8e5-111">Pour la plupart des applications, ce comportement par défaut est suffisant.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="6e8e5-112">Vous devez uniquement manuellement contrôler transactions si les exigences de votre application le jugent nécessaire.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="6e8e5-113">Contrôle des transactions</span><span class="sxs-lookup"><span data-stu-id="6e8e5-113">Controlling transactions</span></span>

<span data-ttu-id="6e8e5-114">Vous pouvez utiliser la `DbContext.Database` API pour commencer, valider et annuler les transactions.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="6e8e5-115">L’exemple suivant montre deux `SaveChanges()` LINQ et les opérations de requête en cours d’exécution dans une transaction unique.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="6e8e5-116">Tous les fournisseurs de base de données prend en charge les transactions.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-116">Not all database providers support transactions.</span></span> <span data-ttu-id="6e8e5-117">Certains fournisseurs peuvent lever ou nulle lors de la transaction API sont appelées.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="6e8e5-118">Contexte de la transaction (bases de données relationnelles uniquement)</span><span class="sxs-lookup"><span data-stu-id="6e8e5-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="6e8e5-119">Vous pouvez également partager une transaction sur plusieurs instances de contexte.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="6e8e5-120">Cette fonctionnalité est disponible uniquement lorsque vous utilisez un fournisseur de base de données relationnelle, car elle requiert l’utilisation de `DbTransaction` et `DbConnection`, qui sont propres aux bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="6e8e5-121">Pour partager une transaction, les contextes doivent partager un `DbConnection` et un `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="6e8e5-122">Autoriser la connexion doit être fourni en externe</span><span class="sxs-lookup"><span data-stu-id="6e8e5-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="6e8e5-123">Partage une `DbConnection` nécessite la possibilité de passer une connexion dans un contexte lors de la construction.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="6e8e5-124">Le moyen le plus simple pour autoriser `DbConnection` doit être fourni en externe, consiste à arrêter d’utiliser le `DbContext.OnConfiguring` méthode pour configurer le contexte et en externe créer `DbContextOptions` et les passer au constructeur de contexte.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="6e8e5-125">`DbContextOptionsBuilder` est de l’API que vous avez utilisé dans `DbContext.OnConfiguring` pour configurer le contexte, vous allez maintenant utiliser en externe pour créer `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="6e8e5-126">Une alternative consiste à continuer à utiliser `DbContext.OnConfiguring`, mais accepte un `DbConnection` qui est enregistré et ensuite utilisé dans `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="6e8e5-127">Connexion de partage et de transaction</span><span class="sxs-lookup"><span data-stu-id="6e8e5-127">Share connection and transaction</span></span>

<span data-ttu-id="6e8e5-128">Vous pouvez désormais créer plusieurs instances de contexte qui partagent la même connexion.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="6e8e5-129">Utilisez ensuite le `DbContext.Database.UseTransaction(DbTransaction)` API pour inscrire les deux contextes dans la même transaction.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="6e8e5-130">À l’aide de DbTransactions externes (bases de données relationnelles uniquement)</span><span class="sxs-lookup"><span data-stu-id="6e8e5-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="6e8e5-131">Si vous utilisez plusieurs technologies d’accès aux données pour accéder à une base de données relationnelle, vous souhaiterez partagent une transaction entre les opérations effectuées par ces différentes technologies.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="6e8e5-132">L’exemple suivant, montre comment effectuer une opération ADO.NET SqlClient et une opération d’Entity Framework Core dans la même transaction.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="6e8e5-133">Utilisation de System.Transactions</span><span class="sxs-lookup"><span data-stu-id="6e8e5-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="6e8e5-134">Cette fonctionnalité est une nouveauté dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="6e8e5-135">Il est possible d’utiliser des transactions ambiantes si vous avez besoin coordonner sur une plus grande portée.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="6e8e5-136">Il est également possible de s’inscrire dans une transaction explicite.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="6e8e5-137">Limitations de System.Transactions</span><span class="sxs-lookup"><span data-stu-id="6e8e5-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="6e8e5-138">EF Core s’appuie sur les fournisseurs de base de données pour implémenter la prise en charge de System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="6e8e5-139">Bien que la prise en charge est assez courant parmi les fournisseurs ADO.NET de .NET Framework, l’API a été récemment ajoutés uniquement au .NET Core et par conséquent pas être répandu.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="6e8e5-140">Si un fournisseur n’implémente pas la prise en charge de System.Transactions, il est possible que les appels à ces API sont complètement ignorés.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="6e8e5-141">SqlClient pour .NET Core prend en charge il 2.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="6e8e5-142">SqlClient pour .NET Core 2.0 lève une exception de tentative d’utilisation de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="6e8e5-143">Il est recommandé de tester que l’API se comporte correctement avec votre fournisseur avant dépendent de la gestion des transactions.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="6e8e5-144">Vous êtes invité à contacter le chargé de maintenance du fournisseur de base de données si elle n’est pas le cas.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="6e8e5-145">À compter de la version 2.1, l’implémentation de System.Transactions dans .NET Core n’inclut pas de prise en charge des transactions distribuées, par conséquent, vous ne pouvez pas utiliser `TransactionScope` ou `CommitableTransaction`pour coordonner des transactions sur plusieurs gestionnaires de ressources.</span><span class="sxs-lookup"><span data-stu-id="6e8e5-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction`to coordinate transactions across multiple resource managers.</span></span> 
