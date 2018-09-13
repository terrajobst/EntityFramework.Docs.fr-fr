---
title: Async interroge et enregistre - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: a6f49f3c31601ab04a3c04c074ce8fddfc6fe301
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489685"
---
# <a name="async-query-and-save"></a><span data-ttu-id="827cb-102">Async interroger et enregistrer</span><span class="sxs-lookup"><span data-stu-id="827cb-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="827cb-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="827cb-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="827cb-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="827cb-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="827cb-105">EF6 a introduit la prise en charge pour une requête asynchrone et enregistrer à l’aide de la [async et await mots clés](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) qui ont été introduites dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="827cb-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="827cb-106">Alors que toutes les applications ne puissent bénéficier de l’asynchronie, il peut être utilisé pour améliorer l’extensibilité de la réactivité et de serveur client lors de la gestion des longues, de réseau ou de tâches d’e/S.</span><span class="sxs-lookup"><span data-stu-id="827cb-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="827cb-107">Quand utiliser réellement async</span><span class="sxs-lookup"><span data-stu-id="827cb-107">When to really use async</span></span>

<span data-ttu-id="827cb-108">L’objectif de cette procédure pas à pas consiste à présenter les concepts asynchrone d’une manière qui rend facile à observer la différence entre l’exécution du programme synchrone et asynchrone.</span><span class="sxs-lookup"><span data-stu-id="827cb-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="827cb-109">Cette procédure pas à pas ne vise pas pour illustrer un des principaux scénarios où la programmation asynchrone fournit des avantages.</span><span class="sxs-lookup"><span data-stu-id="827cb-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="827cb-110">Programmation asynchrone est principalement axée sur libérer le thread managé actuel (code .NET en cours d’exécution thread) à effectuer d’autres tâches pendant qu’il attend une opération qui ne nécessite pas les temps de calcul à partir d’un thread managé.</span><span class="sxs-lookup"><span data-stu-id="827cb-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="827cb-111">Par exemple, tandis que le moteur de base de données traite une requête a rien à faire en code .NET.</span><span class="sxs-lookup"><span data-stu-id="827cb-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="827cb-112">Dans les applications clientes (WinForms, WPF, etc.) du thread actuel peut être utilisé pour maintenir la réactivité de l’interface utilisateur pendant que l’opération asynchrone est effectuée.</span><span class="sxs-lookup"><span data-stu-id="827cb-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="827cb-113">Dans les applications de serveur (ASP.NET, etc.) que le thread peut être utilisé pour traiter les autres demandes entrantes - cela peut réduire l’utilisation de mémoire et/ou augmenter le débit du serveur.</span><span class="sxs-lookup"><span data-stu-id="827cb-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="827cb-114">Dans la plupart des applications à l’aide d’async n’aura aucun avantage notable et même peut être préjudiciable.</span><span class="sxs-lookup"><span data-stu-id="827cb-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="827cb-115">Utiliser des tests, de profilage et de bon sens pour mesurer l’impact des opérations asynchrones dans votre scénario particulier avant de le valider.</span><span class="sxs-lookup"><span data-stu-id="827cb-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="827cb-116">Voici d’autres ressources pour en savoir plus sur async :</span><span class="sxs-lookup"><span data-stu-id="827cb-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="827cb-117">Vue d’ensemble de Brandon Bray d’async/await dans .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="827cb-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="827cb-118">[Programmation asynchrone](https://msdn.microsoft.com/library/hh191443.aspx) pages dans la bibliothèque MSDN</span><span class="sxs-lookup"><span data-stu-id="827cb-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="827cb-119">[Comment générer une Async d’à l’aide de ASP.NET Web Applications](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclut une démonstration de débit d’accrue du serveur)</span><span class="sxs-lookup"><span data-stu-id="827cb-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="827cb-120">Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="827cb-120">Create the model</span></span>

<span data-ttu-id="827cb-121">Nous allons utiliser le [workflow Code First](~/ef6/modeling/code-first/workflows/new-database.md) pour créer notre modèle et générer la base de données, mais la fonctionnalité asynchrone fonctionne avec tous les modèles EF, y compris ceux créés avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="827cb-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="827cb-122">Créez une Application Console et appelez-le **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="827cb-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="827cb-123">Ajoutez le package EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="827cb-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="827cb-124">Dans l’Explorateur de solutions, cliquez sur le **AsyncDemo** projet</span><span class="sxs-lookup"><span data-stu-id="827cb-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="827cb-125">Sélectionnez **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="827cb-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="827cb-126">Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package</span><span class="sxs-lookup"><span data-stu-id="827cb-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="827cb-127">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="827cb-127">Click **Install**</span></span>
-   <span data-ttu-id="827cb-128">Ajouter un **Model.cs** classe avec l’implémentation suivante</span><span class="sxs-lookup"><span data-stu-id="827cb-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="827cb-129">Créer un programme synchrone</span><span class="sxs-lookup"><span data-stu-id="827cb-129">Create a synchronous program</span></span>

<span data-ttu-id="827cb-130">Maintenant que nous avons un modèle EF, nous allons écrire du code qui l’utilise pour effectuer certains accès aux données.</span><span class="sxs-lookup"><span data-stu-id="827cb-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="827cb-131">Remplacez le contenu de **Program.cs** par le code suivant</span><span class="sxs-lookup"><span data-stu-id="827cb-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="827cb-132">Ce code appelle la **PerformDatabaseOperations** méthode qui enregistre une nouvelle **Blog** à la base de données, puis récupère tous les **Blogs** à partir de la base de données et les imprimer à la **Console**.</span><span class="sxs-lookup"><span data-stu-id="827cb-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="827cb-133">Après cela, le programme écrit une citation du jour à la **Console**.</span><span class="sxs-lookup"><span data-stu-id="827cb-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="827cb-134">Étant donné que le code est synchrone, nous pouvons observer le flux d’exécution suivant lorsque nous exécutons le programme :</span><span class="sxs-lookup"><span data-stu-id="827cb-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="827cb-135">**SaveChanges** commence à transmettre le nouveau **Blog** à la base de données</span><span class="sxs-lookup"><span data-stu-id="827cb-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="827cb-136">**SaveChanges** terminée</span><span class="sxs-lookup"><span data-stu-id="827cb-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="827cb-137">Requête pour tous les **Blogs** est envoyé à la base de données</span><span class="sxs-lookup"><span data-stu-id="827cb-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="827cb-138">Retourne de la requête et les résultats sont écrits dans **Console**</span><span class="sxs-lookup"><span data-stu-id="827cb-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="827cb-139">Citation du jour est écrite dans **Console**</span><span class="sxs-lookup"><span data-stu-id="827cb-139">Quote of the day is written to **Console**</span></span>

![Sortie de la synchronisation](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="827cb-141">Rendre asynchrone</span><span class="sxs-lookup"><span data-stu-id="827cb-141">Making it asynchronous</span></span>

<span data-ttu-id="827cb-142">Maintenant que nous avons notre programme soit opérationnel, nous pouvons commencer faisant usage de la nouvelle async et await de mots clés.</span><span class="sxs-lookup"><span data-stu-id="827cb-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="827cb-143">Nous avons apporté les modifications suivantes au fichier Program.cs</span><span class="sxs-lookup"><span data-stu-id="827cb-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="827cb-144">Ligne 2 : L’à l’aide de l’instruction pour le **System.Data.Entity** espace de noms nous donne un accès pour les méthodes d’extension async EF.</span><span class="sxs-lookup"><span data-stu-id="827cb-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="827cb-145">Ligne 4 : L’à l’aide de l’instruction pour la **System.Threading.Tasks** espace de noms permet d’utiliser le **tâche** type.</span><span class="sxs-lookup"><span data-stu-id="827cb-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="827cb-146">Ligne 12 & 18 : nous vous proposons en tant que tâche qui surveille la progression de **PerformSomeDatabaseOperations** (ligne 12) et bloque l’exécution du programme pour cette tâche une fois terminée tout le travail pour le programme est réalisé (ligne 18).</span><span class="sxs-lookup"><span data-stu-id="827cb-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="827cb-147">Ligne 25 : Nous avons mise à jour **PerformSomeDatabaseOperations** à marquer comme **async** et retourner un **tâche**.</span><span class="sxs-lookup"><span data-stu-id="827cb-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="827cb-148">Ligne 35 : Nous allons maintenant appeler la version asynchrone de SaveChanges et en attente de son achèvement.</span><span class="sxs-lookup"><span data-stu-id="827cb-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="827cb-149">Ligne 42 : Nous allons maintenant appeler la version asynchrone de ToList et en attente sur le résultat.</span><span class="sxs-lookup"><span data-stu-id="827cb-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="827cb-150">Pour obtenir la liste complète des méthodes d’extension disponible dans l’espace de noms System.Data.Entity, reportez-vous à la classe QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="827cb-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="827cb-151">*Vous devrez également ajouter « using System.Data.Entity » à l’aide de vos instructions.*</span><span class="sxs-lookup"><span data-stu-id="827cb-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="827cb-152">Maintenant que le code est asynchrone, nous pouvons observer un flux d’exécution différent lorsque nous exécutons le programme :</span><span class="sxs-lookup"><span data-stu-id="827cb-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="827cb-153">**SaveChanges** commence à transmettre le nouveau **Blog** à la base de données *une fois que la commande est envoyée à la base de données, pas plus de calcul de temps est nécessaire sur le thread managé actuel. Le **PerformDatabaseOperations** méthode est retournée (même s’il n’a pas terminé l’exécution) et flux de programme dans la méthode Main se poursuit.*</span><span class="sxs-lookup"><span data-stu-id="827cb-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="827cb-154">**Citation du jour est écrite dans la Console**
    \*puisqu’il n’existe plus aucune tâche à effectuer dans la méthode Main, le thread géré est bloqué sur l’attente appeler jusqu'à ce que l’opération de base de données se termine. Une fois terminée, le reste de notre **PerformDatabaseOperations** \* sera exécuté.</span><span class="sxs-lookup"><span data-stu-id="827cb-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="827cb-155">**SaveChanges** terminée</span><span class="sxs-lookup"><span data-stu-id="827cb-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="827cb-156">Requête pour tous les **Blogs** est envoyé à la base de données *là encore, le thread managé est libre d’effectuer d’autres tâches pendant que la requête est traitée dans la base de données. Étant donné que tous les autres l’exécution terminée, le thread simplement arrêtera sur l’appel d’attente cependant.*</span><span class="sxs-lookup"><span data-stu-id="827cb-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="827cb-157">Retourne de la requête et les résultats sont écrits dans **Console**</span><span class="sxs-lookup"><span data-stu-id="827cb-157">Query returns and results are written to **Console**</span></span>

![Sortie d’async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="827cb-159">Le principal avantage</span><span class="sxs-lookup"><span data-stu-id="827cb-159">The takeaway</span></span>

<span data-ttu-id="827cb-160">Nous avons maintenant vu combien il est facile pour rendre utiliser des méthodes asynchrones d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="827cb-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="827cb-161">Bien que les avantages des opérations asynchrones n’est peut-être pas très évidentes avec une application console simple, ces stratégies peuvent être appliquées dans les situations où les activités longues ou limite de réseau peuvent sinon bloquer l’application, ou un grand nombre de threads à augmenter l’encombrement mémoire.</span><span class="sxs-lookup"><span data-stu-id="827cb-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
