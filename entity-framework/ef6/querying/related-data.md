---
title: Le chargement d’entités - EF6 associées
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121146"
---
# <a name="loading-related-entities"></a>Chargement des entités connexes
Entity Framework prend en charge les trois façons de charger les données associées - chargement hâtif, le chargement différé et le chargement explicite. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

## <a name="eagerly-loading"></a>Chargement de manière anticipée  

Le chargement hâtif est le processus par lequel une requête pour un type d’entité charge également les entités associées dans le cadre de la requête. Le chargement hâtif est obtenu à l’aide de la méthode Include. Par exemple, les requêtes ci-dessous chargera les blogs et toutes les publications relatives à chaque blog.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

Notez que Include est une méthode d’extension dans l’espace de noms System.Data.Entity Veillez donc à l’aide de cet espace de noms.  

### <a name="eagerly-loading-multiple-levels"></a>Chargement de manière anticipée plusieurs niveaux  

Il est également possible de charger plusieurs niveaux d’entités associées de manière anticipée. Les requêtes ci-dessous illustrent comment effectuer cette opération pour la collecte et de propriétés de navigation de référence.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Notez qu’il n’est pas encore possible de filtrer les entités associées sont chargées. Inclure sera toujours apporter dans toutes les entités.  

## <a name="lazy-loading"></a>Chargement différé  

Le chargement différé est le processus par lequel une entité ou une collection d’entités est automatiquement chargée à partir de la base de données la première fois que l’accès à une propriété qui fait référence à l’entité/entités. Lorsque vous utilisez des types d’entités POCO, le chargement différé est obtenu en créant des instances de types de proxy dérivée et puis en substituant les propriétés virtuelles pour ajouter le raccordement de chargement. Par exemple, lorsque vous utilisez la classe d’entité Blog définie ci-dessous, les messages liés seront chargées la première fois que la propriété de navigation de billets est accessible :  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a>Activer le chargement différé désactivé pour sérialisation  

Sérialisation et le chargement différé ne mélangez pas correctement, et si vous ne faites pas attention vous pouvez retrouver d’interrogation pour votre base de données entière juste, car le chargement différé est activé. La plupart des sérialiseurs fonctionnent en accédant à chaque propriété sur une instance d’un type. Accès à la propriété déclenche le chargement différé, donc plusieurs entités sont sérialisées. Sur ces entités les propriétés sont accessibles, et d’autres entités sont chargées. Il est conseillé d’activer le chargement désactivé avant de vous sérialisez une entité différé. Les sections suivantes montrent comment effectuer cette opération.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Désactivation de chargement différé pour les propriétés de navigation spécifique  

Le chargement différé de la collection de billets peut être désactivé en rendant la propriété de billets non virtuelle :  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

Le chargement des billets de collection encore possible à l’aide d’un chargement hâtif (consultez *chargement de manière anticipée* ci-dessus) ou la méthode Load (consultez *chargement explicite* ci-dessous).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Désactivez chargement différé pour toutes les entités  

Le chargement différé peut être désactivé pour toutes les entités dans le contexte en définissant un indicateur sur la propriété de Configuration. Exemple :  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Chargement des entités connexes encore possible à l’aide d’un chargement hâtif (consultez *chargement de manière anticipée* ci-dessus) ou la méthode Load (consultez *chargement explicite* ci-dessous).  

## <a name="explicitly-loading"></a>Chargement explicite  

Même lorsque le chargement différé désactivé, il est toujours possible de charger en différé des entités associées, mais elle doit être effectuée avec un appel explicite. Pour ce faire, vous utilisez la méthode de charge sur l’entrée de l’entité associée. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

Notez que la méthode de référence doit être utilisée lorsqu’une entité a une propriété de navigation à une autre entité unique. En revanche, la méthode de collecte doit être utilisée lorsqu’une entité a une propriété de navigation à une collection d’autres entités.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Appliquer des filtres lors du chargement explicite d’entités associées  

La méthode de requête fournit l’accès à la requête sous-jacente qui utilise Entity Framework lors du chargement des entités associées. Vous pouvez ensuite utiliser LINQ pour appliquer des filtres à la requête avant de l’exécuter avec un appel à une méthode d’extension LINQ comme ToList, charge, etc. La méthode de requête peut être utilisée avec les propriétés de navigation de référence et de collection, mais est particulièrement utile pour les collections, où il peut être utilisé pour charger uniquement une partie de la collection. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

Lorsque vous utilisez la méthode de requête, il est généralement préférable désactiver le chargement différé pour la propriété de navigation. Il s’agit, car sinon la collection entière peut sont chargée automatiquement par le mécanisme de chargement différé avant ou après l’exécution de la requête filtrée.  

Notez que lors de la relation peut être spécifiée sous forme de chaîne au lieu d’une expression lambda, IQueryable retournée n’est pas générique lorsqu’une chaîne est utilisée et la méthode de conversion n’est donc généralement nécessaire avant que quelque chose d’utile peut être fait avec elle.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>À l’aide de la requête pour compter les entités associées sans leur chargement  

Il est parfois utile de connaître le nombre d’entités est lié à une autre entité dans la base de données sans réellement impliquer le coût du chargement de toutes ces entités. La méthode de requête avec la méthode Count de LINQ peut être utilisée pour ce faire. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
