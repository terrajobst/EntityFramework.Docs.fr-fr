---
title: Utilisation de DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120803"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="08884-102">Utilisation de DbContext</span><span class="sxs-lookup"><span data-stu-id="08884-102">Working with DbContext</span></span>

<span data-ttu-id="08884-103">Pour utiliser Entity Framework pour interroger, insérer, mettre à jour et supprimer des données à l’aide d’objets .NET, vous devez d’abord [créer un modèle](~/ef6/modeling/index.md) qui mappe les entités et les relations qui sont définies dans votre modèle à des tables dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="08884-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="08884-104">Une fois que vous avez un modèle, la classe principale, votre application interagit avec est `System.Data.Entity.DbContext` (souvent appelé la classe de contexte).</span><span class="sxs-lookup"><span data-stu-id="08884-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="08884-105">Vous pouvez utiliser un DbContext associé à un modèle pour :</span><span class="sxs-lookup"><span data-stu-id="08884-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="08884-106">Écrire et exécuter des requêtes</span><span class="sxs-lookup"><span data-stu-id="08884-106">Write and execute queries</span></span>   
- <span data-ttu-id="08884-107">Matérialiser les résultats de la requête en tant qu’objets d’entité</span><span class="sxs-lookup"><span data-stu-id="08884-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="08884-108">Le suivi des modifications apportées à ces objets.</span><span class="sxs-lookup"><span data-stu-id="08884-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="08884-109">Conserver les modifications de l’objet dans la base de données</span><span class="sxs-lookup"><span data-stu-id="08884-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="08884-110">Lier des objets en mémoire pour les contrôles d’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="08884-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="08884-111">Cette page fournit des conseils sur la gestion de la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="08884-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="08884-112">Définition d’une classe DbContext dérivée</span><span class="sxs-lookup"><span data-stu-id="08884-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="08884-113">La méthode recommandée pour travailler avec contexte consiste à définir une classe qui dérive de DbContext et expose les propriétés DbSet qui représentent des collections d’entités dans le contexte spécifiées.</span><span class="sxs-lookup"><span data-stu-id="08884-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="08884-114">Si vous travaillez avec le Concepteur EF, le contexte sera généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="08884-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="08884-115">Si vous utilisez Code First, vous allez généralement écrire le contexte vous-même.</span><span class="sxs-lookup"><span data-stu-id="08884-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="08884-116">Une fois que vous disposez d’un contexte, vous interroger, ajouter (à l’aide de `Add` ou `Attach` méthodes) ou supprimer (à l’aide de `Remove`) les entités dans le contexte via ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="08884-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="08884-117">L’accès à un `DbSet` propriété sur un objet de contexte représentent une requête initiale qui retourne toutes les entités du type spécifié.</span><span class="sxs-lookup"><span data-stu-id="08884-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="08884-118">Notez que l’accès à une propriété pas exécute la requête.</span><span class="sxs-lookup"><span data-stu-id="08884-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="08884-119">Une requête est exécutée lorsque :</span><span class="sxs-lookup"><span data-stu-id="08884-119">A query is executed when:</span></span>  

- <span data-ttu-id="08884-120">elle est énumérée par une instruction `foreach` (C#) ou `For Each` (Visual Basic) ;</span><span class="sxs-lookup"><span data-stu-id="08884-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="08884-121">Elle est énumérée par une opération de collection comme `ToArray`, `ToDictionary`, ou `ToList`.</span><span class="sxs-lookup"><span data-stu-id="08884-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="08884-122">Les opérateurs LINQ, tels que `First` ou `Any` sont spécifiés dans la partie la plus extérieure de la requête.</span><span class="sxs-lookup"><span data-stu-id="08884-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="08884-123">Une des méthodes suivantes sont appelées : le `Load` méthode d’extension, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, et `DbSet<T>.Find`, si une entité avec la clé spécifiée se trouve pas déjà chargée dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="08884-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="08884-124">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="08884-124">Lifetime</span></span>  

<span data-ttu-id="08884-125">La durée de vie du contexte commence lorsque l’instance est créée et se termine lorsque l’instance est supprimée ou opération de garbage collection.</span><span class="sxs-lookup"><span data-stu-id="08884-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="08884-126">Utilisez **à l’aide de** si vous voulez que toutes les ressources contrôlées par le contexte soient supprimées à la fin du bloc.</span><span class="sxs-lookup"><span data-stu-id="08884-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="08884-127">Lorsque vous utilisez **à l’aide de**, le compilateur crée automatiquement un bloc try/finally et appelle la méthode dispose dans le **enfin** bloc.</span><span class="sxs-lookup"><span data-stu-id="08884-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="08884-128">Voici quelques instructions générales lorsque vous décidez de la durée de vie du contexte :</span><span class="sxs-lookup"><span data-stu-id="08884-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="08884-129">Lorsque vous travaillez avec les applications Web, utilisez une instance de contexte par demande.</span><span class="sxs-lookup"><span data-stu-id="08884-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="08884-130">Lorsque vous travaillez avec Windows Presentation Foundation (WPF) ou Windows Forms, utilisez une instance de contexte par formulaire.</span><span class="sxs-lookup"><span data-stu-id="08884-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="08884-131">Cela vous permet d’utiliser des fonctionnalités de suivi des modifications fournit ce contexte.</span><span class="sxs-lookup"><span data-stu-id="08884-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="08884-132">Si l’instance de contexte est créée par un conteneur d’injection de dépendance, il est généralement la responsabilité du conteneur à supprimer le contexte.</span><span class="sxs-lookup"><span data-stu-id="08884-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="08884-133">Si le contexte est créé dans le code d’application, pensez à supprimer le contexte lorsqu’il n’est plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="08884-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="08884-134">Lorsque vous travaillez avec le contexte d’exécution longue, considérez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="08884-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="08884-135">Comme vous chargez des objets et leurs références dans la mémoire, du contexte de la consommation de mémoire peut augmenter rapidement.</span><span class="sxs-lookup"><span data-stu-id="08884-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="08884-136">Cela peut provoquer des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="08884-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="08884-137">Le contexte n’est pas thread-safe, par conséquent, il ne doit pas être partagé entre plusieurs threads travailler simultanément sur celui-ci.</span><span class="sxs-lookup"><span data-stu-id="08884-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="08884-138">Si une exception provoque le contexte dans un état irrécupérable, l’application entière peut mettre fin.</span><span class="sxs-lookup"><span data-stu-id="08884-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="08884-139">Les risques liés à l'exécution dans un état d'accès concurrentiel augmentent à mesure que l'intervalle entre le moment où les données sont interrogées et mises à jour s'allonge.</span><span class="sxs-lookup"><span data-stu-id="08884-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="08884-140">Connexions</span><span class="sxs-lookup"><span data-stu-id="08884-140">Connections</span></span>  

<span data-ttu-id="08884-141">Par défaut, le contexte gère les connexions à la base de données.</span><span class="sxs-lookup"><span data-stu-id="08884-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="08884-142">Le contexte s’ouvre et ferme les connexions en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="08884-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="08884-143">Par exemple, le contexte ouvre une connexion pour exécuter une requête, puis ferme la connexion lorsque tous les jeux de résultats ont été traitées.</span><span class="sxs-lookup"><span data-stu-id="08884-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="08884-144">Dans certains cas, vous souhaiterez avoir davantage de contrôle sur l'ouverture et la fermeture des connexions.</span><span class="sxs-lookup"><span data-stu-id="08884-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="08884-145">Par exemple, lorsque vous travaillez avec SQL Server Compact, il est souvent recommandé de maintenir une connexion à la base de données ouverte distincte pour la durée de vie de l’application pour améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="08884-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="08884-146">Vous pouvez gérer ce processus manuellement à l'aide de la propriété `Connection`.</span><span class="sxs-lookup"><span data-stu-id="08884-146">You can manage this process manually by using the `Connection` property.</span></span>  
