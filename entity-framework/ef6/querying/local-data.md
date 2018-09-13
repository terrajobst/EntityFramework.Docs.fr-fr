---
title: Données locales - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490452"
---
# <a name="local-data"></a><span data-ttu-id="23c32-102">Données locales</span><span class="sxs-lookup"><span data-stu-id="23c32-102">Local Data</span></span>
<span data-ttu-id="23c32-103">Exécuter une requête LINQ directement sur un DbSet toujours enverra une requête à la base de données, mais vous pouvez accéder aux données qui sont actuellement en mémoire à l’aide de la propriété DbSet.Local.</span><span class="sxs-lookup"><span data-stu-id="23c32-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="23c32-104">Vous pouvez également accéder à l’Entity Framework effectue le suivi des informations supplémentaires sur vos entités avec les méthodes DbContext.Entry et DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="23c32-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="23c32-105">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="23c32-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="23c32-106">À l’aide locale pour examiner les données locales</span><span class="sxs-lookup"><span data-stu-id="23c32-106">Using Local to look at local data</span></span>  

<span data-ttu-id="23c32-107">La propriété locale de DbSet fournit un accès simple aux entités du jeu qui sont actuellement suivis par le contexte et n’ont pas été marquées comme supprimées.</span><span class="sxs-lookup"><span data-stu-id="23c32-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="23c32-108">L’accès à la propriété locale n’entraîne jamais une requête à envoyer à la base de données.</span><span class="sxs-lookup"><span data-stu-id="23c32-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="23c32-109">Cela signifie qu’il est généralement utilisé après qu’une requête a déjà été effectuée.</span><span class="sxs-lookup"><span data-stu-id="23c32-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="23c32-110">La méthode d’extension de charge peut être utilisée pour exécuter une requête afin que le contexte suit les résultats.</span><span class="sxs-lookup"><span data-stu-id="23c32-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="23c32-111">Exemple :</span><span class="sxs-lookup"><span data-stu-id="23c32-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="23c32-112">Si nous avions deux blogs dans la base de données - « ADO.NET Blog » avec un BlogId 1 - et « le Blog Visual Studio' avec un BlogId de 2, nous aurions pu obtenir la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="23c32-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="23c32-113">Cela illustre trois points :</span><span class="sxs-lookup"><span data-stu-id="23c32-113">This illustrates three points:</span></span>  

- <span data-ttu-id="23c32-114">Le nouveau blog « Mon nouveau Blog » est inclus dans la collection locale, même si elle n’a pas encore été enregistré dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="23c32-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="23c32-115">Ce blog a une clé primaire égale à zéro, car la base de données n’a pas encore généré une clé réelle de l’entité.</span><span class="sxs-lookup"><span data-stu-id="23c32-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="23c32-116">Le Blog de ADO.NET n’est pas inclus dans la collection locale, même si elle est toujours suivie par le contexte.</span><span class="sxs-lookup"><span data-stu-id="23c32-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="23c32-117">Il s’agit, car nous avons retiré le DbSet ainsi marquant comme étant supprimé.</span><span class="sxs-lookup"><span data-stu-id="23c32-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="23c32-118">Lorsque DbSet est utilisé pour effectuer une requête le blog marqué pour suppression (Blog ADO.NET) est inclus dans les résultats et le nouveau blog (Blog de nouveau mon) qui n’a pas encore été enregistré à la base de données n’est pas inclus dans les résultats.</span><span class="sxs-lookup"><span data-stu-id="23c32-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="23c32-119">Il s’agit, car le DbSet effectue une requête sur la base de données et les résultats retournés toujours refléter les informations spécifiées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="23c32-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="23c32-120">À l’aide locale pour ajouter et supprimer des entités à partir du contexte</span><span class="sxs-lookup"><span data-stu-id="23c32-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="23c32-121">La propriété locale sur DbSet retourne un [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) avec événements raccordés telle qu’elle reste synchronisée avec le contenu du contexte.</span><span class="sxs-lookup"><span data-stu-id="23c32-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="23c32-122">Cela signifie que les entités peuvent être ajoutées ou supprimées de la collection locale ou le DbSet.</span><span class="sxs-lookup"><span data-stu-id="23c32-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="23c32-123">Cela signifie également que les requêtes qui apportent de nouvelles entités dans le contexte entraîne la collection locale en cours de mise à jour avec ces entités.</span><span class="sxs-lookup"><span data-stu-id="23c32-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="23c32-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="23c32-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="23c32-125">En supposant que nous avions quelques billets marqués avec « entity framework » et « asp.net » la sortie peut ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="23c32-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="23c32-126">Cela illustre trois points :</span><span class="sxs-lookup"><span data-stu-id="23c32-126">This illustrates three points:</span></span>  

- <span data-ttu-id="23c32-127">La nouvelle publication, « Quelles sont les nouveautés dans EF » qui a été ajouté à la variable locale collection devienne suivie par le contexte dans l’état Added.</span><span class="sxs-lookup"><span data-stu-id="23c32-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="23c32-128">Il sera par conséquent insérée dans la base de données lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="23c32-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="23c32-129">Le billet a été supprimé de la collection locale (Guide du débutant en EF) est désormais marqué comme supprimée dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="23c32-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="23c32-130">Il sera par conséquent être supprimé à partir de la base de données lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="23c32-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="23c32-131">Le billet supplémentaire (Guide du débutant en ASP.NET) chargé dans le contexte avec la deuxième requête est automatiquement ajouté à la collection locale.</span><span class="sxs-lookup"><span data-stu-id="23c32-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="23c32-132">Une dernière chose à noter concernant Local qui est car il s’agit de qu'une performance ObservableCollection n’est pas satisfaisante pour un grand nombre d’entités.</span><span class="sxs-lookup"><span data-stu-id="23c32-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="23c32-133">Si vous êtes confronté à des milliers d’entités dans votre contexte il seront donc ne peut-être pas recommandé d’utiliser Local.</span><span class="sxs-lookup"><span data-stu-id="23c32-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="23c32-134">À l’aide locale pour la liaison de données WPF</span><span class="sxs-lookup"><span data-stu-id="23c32-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="23c32-135">La propriété locale sur DbSet peut être utilisée directement pour la liaison de données dans une application WPF s’agissant d’une instance de ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="23c32-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="23c32-136">Comme décrit dans les sections précédentes, que cela signifie qu’il sera automatiquement synchronisée avec le contenu du contexte et le contenu du contexte est automatiquement synchronisés avec lui.</span><span class="sxs-lookup"><span data-stu-id="23c32-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="23c32-137">Notez que vous n’avez pas besoin de préremplir la collection locale avec les données soient rien à lier dans la mesure où Local ne provoque jamais une requête de base de données.</span><span class="sxs-lookup"><span data-stu-id="23c32-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="23c32-138">Ce n’est pas un emplacement approprié pour un exemple de liaison de données WPF complet, mais les éléments clés sont :</span><span class="sxs-lookup"><span data-stu-id="23c32-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="23c32-139">Le programme d’installation d’une source de liaison</span><span class="sxs-lookup"><span data-stu-id="23c32-139">Setup a binding source</span></span>  
- <span data-ttu-id="23c32-140">Lier à la propriété locale de votre jeu</span><span class="sxs-lookup"><span data-stu-id="23c32-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="23c32-141">Remplir Local à l’aide d’une requête à la base de données.</span><span class="sxs-lookup"><span data-stu-id="23c32-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="23c32-142">Liaison de WPF pour les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="23c32-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="23c32-143">Si vous effectuez des liaisons de données maître/détail peuvent souhaiter lier l’affichage des détails à une propriété de navigation d’un de vos entités.</span><span class="sxs-lookup"><span data-stu-id="23c32-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="23c32-144">Un moyen simple pour y parvenir consiste à utiliser ObservableCollection pour la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="23c32-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="23c32-145">Exemple :</span><span class="sxs-lookup"><span data-stu-id="23c32-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="23c32-146">À l’aide locale pour nettoyer les entités dans SaveChanges</span><span class="sxs-lookup"><span data-stu-id="23c32-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="23c32-147">Dans la plupart des cas les entités supprimées à partir d’une propriété de navigation ne seront pas automatiquement marquées comme étant supprimées dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="23c32-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="23c32-148">Par exemple, si vous supprimez un objet de publication à partir de la collection Blog.Posts puis qui publient n'est pas supprimées automatiquement lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="23c32-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="23c32-149">Si vous avez besoin pour être supprimés puis vous devrez peut-être rechercher ces entités non résolues et de les marquer comme étant supprimé avant l’appel de SaveChanges ou comme partie d’un SaveChanges substituée.</span><span class="sxs-lookup"><span data-stu-id="23c32-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="23c32-150">Exemple :</span><span class="sxs-lookup"><span data-stu-id="23c32-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="23c32-151">Le code ci-dessus utilise la collection locale pour rechercher toutes les publications et les marques de ceux qui n’ont pas une référence de blog comme étant supprimé.</span><span class="sxs-lookup"><span data-stu-id="23c32-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="23c32-152">L’appel ToList est nécessaire, car sinon la collection sera modifiée par la suppression appel pendant son énumération.</span><span class="sxs-lookup"><span data-stu-id="23c32-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="23c32-153">Dans la plupart des autres situations, vous pouvez interroger directement par rapport à la propriété locale sans utiliser ToList tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="23c32-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="23c32-154">À l’aide de la liaison de données locale et ToBindingList for Windows Forms</span><span class="sxs-lookup"><span data-stu-id="23c32-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="23c32-155">Windows Forms ne prend pas en charge la liaison de données d’une fidélité optimale à l’aide de ObservableCollection directement.</span><span class="sxs-lookup"><span data-stu-id="23c32-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="23c32-156">Toutefois, vous pouvez toujours utiliser la propriété DbSet locale pour la liaison de données pour obtenir tous les avantages décrits dans les sections précédentes.</span><span class="sxs-lookup"><span data-stu-id="23c32-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="23c32-157">Ceci se fait via la méthode d’extension ToBindingList qui crée un [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implémentation est garantie par ObservableCollection Local.</span><span class="sxs-lookup"><span data-stu-id="23c32-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="23c32-158">Ce n’est pas un emplacement approprié pour un exemple de liaison de données Windows Forms complet, mais les éléments clés sont :</span><span class="sxs-lookup"><span data-stu-id="23c32-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="23c32-159">Le programme d’installation d’un objet source de liaison</span><span class="sxs-lookup"><span data-stu-id="23c32-159">Setup an object binding source</span></span>  
- <span data-ttu-id="23c32-160">Lier à la propriété locale de votre jeu à l’aide de Local.ToBindingList()</span><span class="sxs-lookup"><span data-stu-id="23c32-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="23c32-161">Remplir Local à l’aide d’une requête à la base de données</span><span class="sxs-lookup"><span data-stu-id="23c32-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="23c32-162">Obtenir des informations détaillées sur les entités suivies</span><span class="sxs-lookup"><span data-stu-id="23c32-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="23c32-163">La plupart des exemples de cette série utilisent la méthode d’entrée pour retourner une instance DbEntityEntry pour une entité.</span><span class="sxs-lookup"><span data-stu-id="23c32-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="23c32-164">Cet objet d’entrée agit alors comme point de départ pour la collecte des informations sur l’entité telles que son état actuel, ainsi que pour effectuer des opérations sur l’entité telles que le chargement explicite d’une entité associée.</span><span class="sxs-lookup"><span data-stu-id="23c32-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="23c32-165">Les méthodes d’entrées retournent des objets DbEntityEntry pour nombreuses ou toutes les entités suivies par le contexte.</span><span class="sxs-lookup"><span data-stu-id="23c32-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="23c32-166">Cela vous permet de collecter des informations ou effectuer des opérations sur nombreuses entités plutôt que simplement une seule entrée.</span><span class="sxs-lookup"><span data-stu-id="23c32-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="23c32-167">Exemple :</span><span class="sxs-lookup"><span data-stu-id="23c32-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="23c32-168">Vous remarquerez que nous avons introduit une classe de l’auteur et le lecteur dans l’exemple : les deux de ces classes implémentent l’interface IPerson.</span><span class="sxs-lookup"><span data-stu-id="23c32-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="23c32-169">Supposons que nous avons les données suivantes dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="23c32-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="23c32-170">Blog avec BlogId = 1 et le nom = 'ADO.NET Blog'</span><span class="sxs-lookup"><span data-stu-id="23c32-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="23c32-171">Blog avec BlogId = 2, nom = 'Le Blog de Visual Studio'</span><span class="sxs-lookup"><span data-stu-id="23c32-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="23c32-172">Blog avec BlogId = 3 et le nom = 'Blog.NET Framework'</span><span class="sxs-lookup"><span data-stu-id="23c32-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="23c32-173">Auteur avec AuthorId = 1 et le nom = 'Joe Bloggs'</span><span class="sxs-lookup"><span data-stu-id="23c32-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="23c32-174">Lecteur avec ReaderId = 1 et le nom = « Jean Dupont »</span><span class="sxs-lookup"><span data-stu-id="23c32-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="23c32-175">La sortie à partir de l’exécution du code serait :</span><span class="sxs-lookup"><span data-stu-id="23c32-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="23c32-176">Les exemples suivants illustrent plusieurs points :</span><span class="sxs-lookup"><span data-stu-id="23c32-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="23c32-177">Les méthodes d’entrées renvoient les entrées pour les entités dans tous les États, y compris Deleted.</span><span class="sxs-lookup"><span data-stu-id="23c32-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="23c32-178">Comparez cela à Local qui exclut supprimé des entités.</span><span class="sxs-lookup"><span data-stu-id="23c32-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="23c32-179">Entrées pour tous les types d’entités sont retournées lorsque la méthode d’entrées non générique est utilisée.</span><span class="sxs-lookup"><span data-stu-id="23c32-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="23c32-180">Lors de l’utilisation de la méthode générique entrées entrées sont retournées uniquement pour les entités qui sont des instances du type générique.</span><span class="sxs-lookup"><span data-stu-id="23c32-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="23c32-181">Cela a été utilisée ci-dessus pour obtenir des entrées pour tous les blogs.</span><span class="sxs-lookup"><span data-stu-id="23c32-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="23c32-182">Elle était également utilisée pour obtenir les entrées pour toutes les entités qui implémentent IPerson.</span><span class="sxs-lookup"><span data-stu-id="23c32-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="23c32-183">Cet exemple montre que le type générique ne devra pas être un type d’entité réelle.</span><span class="sxs-lookup"><span data-stu-id="23c32-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="23c32-184">LINQ aux objets peut être utilisé pour filtrer les résultats retournés.</span><span class="sxs-lookup"><span data-stu-id="23c32-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="23c32-185">Cela était utilisée ci-dessus pour rechercher des entités de n’importe quel type, que s’ils sont modifiés.</span><span class="sxs-lookup"><span data-stu-id="23c32-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="23c32-186">Notez que les instances de DbEntityEntry doit toujours contenant une entité non null.</span><span class="sxs-lookup"><span data-stu-id="23c32-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="23c32-187">Les entrées de relation et des entrées de stub ne sont pas représentées en tant qu’instances de DbEntityEntry il est donc inutile pour filtrer ces.</span><span class="sxs-lookup"><span data-stu-id="23c32-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
