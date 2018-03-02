---
title: "Chargement associées données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: dadc6235c3879ae27ad5c99988a5e594872045df
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="loading-related-data"></a>Chargement de données connexes

Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées. Il existe trois modèles d’O/RM communs utilisés pour charger les données associées.
* **Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.
* **Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à une date ultérieure.
* **Chargement différé** signifie que les données associées en toute transparence sont chargées à partir de la base de données que lorsque la propriété de navigation est accessible.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="eager-loading"></a>Chargement hâtif

Vous pouvez utiliser la `Include` méthode pour spécifier les données connexes à inclure dans les résultats de la requête. Dans l’exemple suivant, les blogs sont retournés dans les résultats auront leurs `Posts` propriété remplie avec les messages liés.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core va corriger automatiquement des propriétés de navigation à toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte. Par conséquent, même si vous n’incluez pas explicitement les données pour une propriété de navigation, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.


Vous pouvez inclure des données liées provenant de plusieurs relations dans une requête unique.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Y compris plusieurs niveaux

Vous pouvez descendre, par le biais des relations à inclure plusieurs niveaux de données connexes à l’aide du `ThenInclude` (méthode). L’exemple suivant charge tous les blogs, leurs messages liés et l’auteur de chaque publication.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrect et peut provoquer des expressions correctes marquage avec des erreurs de syntaxe lorsque vous utilisez la `ThenInclude` méthode après une propriété de navigation de collection. Ceci est le symptôme d’un bogue IntelliSense suivi à https://github.com/dotnet/roslyn/issues/8237. Il est possible d’ignorer ces erreurs de syntaxe fausses tant que le code est correct et qu’il peut être compilé avec succès. 

Vous pouvez chaîner plusieurs appels à `ThenInclude` pour continuer, notamment les prochaines niveaux de données associées.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Vous pouvez combiner tous de cette option pour inclure les données associées à partir de plusieurs niveaux et plusieurs racines dans la même requête.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Voulez-vous inclure plusieurs entités associées pour l’une des entités qui est incluse. Par exemple, lors de l’interrogation `Blog`s, vous incluez `Posts` , puis à inclure à la fois le `Author` et `Tags` de la `Posts`. Pour ce faire, vous devez spécifier chaque inclure le chemin d’accès à partir de la racine. Par exemple, `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`. Cela ne signifie pas que vous obtiendrez des jointures redondantes, dans la plupart des cas QU'EF consolide les jointures lors de la génération SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Inclure dans les types dérivés

Vous pouvez inclure des données liées provenant uniquement définies sur un type dérivé à l’aide de navigations `Include` et `ThenInclude`. 

Étant donné le modèle suivant :

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

Contenu de `School` navigation de toutes les personnes qui sont les étudiants peut être chargée dynamiquement à l’aide d’un certain nombre de modèles :

- utilisation de cast
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- à l’aide de `as` (opérateur)
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- à l’aide de la surcharge de `Include` qui accepte le paramètre de type `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Ignoré inclut

Si vous modifiez la requête afin qu’elle retourne ne sont plus des instances de la requête a commencé avec le type d’entité, les opérateurs include sont ignorés.

Dans l’exemple suivant, les opérateurs include sont basées sur les `Blog`, puis le `Select` opérateur est utilisé pour modifier la requête pour retourner un type anonyme. Dans ce cas, les opérateurs include n’ont aucun effet.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Par défaut, EF Core consigne un avertissement lorsque incluent les opérateurs sont ignorés. Consultez [journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de journalisation. Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré lever ou ne rien faire. Cette opération est effectuée lors de la définition en général, dans les options pour votre contexte - `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Chargement explicite

> [!NOTE]  
> Cette fonctionnalité a été introduite dans EF Core 1.1.

Vous pouvez charger explicitement une propriété de navigation via la `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées. Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core automatiquement définit les propriétés de navigation de l’entité qui vient d’être chargé pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargé pour faire référence à la entité qui vient d’être chargé.

### <a name="querying-related-entities"></a>Interrogation des entités connexes

Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.

Cela vous permet de vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Vous pouvez également filtrer les entités associées sont chargées en mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Chargement différé

> [!NOTE]  
> Cette fonctionnalité a été introduite dans EF Core 2.1.

La façon la plus simple d’utiliser le chargement différé est en installant le [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package et l’activer avec un appel à `UseLazyLoadingProxies`. Exemple :
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Ou bien, lors de l’utilisation de AddDbContext :
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF Core ensuite d’activer le chargement différé pour n’importe quelle propriété de navigation qui peut être substituée--qui est, elle doit être `virtual` et sur une classe qui peut être héritée. Par exemple, dans les entités suivantes, le `Post.Blog` et `Blog.Posts` propriétés de navigation seront chargées en différé.
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a>Lazy-chargement sans proxy

Chargement Lazy proxys fonctionnent en injectant le `ILazyLoader` de service dans une entité, comme décrit dans [constructeurs de Type d’entité](../modeling/constructors.md). Exemple :
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Cela ne nécessite pas hérité de types d’entité ou de propriétés de navigation à être des ordinateurs virtuels et permet aux instances d’entité créées avec `new` charger en différé-une fois attaché à un contexte. Toutefois, il requiert une référence à le `ILazyLoader` service, qui combine des types d’entités à l’assembly EF Core. Pour éviter ce cœur EF permet la `ILazyLoader.Load` méthode être ajoutés en tant que délégué. Exemple :
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Le code ci-dessus utilise un `Load` pour rendre l’utilisation du délégué un peu plus propre méthode d’extension :
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> Le paramètre de constructeur pour le chargement différé de délégué doit être appelé « lazyLoader ». Configuration à utiliser un nom différent, qu'il est prévu pour une version ultérieure.

## <a name="related-data-and-serialization"></a>Sérialisation et les données associées

Étant donné que les propriétés de EF Core sera automatiquement correctives navigation, vous pouvez retrouver avec des cycles dans votre graphique d’objets. Par exemple, le chargement d’un blog et il est lié billets aboutit à un objet de blog qui fait référence à une collection de publications. Chacune de ces publications aura une référence vers le blog.

Certaines infrastructures de sérialisation n’autorisent pas ces cycles. Par exemple, Json.NET lève l’exception suivante si un cycle est rencontré.

> Newtonsoft.Json.JsonSerializationException : Self faisant référence à la boucle détectée pour la propriété « Blog » avec le type 'MyApplication.Models.Blog'.

Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il se trouve dans le graphique d’objets. Cette opération est effectuée le `ConfigureServices(...)` méthode dans `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
