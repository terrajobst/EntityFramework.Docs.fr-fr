---
title: 'Chargement des données associées : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 65cfea07a40939c1c3615c97ec785a4082b21de5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994786"
---
# <a name="loading-related-data"></a>Chargement des données associées

Entity Framework Core vous permet d’utiliser les propriétés de navigation dans votre modèle pour charger des entités associées. Il existe trois modèles O/RM communs utilisés pour charger les données associées.
* Le **Chargement hâtif** signifie que les données associées sont chargées à partir de la base de données dans le cadre de la requête initiale.
* Le **Chargement explicite** signifie que les données associées sont explicitement chargées à partir de la base de données à un moment ultérieur.
* Le **Chargement différé** signifie que les données associées sont chargées de façon transparente à partir de la base de données lors de l’accès à la propriété de navigation.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="eager-loading"></a>Chargement hâtif

Vous pouvez utiliser la méthode `Include` pour spécifier les données associées à inclure dans les résultats de la requête. Dans l’exemple suivant, les blogs retournés dans les résultats auront leurs propriétés `Posts` remplies avec les billets associés.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte. Ainsi, même vous n’incluez pas explicitement si les données pour une propriété de navigation, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.


Vous pouvez inclure des données associées provenant de plusieurs relations dans une seule requête.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Inclusion de plusieurs niveaux

Vous pouvez descendre dans la hiérarchie des relations pour inclure plusieurs niveaux de données associées à l’aide de la méthode `ThenInclude`. L’exemple suivant charge tous les blogs, leurs messages associés et l’auteur de chaque publication.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Les versions actuelles de Visual Studio offrent des options de saisie semi-automatique de code incorrectes et peuvent provoquer le marquage d’expressions correctes avec des erreurs de syntaxe lorsque vous utilisez la méthode `ThenInclude` après une propriété de navigation de collection. Cela est causé par un bogue d’IntelliSense suivi sur https://github.com/dotnet/roslyn/issues/8237. Il est possible d’ignorer ces fausses erreurs de syntaxe tant que le code est correct et qu’il peut être compilé avec succès. 

Vous pouvez enchaîner plusieurs appels à `ThenInclude` pour continuer à inclure les niveaux de données associées suivants.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Vous pouvez combiner tout cela pour inclure les données associées de plusieurs niveaux et plusieurs racines dans la même requête.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Vous pourriez souhaiter inclure plusieurs entités associées pour une entité qui est incluse. Par exemple, lors de l’interrogation des `Blog`s, vous incluez `Posts`, puis souhaitez inclure à la fois les `Author` et les `Tags` des `Posts`. Pour ce faire, vous devez spécifier chaque chemin d’accès à inclure à partir de la racine. Par exemple : `Blog -> Posts -> Author` et `Blog -> Posts -> Tags`. Cela ne signifie pas que vous obtiendrez des jointures redondantes. Dans la plupart des cas, EF consolide les jointures lors de la génération du SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Inclure des types dérivés

Vous pouvez inclure des données associées provenant de navigations définies uniquement sur un type dérivé à l’aide de `Include` et `ThenInclude`. 

En partant du modèle suivant :

```csharp
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

Le contenu de la navigation `School` de toutes les personnes qui sont des étudiants peut être chargé dynamiquement à l’aide d’un certain nombre de modèles :

- utilisation du forçage de type
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- utilisation de l’opérateur `as`
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- utilisation de la surcharge de `Include` qui accepte les paramètres de type `string`
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Inclusions ignorées

Si vous modifiez la requête afin qu’elle ne renvoie plus les instances du type d’entité par lequel la requête a commencé, les opérateurs include sont ignorés.

Dans l’exemple suivant, les opérateurs include sont basés sur le `Blog`, puis l’opérateur `Select` est utilisé pour modifier la requête pour retourner un type anonyme. Dans ce cas, les opérateurs include n’ont aucun effet.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Par défaut, EF Core consigne un avertissement lorsque les opérateurs include sont ignorés. Consultez [Journalisation](../miscellaneous/logging.md) pour plus d’informations sur l’affichage de la sortie de la journalisation. Vous pouvez modifier le comportement lorsqu’un opérateur include est ignoré, entre journaliser ou ne rien faire. Cette opération est effectuée lors de la définition des options pour votre contexte, généralement dans `DbContext.OnConfiguring`, ou `Startup.cs` si vous utilisez ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Chargement explicite

> [!NOTE]  
> Cette fonctionnalité a été introduite dans EF Core 1.1.

Vous pouvez charger explicitement une propriété de navigation via l’API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Vous pouvez également explicitement charger une propriété de navigation en exécutant une requête distincte qui retourne les entités associées. Si le suivi des modifications est activé, lors du chargement d’une entité, EF Core définit automatiquement les propriétés de navigation de l’entité qui vient d’être chargée pour faire référence à toutes les entités déjà chargées et définir les propriétés de navigation des entités déjà chargées pour faire référence à l’entité qui vient d’être chargée.

### <a name="querying-related-entities"></a>Interrogation des entités associées

Vous pouvez également obtenir une requête LINQ qui représente le contenu d’une propriété de navigation.

Cela vous permet d’effectuer des opérations telles que l’exécution d’un opérateur d’agrégation sur les entités associées sans les charger dans la mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Vous pouvez également filtrer les entités associées qui sont chargées en mémoire.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Chargement différé

> [!NOTE]  
> Cette fonctionnalité a été introduite dans EF Core 2.1.

La façon la plus simple d’utiliser le chargement différé est d’installer le package [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) et de l’activer avec un appel à `UseLazyLoadingProxies`. Exemple :
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Ou bien, lors de l’utilisation d’AddDbContext :
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core active ensuite le chargement différé pour n’importe quelle propriété de navigation qui peut être substituée, c’est-à-dire qui doit être `virtual` et sur une classe qui peut être héritée. Par exemple, dans les entités suivantes, les propriétés de navigation `Post.Blog` et `Blog.Posts` seront chargées en différé.
```csharp
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
### <a name="lazy-loading-without-proxies"></a>Chargement différé sans proxy

Les proxys à chargement différé fonctionnent en injectant le service `ILazyLoader` dans une entité, comme décrit dans [Constructeurs de type d’entité](../modeling/constructors.md). Exemple :
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Il n’est pas nécessaire que l’héritage des types d’entité à partir des propriétés de navigation soit virtuel, et cela permet aux instances d’entité créées avec `new` d’être chargées en différé une fois jointes à un contexte. Toutefois, il requiert une référence au service `ILazyLoader`, qui est défini dans le package [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/). Ce package contient un ensemble minimal de types, de sorte que dépendre de celui-ci n’a qu’un impact très limité. Toutefois, pour éviter complètement de dépendre des packages EF Core dans les types d’entité, il est possible d’injecter la méthode `ILazyLoader.Load` en tant que délégué. Exemple :
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
Le code ci-dessus utilise une méthode d'extension `Load` pour rendre l’utilisation du délégué un peu plus propre :
```csharp
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
> Le paramètre de constructeur pour le délégué à chargement différé doit être appelé « lazyLoader ». La configuration permettant d’utiliser un nom différent est prévue pour une version ultérieure.

## <a name="related-data-and-serialization"></a>Données associées et sérialisation

Étant donné qu’EF Core corrigera automatiquement les propriétés de navigation, vous pouvez vous retrouver avec des cycles dans votre graphique d’objets. Par exemple, le chargement d’un blog et de ses billets associés aboutit à un objet de blog qui fait référence à une collection de billets. Chacun de ces billets aura une référence vers le blog.

Certaines infrastructures de sérialisation n’autorisent pas de tels cycles. Par exemple, Json.NET lève l’exception suivante si un cycle est rencontré.

> Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.

Si vous utilisez ASP.NET Core, vous pouvez configurer Json.NET pour ignorer les cycles qu’il trouve dans le graphique d’objets. Cette opération est effectuée dans la méthode `ConfigureServices(...)` de `Startup.cs`.

```csharp
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
