---
title: Utilisation de DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416321"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="aa4dd-102">Utilisation de DbContext</span><span class="sxs-lookup"><span data-stu-id="aa4dd-102">Working with DbContext</span></span>

<span data-ttu-id="aa4dd-103">Pour pouvoir utiliser Entity Framework pour interroger, insérer, mettre à jour et supprimer des données à l’aide d’objets .NET, vous devez d’abord [créer un modèle](~/ef6/modeling/index.md) qui mappe les entités et les relations définies dans votre modèle aux tables d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="aa4dd-104">Une fois que vous disposez d’un modèle, la classe principale avec laquelle votre application interagit est `System.Data.Entity.DbContext` (souvent appelée classe de contexte).</span><span class="sxs-lookup"><span data-stu-id="aa4dd-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="aa4dd-105">Vous pouvez utiliser un DbContext associé à un modèle pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa4dd-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="aa4dd-106">Écrire et exécuter des requêtes</span><span class="sxs-lookup"><span data-stu-id="aa4dd-106">Write and execute queries</span></span>   
- <span data-ttu-id="aa4dd-107">Matérialiser les résultats d’une requête en tant qu’objets entité</span><span class="sxs-lookup"><span data-stu-id="aa4dd-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="aa4dd-108">Suivre les modifications apportées à ces objets</span><span class="sxs-lookup"><span data-stu-id="aa4dd-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="aa4dd-109">Conserver les modifications des objets sur la base de données</span><span class="sxs-lookup"><span data-stu-id="aa4dd-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="aa4dd-110">Lier des objets en mémoire à des contrôles d’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="aa4dd-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="aa4dd-111">Cette page fournit des conseils sur la façon de gérer la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="aa4dd-112">Définition d’une classe dérivée DbContext</span><span class="sxs-lookup"><span data-stu-id="aa4dd-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="aa4dd-113">La méthode recommandée pour utiliser le contexte consiste à définir une classe qui dérive de DbContext et expose des propriétés DbSet qui représentent des collections des entités spécifiées dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="aa4dd-114">Si vous utilisez le concepteur EF, le contexte est généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="aa4dd-115">Si vous utilisez Code First, vous écrivez généralement le contexte vous-même.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="aa4dd-116">Une fois que vous avez un contexte, vous pouvez rechercher, ajouter (à l’aide des méthodes `Add` ou `Attach`) ou supprimer (à l’aide d' `Remove`) des entités dans le contexte via ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="aa4dd-117">L’accès à une propriété de `DbSet` sur un objet de contexte représente une requête de démarrage qui retourne toutes les entités du type spécifié.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="aa4dd-118">Notez que l’accès à une propriété n’exécute pas la requête.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="aa4dd-119">Une requête est exécutée dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="aa4dd-119">A query is executed when:</span></span>  

- <span data-ttu-id="aa4dd-120">elle est énumérée par une instruction `foreach` (C#) ou `For Each` (Visual Basic) ;</span><span class="sxs-lookup"><span data-stu-id="aa4dd-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="aa4dd-121">Elle est énumérée par une opération de collection comme `ToArray`, `ToDictionary`ou `ToList`.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="aa4dd-122">Les opérateurs LINQ tels que `First` ou `Any` sont spécifiés dans la partie la plus à l’extérieur de la requête.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="aa4dd-123">L’une des méthodes suivantes est appelée : la méthode d’extension `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`et `DbSet<T>.Find`, si une entité avec la clé spécifiée est introuvable déjà chargée dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="aa4dd-124">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="aa4dd-124">Lifetime</span></span>  

<span data-ttu-id="aa4dd-125">La durée de vie du contexte commence lorsque l’instance est créée et se termine lorsque l’instance est supprimée ou récupérée par le garbage collector.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="aa4dd-126">Utilisez **si vous** souhaitez que toutes les ressources qui contrôlent le contexte soient supprimées à la fin du bloc.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="aa4dd-127">Lorsque vous utilisez **, le**compilateur crée automatiquement un bloc try/finally et appelle dispose dans le bloc **finally** .</span><span class="sxs-lookup"><span data-stu-id="aa4dd-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="aa4dd-128">Voici quelques recommandations générales pour décider de la durée de vie du contexte :</span><span class="sxs-lookup"><span data-stu-id="aa4dd-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="aa4dd-129">Lorsque vous travaillez avec des applications Web, utilisez une instance de contexte par demande.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="aa4dd-130">Lorsque vous travaillez avec Windows Presentation Foundation (WPF) ou Windows Forms, utilisez une instance de contexte par formulaire.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="aa4dd-131">Cela vous permet d’utiliser la fonctionnalité de suivi des modifications fournie par le contexte.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="aa4dd-132">Si l’instance de contexte est créée par un conteneur d’injection de dépendance, il incombe généralement au conteneur de supprimer le contexte.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="aa4dd-133">Si le contexte est créé dans le code de l’application, n’oubliez pas de supprimer le contexte lorsqu’il n’est plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="aa4dd-134">Lorsque vous travaillez avec un contexte de longue durée, tenez compte des points suivants :</span><span class="sxs-lookup"><span data-stu-id="aa4dd-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="aa4dd-135">Lorsque vous chargez plus d’objets et leurs références dans la mémoire, la consommation de mémoire du contexte peut augmenter rapidement.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="aa4dd-136">Cela peut provoquer des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="aa4dd-137">Le contexte n’est pas thread-safe. par conséquent, il ne doit pas être partagé entre plusieurs threads qui y travaillent simultanément.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="aa4dd-138">Si une exception fait que le contexte est dans un État irrécupérable, l’application entière peut se terminer.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="aa4dd-139">Les risques liés à l'exécution dans un état d'accès concurrentiel augmentent à mesure que l'intervalle entre le moment où les données sont interrogées et mises à jour s'allonge.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="aa4dd-140">Connexions</span><span class="sxs-lookup"><span data-stu-id="aa4dd-140">Connections</span></span>  

<span data-ttu-id="aa4dd-141">Par défaut, le contexte gère les connexions à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="aa4dd-142">Le contexte ouvre et ferme les connexions en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="aa4dd-143">Par exemple, le contexte ouvre une connexion pour exécuter une requête, puis ferme la connexion lorsque tous les jeux de résultats ont été traités.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="aa4dd-144">Dans certains cas, vous souhaiterez avoir davantage de contrôle sur l'ouverture et la fermeture des connexions.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="aa4dd-145">Par exemple, lors de l’utilisation de SQL Server Compact, il est souvent recommandé de conserver une connexion ouverte distincte à la base de données pendant toute la durée de vie de l’application afin d’améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="aa4dd-146">Vous pouvez gérer ce processus manuellement à l'aide de la propriété `Connection`.</span><span class="sxs-lookup"><span data-stu-id="aa4dd-146">You can manage this process manually by using the `Connection` property.</span></span>  
