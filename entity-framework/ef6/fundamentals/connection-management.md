---
title: Gestion des connexions - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489334"
---
# <a name="connection-management"></a>Gestion des connexions
Cette page décrit le comportement d’Entity Framework en ce qui concerne le passage de connexions pour le contexte et les fonctionnalités de la **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Connexions passant au contexte  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportement de EF5 et versions antérieures  

Il existe deux constructeurs qui acceptent les connexions :  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Il est possible de les utiliser, mais vous devez contourner quelques limitations :  

1. Si vous passez une connexion ouverte à une de ces puis la première fois que l’infrastructure essaie d’utiliser qu'une exception InvalidOperationException est levée indiquant que le programme ne peut pas rouvrir une connexion déjà ouverte.  
2. L’indicateur contextOwnsConnection est interprété comme si la connexion à la banque sous-jacente doit être supprimée lorsque le contexte est supprimé ou non. Toutefois, indépendamment de ce paramètre, le magasin de connexion est toujours fermé lorsque le contexte est supprimé. Donc si vous avez plusieurs DbContext avec la même connexion quelle que soit la contexte est supprimé tout d’abord fermera la connexion (même si vous avez mélangé une connexion ADO.NET existante avec un DbContext, DbContext sera toujours fermer la connexion lorsqu’il est supprimé) .  

Il est possible de contourner la limitation première ci-dessus en passant d’une connexion fermée et seulement l’exécution de code qui ouvrirait une fois que tous les contextes ont été créés :  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

La deuxième limite signifie simplement que vous devez éviter de supprimer un de vos objets DbContext jusqu'à ce que vous êtes prêt pour la connexion à fermer.  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportement dans EF6 et versions ultérieures  

Dans EF6 et les versions futures DbContext a les deux constructeurs mêmes mais ne nécessite plus que la connexion passée au constructeur fermée lorsqu’elle est reçue. Par conséquent, vous pouvez désormais :  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

L’indicateur contextOwnsConnection maintenant contrôle également ou non la connexion est fermée et supprimée lorsque la classe DbContext est supprimé. Donc dans l’exemple ci-dessus la connexion n’est pas fermée lorsque le contexte est supprimé (ligne 32) comme il aurait été dans les versions précédentes d’EF, mais lors de la connexion elle-même soit libérée (ligne 40).  

Bien sûr, il est toujours possible pour la classe DbContext prendre le contrôle de la connexion (simplement ensemble contextOwnsConnection sur true ou utilisez un des autres constructeurs) si vous le souhaitez.  

> [!NOTE]
> Il existe certaines considérations supplémentaires lors de l’utilisation de transactions avec ce nouveau modèle. Pour plus d’informations, consultez [utilisation de Transactions](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportement de EF5 et versions antérieures  

Dans EF5 et versions antérieures, il existe un bogue telles que la **ObjectContext.Connection.State** n’a pas mis à jour pour refléter l’état réel de la connexion à la banque sous-jacente. Par exemple, si vous avez exécuté le code suivant vous pouvez affichera l’état **fermé** même si en fait sous-jacent connexion à la banque est **Open**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Séparément, si vous ouvrez la connexion de base de données en appelant Database.Connection.Open() il sera ouverte jusqu'à ce que la prochaine fois que vous exécutez une requête ou appeler tout ce qui requiert une connexion de base de données (par exemple, SaveChanges()) mais après que sous-jacent stocke connexion va être fermée. Le contexte sera puis ouvrez à nouveau et ré-fermer la connexion chaque fois qu’une autre opération de base de données est requise :  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportement dans EF6 et versions ultérieures  

Pour EF6 et les versions futures nous avons adopté l’approche que si le code appelant choisit d’ouvrir la connexion par le contexte d’appel. Database.Connection.Open(), il a une bonne raison de le faire et le framework suppose qu’il souhaite contrôler ouverture et fermeture de la connexion et n’est plus automatiquement ferme la connexion.  

> [!NOTE]
> Cela peut potentiellement entraîner aux connexions qui sont ouverts pour un certain temps, par conséquent, utilisez avec précaution.  

Nous avons également mis à jour le code afin que ObjectContext.Connection.State maintenant effectue le suivi de l’état de la connexion sous-jacente correctement.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
