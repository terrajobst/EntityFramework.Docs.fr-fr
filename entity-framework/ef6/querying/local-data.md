---
title: Données locales - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: dac1a1de20398501c706b118443743d47970df17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994272"
---
# <a name="local-data"></a>Données locales
Exécuter une requête LINQ directement sur un DbSet toujours enverra une requête à la base de données, mais vous pouvez accéder aux données qui sont actuellement en mémoire à l’aide de la propriété DbSet.Local. Vous pouvez également accéder à l’Entity Framework effectue le suivi des informations supplémentaires sur vos entités avec les méthodes DbContext.Entry et DbContext.ChangeTracker.Entries. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

## <a name="using-local-to-look-at-local-data"></a>À l’aide locale pour examiner les données locales  

La propriété locale de DbSet fournit un accès simple aux entités du jeu qui sont actuellement suivis par le contexte et n’ont pas été marquées comme supprimées. L’accès à la propriété locale n’entraîne jamais une requête à envoyer à la base de données. Cela signifie qu’il est généralement utilisé après qu’une requête a déjà été effectuée. La méthode d’extension de charge peut être utilisée pour exécuter une requête afin que le contexte suit les résultats. Exemple :  

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

Si nous avions deux blogs dans la base de données - « ADO.NET Blog » avec un BlogId 1 - et « le Blog Visual Studio' avec un BlogId de 2, nous aurions pu obtenir la sortie suivante :  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Cela illustre trois points :  

- Le nouveau blog « Mon nouveau Blog » est inclus dans la collection locale, même si elle n’a pas encore été enregistré dans la base de données. Ce blog a une clé primaire égale à zéro, car la base de données n’a pas encore généré une clé réelle de l’entité.  
- Le Blog de ADO.NET n’est pas inclus dans la collection locale, même si elle est toujours suivie par le contexte. Il s’agit, car nous avons retiré le DbSet ainsi marquant comme étant supprimé.  
- Lorsque DbSet est utilisé pour effectuer une requête le blog marqué pour suppression (Blog ADO.NET) est inclus dans les résultats et le nouveau blog (Blog de nouveau mon) qui n’a pas encore été enregistré à la base de données n’est pas inclus dans les résultats. Il s’agit, car le DbSet effectue une requête sur la base de données et les résultats retournés toujours refléter les informations spécifiées dans la base de données.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>À l’aide locale pour ajouter et supprimer des entités à partir du contexte  

La propriété locale sur DbSet retourne un [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) avec événements raccordés telle qu’elle reste synchronisée avec le contenu du contexte. Cela signifie que les entités peuvent être ajoutées ou supprimées de la collection locale ou le DbSet. Cela signifie également que les requêtes qui apportent de nouvelles entités dans le contexte entraîne la collection locale en cours de mise à jour avec ces entités. Exemple :  

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

En supposant que nous avions quelques billets marqués avec « entity framework » et « asp.net » la sortie peut ressembler à ceci :  

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

Cela illustre trois points :  

- La nouvelle publication, « Quelles sont les nouveautés dans EF » qui a été ajouté à la variable locale collection devienne suivie par le contexte dans l’état Added. Il sera par conséquent insérée dans la base de données lorsque SaveChanges est appelée.  
- Le billet a été supprimé de la collection locale (Guide du débutant en EF) est désormais marqué comme supprimée dans le contexte. Il sera par conséquent être supprimé à partir de la base de données lorsque SaveChanges est appelée.  
- Le billet supplémentaire (Guide du débutant en ASP.NET) chargé dans le contexte avec la deuxième requête est automatiquement ajouté à la collection locale.  

Une dernière chose à noter concernant Local qui est car il s’agit de qu'une performance ObservableCollection n’est pas satisfaisante pour un grand nombre d’entités. Si vous êtes confronté à des milliers d’entités dans votre contexte il seront donc ne peut-être pas recommandé d’utiliser Local.  

## <a name="using-local-for-wpf-data-binding"></a>À l’aide locale pour la liaison de données WPF  

La propriété locale sur DbSet peut être utilisée directement pour la liaison de données dans une application WPF s’agissant d’une instance de ObservableCollection. Comme décrit dans les sections précédentes, que cela signifie qu’il sera automatiquement synchronisée avec le contenu du contexte et le contenu du contexte est automatiquement synchronisés avec lui. Notez que vous n’avez pas besoin de préremplir la collection locale avec les données soient rien à lier dans la mesure où Local ne provoque jamais une requête de base de données.  

Ce n’est pas un emplacement approprié pour un exemple de liaison de données WPF complet, mais les éléments clés sont :  

- Le programme d’installation d’une source de liaison  
- Lier à la propriété locale de votre jeu  
- Remplir Local à l’aide d’une requête à la base de données.  

## <a name="wpf-binding-to-navigation-properties"></a>Liaison de WPF pour les propriétés de navigation  

Si vous effectuez des liaisons de données maître/détail peuvent souhaiter lier l’affichage des détails à une propriété de navigation d’un de vos entités. Un moyen simple pour y parvenir consiste à utiliser ObservableCollection pour la propriété de navigation. Exemple :  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>À l’aide locale pour nettoyer les entités dans SaveChanges  

Dans la plupart des cas les entités supprimées à partir d’une propriété de navigation ne seront pas automatiquement marquées comme étant supprimées dans le contexte. Par exemple, si vous supprimez un objet de publication à partir de la collection Blog.Posts puis qui publient n'est pas supprimées automatiquement lorsque SaveChanges est appelée. Si vous avez besoin pour être supprimés puis vous devrez peut-être rechercher ces entités non résolues et de les marquer comme étant supprimé avant l’appel de SaveChanges ou comme partie d’un SaveChanges substituée. Exemple :  

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

Le code ci-dessus utilise la collection locale pour rechercher toutes les publications et les marques de ceux qui n’ont pas une référence de blog comme étant supprimé. L’appel ToList est nécessaire, car sinon la collection sera modifiée par la suppression appel pendant son énumération. Dans la plupart des autres situations, vous pouvez interroger directement par rapport à la propriété locale sans utiliser ToList tout d’abord.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>À l’aide de la liaison de données locale et ToBindingList for Windows Forms  

Windows Forms ne prend pas en charge la liaison de données d’une fidélité optimale à l’aide de ObservableCollection directement. Toutefois, vous pouvez toujours utiliser la propriété DbSet locale pour la liaison de données pour obtenir tous les avantages décrits dans les sections précédentes. Ceci se fait via la méthode d’extension ToBindingList qui crée un [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implémentation est garantie par ObservableCollection Local.  

Ce n’est pas un emplacement approprié pour un exemple de liaison de données Windows Forms complet, mais les éléments clés sont :  

- Le programme d’installation d’un objet source de liaison  
- Lier à la propriété locale de votre jeu à l’aide de Local.ToBindingList()  
- Remplir Local à l’aide d’une requête à la base de données  

## <a name="getting-detailed-information-about-tracked-entities"></a>Obtenir des informations détaillées sur les entités suivies  

La plupart des exemples de cette série utilisent la méthode d’entrée pour retourner une instance DbEntityEntry pour une entité. Cet objet d’entrée agit alors comme point de départ pour la collecte des informations sur l’entité telles que son état actuel, ainsi que pour effectuer des opérations sur l’entité telles que le chargement explicite d’une entité associée.  

Les méthodes d’entrées retournent des objets DbEntityEntry pour nombreuses ou toutes les entités suivies par le contexte. Cela vous permet de collecter des informations ou effectuer des opérations sur nombreuses entités plutôt que simplement une seule entrée. Exemple :  

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

Vous remarquerez que nous avons introduit une classe de l’auteur et le lecteur dans l’exemple : les deux de ces classes implémentent l’interface IPerson.  

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

Supposons que nous avons les données suivantes dans la base de données :

Blog avec BlogId = 1 et le nom = 'ADO.NET Blog'  
Blog avec BlogId = 2, nom = 'Le Blog de Visual Studio'  
Blog avec BlogId = 3 et le nom = 'Blog.NET Framework'  
Auteur avec AuthorId = 1 et le nom = 'Joe Bloggs'  
Lecteur avec ReaderId = 1 et le nom = « Jean Dupont »  

La sortie à partir de l’exécution du code serait :  

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

Les exemples suivants illustrent plusieurs points :  

- Les méthodes d’entrées renvoient les entrées pour les entités dans tous les États, y compris Deleted. Comparez cela à Local qui exclut supprimé des entités.  
- Entrées pour tous les types d’entités sont retournées lorsque la méthode d’entrées non générique est utilisée. Lors de l’utilisation de la méthode générique entrées entrées sont retournées uniquement pour les entités qui sont des instances du type générique. Cela a été utilisée ci-dessus pour obtenir des entrées pour tous les blogs. Elle était également utilisée pour obtenir les entrées pour toutes les entités qui implémentent IPerson. Cet exemple montre que le type générique ne devra pas être un type d’entité réelle.  
- LINQ aux objets peut être utilisé pour filtrer les résultats retournés. Cela était utilisée ci-dessus pour rechercher des entités de n’importe quel type, que s’ils sont modifiés.  

Notez que les instances de DbEntityEntry doit toujours contenant une entité non null. Les entrées de relation et des entrées de stub ne sont pas représentées en tant qu’instances de DbEntityEntry il est donc inutile pour filtrer ces.
