---
title: Types d’entités avec des constructeurs - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 8cea624c295f99ef54cb8b4758642eade03c235e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812493"
---
# <a name="entity-types-with-constructors"></a>Types d’entités avec des constructeurs

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 2.1.

À compter de EF Core 2.1, il est désormais possible de définir un constructeur avec des paramètres et appeler ce constructeur lorsque vous créez une instance de l’entité de EF Core. Les paramètres du constructeur peuvent être liés aux propriétés mappées, à différents types de services pour faciliter les comportements like ou chargement différé.

> [!NOTE]  
> À compter de EF Core 2.1, toute liaison de constructeur est par convention. Configuration des constructeurs spécifiques à utiliser est prévue pour une version ultérieure.

## <a name="binding-to-mapped-properties"></a>Liaison aux propriétés mappées

Considérez un modèle/billet de Blog classique :

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

Lorsque EF Core crée des instances de ces types, tels que pour les résultats d’une requête, il sera tout d’abord appeler le constructeur sans paramètre par défaut, puis définissez chaque propriété à la valeur à partir de la base de données. Toutefois, si EF Core recherche un constructeur paramétrable avec des noms de paramètre et les types qui correspondent à ceux de mappé des propriétés, puis il appelle à la place du constructeur paramétré avec des valeurs pour ces propriétés et ne définit pas explicitement de chaque propriété. Par exemple :

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Certains points à noter :
* Toutes les propriétés ne doivent avoir des paramètres du constructeur. Par exemple, la propriété de Post.Content n’est pas définie par un paramètre de constructeur, donc EF Core définirai ces informations après avoir appelé le constructeur de façon normale.
* Les noms et types de paramètres doivent correspondre à des types de propriété et les noms, à ceci près que les propriétés peuvent être casse Pascal tandis que les paramètres sont de casse mixte.
* EF principal ne peut pas définir les propriétés de navigation (par exemple, le Blog ou publications ci-dessus) à l’aide d’un constructeur.
* Le constructeur peut être public, privé, ou avoir n’importe quelle autre accessibilité.

### <a name="read-only-properties"></a>Propriétés en lecture seule

Une fois que les propriétés sont définies via le constructeur, il peut être judicieux pour rendre certains d'entre eux en lecture seule. EF Core prend en charge, mais certains éléments à prendre en compte :
* Propriétés sans méthodes setter ne sont pas mappées par convention. (Cela a tendance à mapper les propriétés qui ne doivent pas être mappées, telles que les propriétés calculées.)
* À l’aide des valeurs de clés générées automatiquement nécessite une propriété de clé qui est en lecture-écriture, étant donné que la valeur de clé doit être définie par le Générateur de clé lors de l’insertion de nouvelles entités.

Un moyen simple pour éviter ces éléments consiste à utiliser les méthodes setter privé. Par exemple :
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```
EF Core voit une propriété avec un setter privé en lecture-écriture, ce qui signifie que toutes les propriétés sont mappées comme avant, et la clé peut toujours être générées par le magasin.

Une alternative à l’utilisation de méthodes setter privé consiste à définir des propriétés réellement en lecture seule et ajouter un mappage plus explicite dans OnModelCreating. De même, certaines propriétés peuvent être complètement supprimées et remplacées par uniquement des champs. Par exemple, considérez ces types d’entités :

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
Et cette configuration dans OnModelCreating :
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
Points à noter :
* La clé « property » est désormais un champ. Il n’est pas un `readonly` afin que les clés générées par le magasin peuvent être utilisés. 
* Les autres propriétés sont en lecture seule défini uniquement dans le constructeur.
* Si la valeur de clé primaire est toujours définie par EF ou lire à partir de la base de données, puis il n’est pas nécessaire d’inclure dans le constructeur. Cela laisse la clé « property » comme un champ de type simple et indique clairement qu’il ne doit pas être défini explicitement lors de la création de nouveaux blogs ou des publications.

> [!NOTE]  
> Ce code génère avertissement '169' indiquant que le champ n’est jamais utilisé. Cela peut être ignoré, car il est en réalité EF Core utilise le champ de manière extralinguistic.

## <a name="injecting-services"></a>Injection des services

EF Core pouvez injecter également un « services » dans le constructeur du type d’une entité. Par exemple, les éléments suivants peuvent être ajoutés :
* `DbContext` -l’instance de contexte actuel, qui peut également être de type votre type dérivé de DbContext
* `ILazyLoader` -le service de chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations
* `Action<object, string>` -un délégué de chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations
* `IEntityType` -les métadonnées de EF principales associées à ce type d’entité

> [!NOTE]  
> À compter de EF Core 2.1, uniquement les services connus EF Core peuvent être ajoutées. Prise en charge pour l’injection des services d’application est envisagée pour une version ultérieure.

Par exemple, un DbContext injecté peut servir à sélectivement accéder à la base de données pour obtenir des informations sur les entités associées sans charger toutes les. Dans l’exemple ci-dessous, cela est utilisé pour obtenir le nombre de publications de blog de sans charger les billets de :

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Choses à noter à ce sujet :
* Le constructeur est privé, car il est toujours appelée par EF Core, et il existe un autre constructeur public pour une utilisation générale.
* Le code qui utilise le service injecté (autrement dit, le contexte) est défense contre lui étant `null` pour gérer les cas où EF Core n’est pas création de l’instance.
* Étant donné que le service est stocké dans une propriété en lecture/écriture, elle sera réinitialisée lorsque l’entité est attachée à une nouvelle instance de contexte.

> [!WARNING]  
> Injection le DbContext comme cela est souvent considéré un anti-modèle, car il associe vos types d’entité directement à EF Core. Étudiez attentivement toutes les options avant d’utiliser l’injection des services comme suit.
