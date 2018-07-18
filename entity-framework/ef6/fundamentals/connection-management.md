---
title: Gestion des connexions - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121471"
---
# <a name="connection-management"></a><span data-ttu-id="326c9-102">Gestion des connexions</span><span class="sxs-lookup"><span data-stu-id="326c9-102">Connection management</span></span>
<span data-ttu-id="326c9-103">Cette page décrit le comportement d’Entity Framework en ce qui concerne le passage de connexions pour le contexte et les fonctionnalités de la **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="326c9-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="326c9-104">Connexions passant au contexte</span><span class="sxs-lookup"><span data-stu-id="326c9-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="326c9-105">Comportement de EF5 et versions antérieures</span><span class="sxs-lookup"><span data-stu-id="326c9-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="326c9-106">Il existe deux constructeurs qui acceptent les connexions :</span><span class="sxs-lookup"><span data-stu-id="326c9-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="326c9-107">Il est possible de les utiliser, mais vous devez contourner quelques limitations :</span><span class="sxs-lookup"><span data-stu-id="326c9-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="326c9-108">Si vous passez une connexion ouverte à une de ces puis la première fois que l’infrastructure essaie d’utiliser qu'une exception InvalidOperationException est levée indiquant que le programme ne peut pas rouvrir une connexion déjà ouverte.</span><span class="sxs-lookup"><span data-stu-id="326c9-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="326c9-109">L’indicateur contextOwnsConnection est interprété comme si la connexion à la banque sous-jacente doit être supprimée lorsque le contexte est supprimé ou non.</span><span class="sxs-lookup"><span data-stu-id="326c9-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="326c9-110">Toutefois, indépendamment de ce paramètre, le magasin de connexion est toujours fermé lorsque le contexte est supprimé.</span><span class="sxs-lookup"><span data-stu-id="326c9-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="326c9-111">Donc si vous avez plusieurs DbContext avec la même connexion quelle que soit la contexte est supprimé tout d’abord fermera la connexion (même si vous avez mélangé une connexion ADO.NET existante avec un DbContext, DbContext sera toujours fermer la connexion lorsqu’il est supprimé) .</span><span class="sxs-lookup"><span data-stu-id="326c9-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="326c9-112">Il est possible de contourner la limitation première ci-dessus en passant d’une connexion fermée et seulement l’exécution de code qui ouvrirait une fois que tous les contextes ont été créés :</span><span class="sxs-lookup"><span data-stu-id="326c9-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="326c9-113">La deuxième limite signifie simplement que vous devez éviter de supprimer un de vos objets DbContext jusqu'à ce que vous êtes prêt pour la connexion à fermer.</span><span class="sxs-lookup"><span data-stu-id="326c9-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="326c9-114">Comportement dans EF6 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="326c9-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="326c9-115">Dans EF6 et les versions futures DbContext a les deux constructeurs mêmes mais ne nécessite plus que la connexion passée au constructeur fermée lorsqu’elle est reçue.</span><span class="sxs-lookup"><span data-stu-id="326c9-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="326c9-116">Par conséquent, vous pouvez désormais :</span><span class="sxs-lookup"><span data-stu-id="326c9-116">So this is now possible:</span></span>  

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

<span data-ttu-id="326c9-117">L’indicateur contextOwnsConnection maintenant contrôle également ou non la connexion est fermée et supprimée lorsque la classe DbContext est supprimé.</span><span class="sxs-lookup"><span data-stu-id="326c9-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="326c9-118">Donc dans l’exemple ci-dessus la connexion n’est pas fermée lorsque le contexte est supprimé (ligne 32) comme il aurait été dans les versions précédentes d’EF, mais lors de la connexion elle-même soit libérée (ligne 40).</span><span class="sxs-lookup"><span data-stu-id="326c9-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="326c9-119">Bien sûr, il est toujours possible pour la classe DbContext prendre le contrôle de la connexion (simplement ensemble contextOwnsConnection sur true ou utilisez un des autres constructeurs) si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="326c9-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="326c9-120">Il existe certaines considérations supplémentaires lors de l’utilisation de transactions avec ce nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="326c9-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="326c9-121">Pour plus d’informations, consultez [utilisation de Transactions](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="326c9-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="326c9-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="326c9-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="326c9-123">Comportement de EF5 et versions antérieures</span><span class="sxs-lookup"><span data-stu-id="326c9-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="326c9-124">Dans EF5 et versions antérieures, il existe un bogue telles que la **ObjectContext.Connection.State** n’a pas mis à jour pour refléter l’état réel de la connexion à la banque sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="326c9-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="326c9-125">Par exemple, si vous avez exécuté le code suivant vous pouvez affichera l’état **fermé** même si en fait sous-jacent connexion à la banque est **Open**.</span><span class="sxs-lookup"><span data-stu-id="326c9-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="326c9-126">Séparément, si vous ouvrez la connexion de base de données en appelant Database.Connection.Open() il sera ouverte jusqu'à ce que la prochaine fois que vous exécutez une requête ou appeler tout ce qui requiert une connexion de base de données (par exemple, SaveChanges()) mais après que sous-jacent stocke connexion va être fermée.</span><span class="sxs-lookup"><span data-stu-id="326c9-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="326c9-127">Le contexte sera puis ouvrez à nouveau et ré-fermer la connexion chaque fois qu’une autre opération de base de données est requise :</span><span class="sxs-lookup"><span data-stu-id="326c9-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="326c9-128">Comportement dans EF6 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="326c9-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="326c9-129">Pour EF6 et les versions futures nous avons adopté l’approche que si le code appelant choisit d’ouvrir la connexion par le contexte d’appel. Database.Connection.Open(), il a une bonne raison de le faire et le framework suppose qu’il souhaite contrôler ouverture et fermeture de la connexion et n’est plus automatiquement ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="326c9-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="326c9-130">Cela peut potentiellement entraîner aux connexions qui sont ouverts pour un certain temps, par conséquent, utilisez avec précaution.</span><span class="sxs-lookup"><span data-stu-id="326c9-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="326c9-131">Nous avons également mis à jour le code afin que ObjectContext.Connection.State maintenant effectue le suivi de l’état de la connexion sous-jacente correctement.</span><span class="sxs-lookup"><span data-stu-id="326c9-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
