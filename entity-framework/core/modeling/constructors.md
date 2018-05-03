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
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="45c83-102">Types d’entités avec des constructeurs</span><span class="sxs-lookup"><span data-stu-id="45c83-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="45c83-103">Cette fonctionnalité est une nouveauté dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="45c83-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="45c83-104">À compter de EF Core 2.1, il est désormais possible de définir un constructeur avec des paramètres et appeler ce constructeur lorsque vous créez une instance de l’entité de EF Core.</span><span class="sxs-lookup"><span data-stu-id="45c83-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="45c83-105">Les paramètres du constructeur peuvent être liés aux propriétés mappées, à différents types de services pour faciliter les comportements like ou chargement différé.</span><span class="sxs-lookup"><span data-stu-id="45c83-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="45c83-106">À compter de EF Core 2.1, toute liaison de constructeur est par convention.</span><span class="sxs-lookup"><span data-stu-id="45c83-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="45c83-107">Configuration des constructeurs spécifiques à utiliser est prévue pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="45c83-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="45c83-108">Liaison aux propriétés mappées</span><span class="sxs-lookup"><span data-stu-id="45c83-108">Binding to mapped properties</span></span>

<span data-ttu-id="45c83-109">Considérez un modèle/billet de Blog classique :</span><span class="sxs-lookup"><span data-stu-id="45c83-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="45c83-110">Lorsque EF Core crée des instances de ces types, tels que pour les résultats d’une requête, il sera tout d’abord appeler le constructeur sans paramètre par défaut, puis définissez chaque propriété à la valeur à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="45c83-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="45c83-111">Toutefois, si EF Core recherche un constructeur paramétrable avec des noms de paramètre et les types qui correspondent à ceux de mappé des propriétés, puis il appelle à la place du constructeur paramétré avec des valeurs pour ces propriétés et ne définit pas explicitement de chaque propriété.</span><span class="sxs-lookup"><span data-stu-id="45c83-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="45c83-112">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="45c83-112">For example:</span></span>

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
<span data-ttu-id="45c83-113">Certains points à noter :</span><span class="sxs-lookup"><span data-stu-id="45c83-113">Some things to note:</span></span>
* <span data-ttu-id="45c83-114">Toutes les propriétés ne doivent avoir des paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="45c83-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="45c83-115">Par exemple, la propriété de Post.Content n’est pas définie par un paramètre de constructeur, donc EF Core définirai ces informations après avoir appelé le constructeur de façon normale.</span><span class="sxs-lookup"><span data-stu-id="45c83-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="45c83-116">Les noms et types de paramètres doivent correspondre à des types de propriété et les noms, à ceci près que les propriétés peuvent être casse Pascal tandis que les paramètres sont de casse mixte.</span><span class="sxs-lookup"><span data-stu-id="45c83-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="45c83-117">EF principal ne peut pas définir les propriétés de navigation (par exemple, le Blog ou publications ci-dessus) à l’aide d’un constructeur.</span><span class="sxs-lookup"><span data-stu-id="45c83-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="45c83-118">Le constructeur peut être public, privé, ou avoir n’importe quelle autre accessibilité.</span><span class="sxs-lookup"><span data-stu-id="45c83-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="45c83-119">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="45c83-119">Read-only properties</span></span>

<span data-ttu-id="45c83-120">Une fois que les propriétés sont définies via le constructeur, il peut être judicieux pour rendre certains d'entre eux en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="45c83-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="45c83-121">EF Core prend en charge, mais certains éléments à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="45c83-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="45c83-122">Propriétés sans méthodes setter ne sont pas mappées par convention.</span><span class="sxs-lookup"><span data-stu-id="45c83-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="45c83-123">(Cela a tendance à mapper les propriétés qui ne doivent pas être mappées, telles que les propriétés calculées.)</span><span class="sxs-lookup"><span data-stu-id="45c83-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="45c83-124">À l’aide des valeurs de clés générées automatiquement nécessite une propriété de clé qui est en lecture-écriture, étant donné que la valeur de clé doit être définie par le Générateur de clé lors de l’insertion de nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="45c83-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="45c83-125">Un moyen simple pour éviter ces éléments consiste à utiliser les méthodes setter privé.</span><span class="sxs-lookup"><span data-stu-id="45c83-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="45c83-126">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="45c83-126">For example:</span></span>
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
<span data-ttu-id="45c83-127">EF Core voit une propriété avec un setter privé en lecture-écriture, ce qui signifie que toutes les propriétés sont mappées comme avant, et la clé peut toujours être générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="45c83-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="45c83-128">Une alternative à l’utilisation de méthodes setter privé consiste à définir des propriétés réellement en lecture seule et ajouter un mappage plus explicite dans OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="45c83-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="45c83-129">De même, certaines propriétés peuvent être complètement supprimées et remplacées par uniquement des champs.</span><span class="sxs-lookup"><span data-stu-id="45c83-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="45c83-130">Par exemple, considérez ces types d’entités :</span><span class="sxs-lookup"><span data-stu-id="45c83-130">For example, consider these entity types:</span></span>

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
<span data-ttu-id="45c83-131">Et cette configuration dans OnModelCreating :</span><span class="sxs-lookup"><span data-stu-id="45c83-131">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="45c83-132">Points à noter :</span><span class="sxs-lookup"><span data-stu-id="45c83-132">Things to note:</span></span>
* <span data-ttu-id="45c83-133">La clé « property » est désormais un champ.</span><span class="sxs-lookup"><span data-stu-id="45c83-133">The key "property" is now a field.</span></span> <span data-ttu-id="45c83-134">Il n’est pas un `readonly` afin que les clés générées par le magasin peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="45c83-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="45c83-135">Les autres propriétés sont en lecture seule défini uniquement dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="45c83-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="45c83-136">Si la valeur de clé primaire est toujours définie par EF ou lire à partir de la base de données, puis il n’est pas nécessaire d’inclure dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="45c83-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="45c83-137">Cela laisse la clé « property » comme un champ de type simple et indique clairement qu’il ne doit pas être défini explicitement lors de la création de nouveaux blogs ou des publications.</span><span class="sxs-lookup"><span data-stu-id="45c83-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="45c83-138">Ce code génère avertissement '169' indiquant que le champ n’est jamais utilisé.</span><span class="sxs-lookup"><span data-stu-id="45c83-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="45c83-139">Cela peut être ignoré, car il est en réalité EF Core utilise le champ de manière extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="45c83-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="45c83-140">Injection des services</span><span class="sxs-lookup"><span data-stu-id="45c83-140">Injecting services</span></span>

<span data-ttu-id="45c83-141">EF Core pouvez injecter également un « services » dans le constructeur du type d’une entité.</span><span class="sxs-lookup"><span data-stu-id="45c83-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="45c83-142">Par exemple, les éléments suivants peuvent être ajoutés :</span><span class="sxs-lookup"><span data-stu-id="45c83-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="45c83-143">`DbContext` -l’instance de contexte actuel, qui peut également être de type votre type dérivé de DbContext</span><span class="sxs-lookup"><span data-stu-id="45c83-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="45c83-144">`ILazyLoader` -le service de chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations</span><span class="sxs-lookup"><span data-stu-id="45c83-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="45c83-145">`Action<object, string>` -un délégué de chargement différé, consultez le [chargement différé documentation](../querying/related-data.md) pour plus d’informations</span><span class="sxs-lookup"><span data-stu-id="45c83-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="45c83-146">`IEntityType` -les métadonnées de EF principales associées à ce type d’entité</span><span class="sxs-lookup"><span data-stu-id="45c83-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="45c83-147">À compter de EF Core 2.1, uniquement les services connus EF Core peuvent être ajoutées.</span><span class="sxs-lookup"><span data-stu-id="45c83-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="45c83-148">Prise en charge pour l’injection des services d’application est envisagée pour une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="45c83-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="45c83-149">Par exemple, un DbContext injecté peut servir à sélectivement accéder à la base de données pour obtenir des informations sur les entités associées sans charger toutes les.</span><span class="sxs-lookup"><span data-stu-id="45c83-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="45c83-150">Dans l’exemple ci-dessous, cela est utilisé pour obtenir le nombre de publications de blog de sans charger les billets de :</span><span class="sxs-lookup"><span data-stu-id="45c83-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="45c83-151">Choses à noter à ce sujet :</span><span class="sxs-lookup"><span data-stu-id="45c83-151">A few things to notice about this:</span></span>
* <span data-ttu-id="45c83-152">Le constructeur est privé, car il est toujours appelée par EF Core, et il existe un autre constructeur public pour une utilisation générale.</span><span class="sxs-lookup"><span data-stu-id="45c83-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="45c83-153">Le code qui utilise le service injecté (autrement dit, le contexte) est défense contre lui étant `null` pour gérer les cas où EF Core n’est pas création de l’instance.</span><span class="sxs-lookup"><span data-stu-id="45c83-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="45c83-154">Étant donné que le service est stocké dans une propriété en lecture/écriture, elle sera réinitialisée lorsque l’entité est attachée à une nouvelle instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="45c83-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="45c83-155">Injection le DbContext comme cela est souvent considéré un anti-modèle, car il associe vos types d’entité directement à EF Core.</span><span class="sxs-lookup"><span data-stu-id="45c83-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="45c83-156">Étudiez attentivement toutes les options avant d’utiliser l’injection des services comme suit.</span><span class="sxs-lookup"><span data-stu-id="45c83-156">Carefully consider all options before using service injection like this.</span></span>
