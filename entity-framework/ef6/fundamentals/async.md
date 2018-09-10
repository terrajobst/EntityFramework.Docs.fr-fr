---
title: Async interroge et enregistre - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 35604fc16ea37415d39801831aa162d0d42c2a2f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250749"
---
# <a name="async-query-and-save"></a>Async interroger et enregistrer
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

EF6 a introduit la prise en charge pour une requête asynchrone et enregistrer à l’aide de la [async et await mots clés](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) qui ont été introduites dans .NET 4.5. Alors que toutes les applications ne puissent bénéficier de l’asynchronie, il peut être utilisé pour améliorer l’extensibilité de la réactivité et de serveur client lors de la gestion des longues, de réseau ou de tâches d’e/S.

## <a name="when-to-really-use-async"></a>Quand utiliser réellement async

L’objectif de cette procédure pas à pas consiste à présenter les concepts asynchrone d’une manière qui rend facile à observer la différence entre l’exécution du programme synchrone et asynchrone. Cette procédure pas à pas ne vise pas pour illustrer un des principaux scénarios où la programmation asynchrone fournit des avantages.

Programmation asynchrone est principalement axée sur libérer le thread managé actuel (code .NET en cours d’exécution thread) à effectuer d’autres tâches pendant qu’il attend une opération qui ne nécessite pas les temps de calcul à partir d’un thread managé. Par exemple, tandis que le moteur de base de données traite une requête a rien à faire en code .NET.

Dans les applications clientes (WinForms, WPF, etc.) du thread actuel peut être utilisé pour maintenir la réactivité de l’interface utilisateur pendant que l’opération asynchrone est effectuée. Dans les applications de serveur (ASP.NET, etc.) que le thread peut être utilisé pour traiter les autres demandes entrantes - cela peut réduire l’utilisation de mémoire et/ou augmenter le débit du serveur.

Dans la plupart des applications à l’aide d’async n’aura aucun avantage notable et même peut être préjudiciable. Utiliser des tests, de profilage et de bon sens pour mesurer l’impact des opérations asynchrones dans votre scénario particulier avant de le valider.

Voici d’autres ressources pour en savoir plus sur async :

-   [Vue d’ensemble de Brandon Bray d’async/await dans .NET 4.5](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Programmation asynchrone](https://msdn.microsoft.com/library/hh191443.aspx) pages dans la bibliothèque MSDN
-   [Comment générer une Async d’à l’aide de ASP.NET Web Applications](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclut une démonstration de débit d’accrue du serveur)

## <a name="create-the-model"></a>Créer le modèle

Nous allons utiliser le [workflow Code First](~/ef6/modeling/code-first/workflows/new-database.md) pour créer notre modèle et générer la base de données, mais la fonctionnalité asynchrone fonctionne avec tous les modèles EF, y compris ceux créés avec le Concepteur EF.

-   Créez une Application Console et appelez-le **AsyncDemo**
-   Ajoutez le package EntityFramework NuGet
    -   Dans l’Explorateur de solutions, cliquez sur le **AsyncDemo** projet
    -   Sélectionnez **gérer les Packages NuGet...**
    -   Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package
    -   Cliquez sur **installer**
-   Ajouter un **Model.cs** classe avec l’implémentation suivante

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

 

## <a name="create-a-synchronous-program"></a>Créer un programme synchrone

Maintenant que nous avons un modèle EF, nous allons écrire du code qui l’utilise pour effectuer certains accès aux données.

-   Remplacez le contenu de **Program.cs** par le code suivant

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

Ce code appelle la **PerformDatabaseOperations** méthode qui enregistre une nouvelle **Blog** à la base de données, puis récupère tous les **Blogs** à partir de la base de données et les imprimer à la **Console**. Après cela, le programme écrit une citation du jour à la **Console**.

Étant donné que le code est synchrone, nous pouvons observer le flux d’exécution suivant lorsque nous exécutons le programme :

1.  **SaveChanges** commence à transmettre le nouveau **Blog** à la base de données
2.  **SaveChanges** terminée
3.  Requête pour tous les **Blogs** est envoyé à la base de données
4.  Retourne de la requête et les résultats sont écrits dans **Console**
5.  Citation du jour est écrite dans **Console**

![Sortie de la synchronisation](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Rendre asynchrone

Maintenant que nous avons notre programme soit opérationnel, nous pouvons commencer faisant usage de la nouvelle async et await de mots clés. Nous avons apporté les modifications suivantes au fichier Program.cs

1.  Ligne 2 : L’à l’aide de l’instruction pour le **System.Data.Entity** espace de noms nous donne un accès pour les méthodes d’extension async EF.
2.  Ligne 4 : L’à l’aide de l’instruction pour la **System.Threading.Tasks** espace de noms permet d’utiliser le **tâche** type.
3.  Ligne 12 & 18 : nous vous proposons en tant que tâche qui surveille la progression de **PerformSomeDatabaseOperations** (ligne 12) et bloque l’exécution du programme pour cette tâche une fois terminée tout le travail pour le programme est réalisé (ligne 18).
4.  Ligne 25 : Nous avons mise à jour **PerformSomeDatabaseOperations** à marquer comme **async** et retourner un **tâche**.
5.  Ligne 35 : Nous allons maintenant appeler la version asynchrone de SaveChanges et en attente de son achèvement.
6.  Ligne 42 : Nous allons maintenant appeler la version asynchrone de ToList et en attente sur le résultat.

Pour obtenir la liste complète des méthodes d’extension disponible dans l’espace de noms System.Data.Entity, reportez-vous à la classe QueryableExtensions. *Vous devrez également ajouter « using System.Data.Entity » à l’aide de vos instructions.*

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

Maintenant que le code est asynchrone, nous pouvons observer un flux d’exécution différent lorsque nous exécutons le programme :

1.  **SaveChanges** commence à transmettre le nouveau **Blog** à la base de données *une fois que la commande est envoyée à la base de données, pas plus de calcul de temps est nécessaire sur le thread managé actuel. Le **PerformDatabaseOperations** méthode est retournée (même s’il n’a pas terminé l’exécution) et flux de programme dans la méthode Main se poursuit.*
2.  **Citation du jour est écrite dans la Console**
    *puisqu’il n’existe plus aucune tâche à effectuer dans la méthode Main, le thread géré est bloqué sur l’attente appeler jusqu'à ce que l’opération de base de données se termine. Une fois terminée, le reste de notre **PerformDatabaseOperations** * sera exécuté.
3.  **SaveChanges** terminée
4.  Requête pour tous les **Blogs** est envoyé à la base de données *là encore, le thread managé est libre d’effectuer d’autres tâches pendant que la requête est traitée dans la base de données. Étant donné que tous les autres l’exécution terminée, le thread simplement arrêtera sur l’appel d’attente cependant.*
5.  Retourne de la requête et les résultats sont écrits dans **Console**

![Sortie d’async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Le principal avantage

Nous avons maintenant vu combien il est facile pour rendre utiliser des méthodes asynchrones d’Entity Framework. Bien que les avantages des opérations asynchrones n’est peut-être pas très évidentes avec une application console simple, ces stratégies peuvent être appliquées dans les situations où les activités longues ou limite de réseau peuvent sinon bloquer l’application, ou un grand nombre de threads à augmenter l’encombrement mémoire.
