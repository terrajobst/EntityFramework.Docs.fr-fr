---
title: Transactions - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>L’utilisation de Transactions

Les transactions permettent à plusieurs opérations de base de données à traiter de manière atomique. Si la transaction est validée, toutes les opérations sont appliquées avec succès à la base de données. Si la transaction est restaurée, aucune des opérations sont appliquées à la base de données.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) sur GitHub.

## <a name="default-transaction-behavior"></a>Comportement de transaction par défaut

Par défaut, si le fournisseur de base de données prend en charge les transactions, toutes les modifications dans un seul appel à `SaveChanges()` sont appliquées dans une transaction. Si des modifications échouent, puis la transaction est annulée et aucune des modifications sont appliquées à la base de données. Cela signifie que `SaveChanges()` est garanti complètement réussissent, ou laisser la base de données non modifiée si une erreur se produit.

Pour la plupart des applications, ce comportement par défaut est suffisant. Vous devez uniquement manuellement contrôler transactions si les exigences de votre application le jugent nécessaire.

## <a name="controlling-transactions"></a>Contrôle des transactions

Vous pouvez utiliser la `DbContext.Database` API pour commencer, valider et annuler les transactions. L’exemple suivant montre deux `SaveChanges()` LINQ et les opérations de requête en cours d’exécution dans une transaction unique.

Tous les fournisseurs de base de données prend en charge les transactions. Certains fournisseurs peuvent lever ou nulle lors de la transaction API sont appelées.

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

## <a name="cross-context-transaction-relational-databases-only"></a>Contexte de la transaction (bases de données relationnelles uniquement)

Vous pouvez également partager une transaction sur plusieurs instances de contexte. Cette fonctionnalité est disponible uniquement lorsque vous utilisez un fournisseur de base de données relationnelle, car elle requiert l’utilisation de `DbTransaction` et `DbConnection`, qui sont propres aux bases de données relationnelles.

Pour partager une transaction, les contextes doivent partager un `DbConnection` et un `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Autoriser la connexion doit être fourni en externe

Partage une `DbConnection` nécessite la possibilité de passer une connexion dans un contexte lors de la construction.

Le moyen le plus simple pour autoriser `DbConnection` doit être fourni en externe, consiste à arrêter d’utiliser le `DbContext.OnConfiguring` méthode pour configurer le contexte et en externe créer `DbContextOptions` et les passer au constructeur de contexte.

> [!TIP]  
> `DbContextOptionsBuilder`est de l’API que vous avez utilisé dans `DbContext.OnConfiguring` pour configurer le contexte, vous allez maintenant utiliser en externe pour créer `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

Une alternative consiste à continuer à utiliser `DbContext.OnConfiguring`, mais accepte un `DbConnection` qui est enregistré et ensuite utilisé dans `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Connexion de partage et de transaction

Vous pouvez désormais créer plusieurs instances de contexte qui partagent la même connexion. Utilisez ensuite le `DbContext.Database.UseTransaction(DbTransaction)` API pour inscrire les deux contextes dans la même transaction.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>À l’aide de DbTransactions externes (bases de données relationnelles uniquement)

Si vous utilisez plusieurs technologies d’accès aux données pour accéder à une base de données relationnelle, vous souhaiterez partagent une transaction entre les opérations effectuées par ces différentes technologies.

L’exemple suivant, montre comment effectuer une opération ADO.NET SqlClient et une opération d’Entity Framework Core dans la même transaction.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
