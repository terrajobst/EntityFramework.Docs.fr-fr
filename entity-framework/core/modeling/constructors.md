---
title: Types d’entités avec des constructeurs - EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 5bf49718f02c1860871b1f4c255ec4d98fce2fc7
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405243"
---
# <a name="entity-types-with-constructors"></a>Types d’entités avec des constructeurs

> [!NOTE]  
> Cette fonctionnalité est une nouveauté d’EF Core 2.1.

À compter d’EF Core 2.1, il est désormais possible de définir un constructeur avec des paramètres et appeler ce constructeur lorsque vous créez une instance de l’entité de EF Core. Les paramètres du constructeur peuvent être liés à la propriété mappée, ou à différents types de services pour faciliter les comportements, tels que chargement différé.

> [!NOTE]  
> À compter d’EF Core 2.1, tous les liaison constructeur est par convention. Configuration des constructeurs spécifiques à utiliser est prévue pour une version ultérieure.

## <a name="binding-to-mapped-properties"></a>Liaison aux propriétés mappées

Considérez un modèle de billet de Blog/classique :

``` csharp
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

Quand EF Core crée des instances de ces types, comme pour les résultats d’une requête, il sera tout d’abord appeler le constructeur sans paramètre par défaut et définissez chaque propriété sur la valeur à partir de la base de données. Toutefois, si EF Core recherche un constructeur paramétrable avec des noms de paramètre et les types qui correspondent à ceux de mappé des propriétés, puis il appelle à la place du constructeur paramétré avec des valeurs pour ces propriétés et ne définit pas explicitement de chaque propriété. Exemple :

``` csharp
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
Voici quelques éléments à noter :
* Toutes les propriétés ne doivent avoir des paramètres du constructeur. Par exemple, la propriété de Post.Content n’est pas définie par n’importe quel paramètre de constructeur, donc EF Core est la valeur après l’appel du constructeur de façon normale.
* Les noms et types de paramètres doivent correspondre à des types de propriété et les noms, à ceci près que les propriétés peuvent être casse Pascal tandis que les paramètres sont de casse mixte.
* EF Core ne peut pas définir les propriétés de navigation (par exemple, Blog ou les billets ci-dessus) à l’aide d’un constructeur.
* Le constructeur peut être public, privé, ou avoir n’importe quelle autre accessibilité. Toutefois, les proxys de chargement différé requièrent que le constructeur est accessible à partir de la classe qui hérite de proxy. Cela signifie généralement rendant public ou protégé.

### <a name="read-only-properties"></a>Propriétés en lecture seule

Une fois que les propriétés sont définies via le constructeur, il peut être judicieux pour rendre certains d'entre eux en lecture seule. EF Core prend en charge, mais il existe quelques éléments à rechercher :
* Propriétés sans accesseurs Set ne sont pas mappées par convention. (Cela a tendance à mapper des propriétés qui ne doivent pas être mappées, telles que les propriétés calculées.)
* À l’aide de valeurs de clés générées automatiquement nécessite une propriété de clé qui est en lecture-écriture, dans la mesure où la valeur de clé doit être définie par le Générateur de clé lors de l’insertion de nouvelles entités.

Un moyen simple pour éviter ces choses consiste à utiliser les méthodes Set privées. Exemple :
``` csharp
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
EF Core voit une propriété avec un setter privée en lecture-écriture, ce qui signifie que toutes les propriétés sont mappées comme auparavant et la clé peut toujours être générées par le magasin.

Une alternative à l’utilisation de méthodes setter privée consiste à rendre les propriétés vraiment en lecture seule et ajouter un mappage plus explicite dans OnModelCreating. De même, certaines propriétés peuvent être complètement supprimées et remplacées par uniquement des champs. Par exemple, considérez ces types d’entités :

``` csharp
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
``` csharp
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
* La clé « property » est désormais un champ. Il n’est pas un `readonly` afin que les clés générées par le magasin peuvent être utilisées.
* Les autres propriétés sont en lecture seule défini uniquement dans le constructeur.
* Si la valeur de clé primaire est toujours définie par Entity Framework ou lire à partir de la base de données, il est inutile d’inclure cela dans le constructeur. Cela laisse la clé « property » comme un champ simple et permet d’identifier clairement qu’il ne doit pas être défini explicitement lors de la création de nouveaux blogs ou les billets.

> [!NOTE]  
> Ce code entraîne '169' indiquant que le champ n’est jamais utilisé d’avertissement du compilateur. Cela peut être ignoré, car en réalité EF Core utilise le champ de manière extralinguistic.

## <a name="injecting-services"></a>Injection de services

EF Core peut également injecter des « services » dans le constructeur du type d’une entité. Par exemple, ce qui suit peut être injecté :
* `DbContext` -l’instance de contexte actuel, ce qui peut également être tapé sous forme votre type DbContext dérivée
* `ILazyLoader` -le service de chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations
* `Action<object, string>` -un délégué sur le chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations
* `IEntityType` -les métadonnées de EF Core associées à ce type d’entité

> [!NOTE]  
> À compter d’EF Core 2.1, seuls les services connus par EF Core peuvent être injectées. Prise en charge d’injection de services d’application est envisagée pour une version ultérieure.

Par exemple, un DbContext injecté peut être utilisé pour accéder de façon sélective la base de données pour obtenir des informations sur les entités connexes sans charger toutes les. Dans l’exemple ci-dessous, cela est utilisé pour obtenir le nombre de billets dans un blog sans charger les billets de :

``` csharp
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
Choses à remarquer à ce sujet :
* Le constructeur est privé, dans la mesure où elle est toujours appelée par EF Core, et il existe un autre constructeur public pour une utilisation générale.
* Le code qui utilise le service injecté (autrement dit, le contexte) est la défense contre lui en cours `null` pour gérer les cas où EF Core n’est pas création de l’instance.
* Étant donné que le service est stocké dans une propriété en lecture/écriture, elle sera réinitialisée lorsque l’entité est attachée à une nouvelle instance de contexte.

> [!WARNING]  
> Injecter le DbContext, comme cela est souvent considéré comme un anti-modèle dans la mesure où elle associe vos types d’entité directement vers EF Core. Étudiez attentivement toutes les options avant d’utiliser l’injection de service comme suit.
