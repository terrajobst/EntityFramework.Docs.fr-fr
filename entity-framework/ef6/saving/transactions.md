---
title: Utilisation de Transactions - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 96cfff4cca59ab27dd68f50d0260e90902e33a92
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873237"
---
# <a name="working-with-transactions"></a>Utilisation des Transactions
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Ce document décrit l’utilisation de transactions dans EF6, y compris les améliorations que nous avons ajouté depuis EF5 afin de faciliter l’utilisation de transactions.  

## <a name="what-ef-does-by-default"></a>Ce que fait EF par défaut  

Dans toutes les versions d’Entity Framework, chaque fois que vous exécutez **SaveChanges()** à insérer, mettre à jour ou supprimer la base de données, le framework encapsule cette opération dans une transaction. Cette opération dure uniquement suffisamment longtemps pour exécuter l’opération, puis termine. Lorsque vous exécutez une autre opération de ce type, une nouvelle transaction est démarrée.  

En commençant par EF6 **Database.ExecuteSqlCommand()** par défaut encapsule la commande dans une transaction si une n’était pas présente. Il existe des surcharges de cette méthode qui vous permettent de remplacer ce comportement si vous le souhaitez. Également dans l’exécution d’EF6 de procédures stockées incluses dans le modèle via des API telles que **ObjectContext.ExecuteFunction()** fait de même (sauf le comportement par défaut ne peut pas actuellement être substitué).  

Dans les deux cas, le niveau d’isolation de la transaction est le niveau d’isolation le fournisseur de base de données considère que sa valeur par défaut. Par défaut, par exemple, sur SQL Server CE est READ COMMITTED.  

Entity Framework n’encapsule pas les requêtes dans une transaction.  

Cette fonctionnalité par défaut convient pour un grand nombre d’utilisateurs et s’il est donc pas nécessaire d’agir différemment dans EF6 ; écrivez simplement le code comme vous l’avez toujours fait.  

Toutefois, certains utilisateurs ont besoin de mieux contrôler leurs transactions – ce sujet est traité dans les sections suivantes.  

## <a name="how-the-apis-work"></a>Fonctionnement de l’API  

Avant d’EF6 Entity Framework insisté sur l’ouverture de la connexion de base de données elle-même (il a levé une exception si elle a été passée à une connexion qui est déjà ouverte). Dans la mesure où une transaction peut être démarrée uniquement sur une connexion ouverte, cela signifiait que la seule façon d’un utilisateur peut encapsuler plusieurs opérations en une seule transaction était d’utiliser un [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) ou utiliser le  **ObjectContext.Connection** propriété et l’appel de start **Open()** et **BeginTransaction()** directement sur retourné **EntityConnection** objet. En outre, les appels d’API qui contacter la base de données échoue si vous aviez commencé une transaction sur la connexion de base de données sous-jacente à votre rythme.  

> [!NOTE]
> La limitation d’accepter uniquement les connexions fermées a été supprimée dans Entity Framework 6. Pour plus d’informations, consultez [gestion des connexions](~/ef6/fundamentals/connection-management.md).  

À partir de EF6 le framework maintenant fournit :  

1. **Database.BeginTransaction()** : Une méthode plus facile pour un utilisateur effectuer les transactions proprement dites au sein d’un DbContext existant – autorisant plusieurs opérations à être combinés au sein de la même transaction et par conséquent, tous situés validée ou restaurée en tant qu’un. Il permet également de l’utilisateur de spécifier plus facilement le niveau d’isolation de la transaction.  
2. **Database.UseTransaction()** : ce qui permet d’utiliser une transaction qui a été démarrée en dehors d’Entity Framework DbContext.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Combinaison de plusieurs opérations en une seule transaction dans le même contexte  

**Database.BeginTransaction()** a deux remplacements : une qui prend une explicite [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) et l’autre qui n’accepte aucun argument et utilise la valeur par défaut IsolationLevel à partir du fournisseur de base de données sous-jacente. Les deux remplacements retournent un **DbContextTransaction** objet qui fournit des **Commit()** et **Rollback()** méthodes qui effectuent la validation et restauration sur le magasin sous-jacent transaction.  

Le **DbContextTransaction** est destiné à être supprimé une fois qu’il a été validée ou restaurée. Un moyen facile de parvenir est la **using(...) {...}** syntaxe qui appelle automatiquement **Dispose()** lorsque du bloc est terminée :  

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
> Commencer une transaction requiert que la connexion à la banque sous-jacente soit ouverte. Ainsi, l’appel Database.BeginTransaction() ouvrira la connexion si elle n’est pas déjà ouvert. Si DbContextTransaction ouvert la connexion puis il fermera il lorsque Dispose() est appelée.  

### <a name="passing-an-existing-transaction-to-the-context"></a>En passant d’une transaction existante au contexte  

Vous avez parfois besoin une transaction qui est encore plus large dans la portée et qui inclut des opérations sur la même base de données, mais en dehors d’Entity Framework complètement. Pour ce faire, vous devez ouvrir la connexion et démarrer la transaction vous-même et puis demander à EF a) pour utiliser la connexion de base de données déjà ouvert et (b) utiliser la transaction existante sur cette connexion.  

Pour ce faire, vous devez définir et utiliser un constructeur sur votre classe de contexte qui hérite d’un des constructeurs DbContext qui prennent i) d’un paramètre de connexion existant et ii) la contextOwnsConnection booléenne.  

> [!NOTE]
> L’indicateur contextOwnsConnection doit être définie sur false lorsqu’elle est appelée dans ce scénario. Ceci est important car il informe Entity Framework qu’il ne doit pas fermer la connexion lorsqu’il n’est plus (par exemple, voir la ligne 4 ci-dessous) :  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

En outre, vous devez démarrer la transaction vous-même (y compris la propriété IsolationLevel si vous souhaitez éviter le paramètre par défaut) et indiquer à Entity Framework qu’il existe une transaction existante déjà démarrée sur la connexion (voir la ligne 33 ci-dessous).  

Ensuite, vous êtes libre exécuter des opérations de base de données directement sur l’objet SqlConnection lui-même, soit sur le DbContext. Toutes ces opérations sont exécutées au sein d’une seule transaction. Vous prenez la responsabilité pour valider ou restaurer la transaction et pour appeler Dispose() sur celui-ci, ainsi que pour la fermeture et la suppression de la connexion de base de données. Exemple :  

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

### <a name="clearing-up-the-transaction"></a>Effacement de la transaction

Vous pouvez passer null pour Database.UseTransaction() pour effacer les connaissances d’Entity Framework de la transaction actuelle. Entity Framework sera ni validation ni restaurer la transaction existante lorsque vous effectuez cette opération, par conséquent, vous utilisez avec précaution et uniquement si vous ne savez Voici ce que vous voulez faire.  

### <a name="errors-in-usetransaction"></a>Erreurs dans UseTransaction

Vous verrez une exception à partir de Database.UseTransaction() si vous passez d’une transaction lorsque :  
- Entity Framework a déjà une transaction existante  
- Entity Framework est déjà opérant dans un TransactionScope  
- L’objet de connexion dans la transaction passée est null. Autrement dit, la transaction n’est pas associée à une connexion : c’est généralement le signe que cette transaction est déjà terminée.  
- L’objet de connexion dans la transaction passée ne correspond pas à la connexion de d’Entity Framework.  

## <a name="using-transactions-with-other-features"></a>À l’aide de transactions avec d’autres fonctionnalités  

Cette section détaille la façon dont les transactions ci-dessus interagissent avec :  

- Résilience de la connexion  
- Méthodes asynchrones  
- Transactions de TransactionScope  

### <a name="connection-resiliency"></a>Résilience des connexions  

La nouvelle fonctionnalité de résilience de connexion ne fonctionne pas avec les transactions lancées par l’utilisateur. Pour plus d’informations, consultez [stratégies une nouvelle tentative d’exécution](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programmation asynchrone  

L’approche décrite dans les sections précédentes a besoin d’aucune options ou des paramètres supplémentaires pour travailler avec le [asynchrone interroge et enregistre les méthodes](~/ef6/fundamentals/async.md
). Sachez toutefois que, selon que vous fassiez dans les méthodes asynchrones, cela peut entraîner la dans des transactions à long terme, ce qui peuvent à son tour entraîner des interblocages ou bloque le c'est-à-dire dégrade les performances de l’application globale.  

### <a name="transactionscope-transactions"></a>Transactions de TransactionScope  

Avant EF6 la méthode recommandée consistant à fournir les transactions plus grande portée était d’utiliser un objet TransactionScope :  

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

Le SqlConnection Entity Framework voulez-vous utiliser la transaction ambiante de TransactionScope et par conséquent, être validées ensemble.  

En commençant par .NET 4.5.1 TransactionScope a été mis à jour pour fonctionner avec des méthodes asynchrones via l’utilisation de la [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) énumération :  

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

Il existe encore quelques limitations à l’approche de TransactionScope :  

- Requiert .NET 4.5.1 ou version ultérieure pour travailler avec les méthodes asynchrones.  
- Il ne peut pas être utilisé dans les scénarios de cloud, sauf si vous êtes sûr que la connexion d’un seul et unique (scénarios de cloud ne prennent pas en charge les transactions distribuées).  
- Il ne peut pas être combiné avec l’approche Database.UseTransaction() des sections précédentes.  
- Elle lève des exceptions si vous émettez tout DDL et que vous n’avez pas activé les transactions distribuées via le Service MSDTC.  

Avantages de l’approche de TransactionScope :  

- Il sera automatiquement mise à niveau une transaction locale en une transaction distribuée si vous rendre plus d’une connexion à une base de données ou combinez une connexion à une base de données avec une connexion à une autre base de données dans la même transaction (Remarque : vous devez avoir le service MSDTC configuré pour autoriser les transactions distribuées pour que cela fonctionne).  
- Facilité de codage. Si vous préférez la transaction ambiante et traitées implicitement en arrière-plan au lieu de vous contrôlez explicitement sous ensuite l’approche de TransactionScope peut vous conviennent mieux.  

En résumé, avec le nouveau Database.BeginTransaction() Database.UseTransaction() API ci-dessus, l’approche de TransactionScope n’est plus nécessaire pour la plupart des utilisateurs. Si vous ne passez pas à utiliser TransactionScope être conscient des limitations ci-dessus. Nous vous recommandons d’utiliser l’approche décrite dans les sections précédentes, au lieu de cela lorsque cela est possible.  
