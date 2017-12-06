---
title: Relations - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a>Relationships

Une relation définit comment deux entités sont liés entre eux. Dans une base de données relationnelle, il est représenté par une contrainte de clé étrangère.

> [!NOTE]  
> La plupart des exemples dans cet article utilisent une relation un-à-plusieurs pour illustrer les concepts. Pour obtenir des exemples de relations un à un et plusieurs-à-plusieurs, consultez le [autres modèles de relation](#other-relationship-patterns) section à la fin de l’article.

## <a name="definition-of-terms"></a>Définition des termes

Il existe un nombre de termes utilisés pour décrire des relations

* **Entité dépendante :** cela est l’entité qui contient les propriétés de clés étrangères. Parfois comme enfant de la relation.

* **Entité principale :** cela est l’entité qui contient les propriétés de clés primaire/remplacement. Parfois appelée « parent » de la relation.

* **Clé étrangère :** les propriétés de l’entité dépendante qui est utilisée pour stocker les valeurs de la propriété de clé principale associée à l’entité.

* **Clé principale :** la propriété qui identifie de façon unique l’entité principale. Cela peut être la clé primaire ou une autre clé.

* **Propriété de navigation :** une propriété définie sur l’entité principale et/ou dépendante qui contient une ou plusieurs références aux ou les entités connexes.

  * **Propriété de navigation de collection :** une propriété de navigation qui contient des références à nombreuses entités connexes.

  * **Référence de propriété de navigation :** une propriété de navigation qui conserve une référence à une entité connexe unique.

  * **Propriété de navigation inversée :** lorsque vous présentez une propriété de navigation particulier, ce terme fait référence à la propriété de navigation à l’autre extrémité de la relation.

Le code suivant montre une relation un-à-plusieurs entre `Blog` et`Post`

* `Post`est de l’entité dépendante

* `Blog`est de l’entité principale

* `Post.BlogId`est la clé étrangère

* `Blog.BlogId`est la clé principale (dans ce cas il est une clé primaire plutôt que d’une autre clé)

* `Post.Blog`est une propriété de navigation de référence

* `Blog.Posts`est une propriété de navigation de collection

* `Post.Blog`la propriété de navigation inversée de `Blog.Posts` (et vice versa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Conventions

Par convention, une relation sera créée lors d’une propriété de navigation détectée sur un type. Une propriété est considérée comme une propriété de navigation si le type vers lequel elle pointe ne peut pas être mappé en tant que type scalaire par le fournisseur actuel de la base de données.

> [!NOTE]  
> Les relations qui sont découverts par convention ciblera toujours la clé primaire de l’entité principale. Pour cibler une autre clé, une configuration supplémentaire doit être effectuée à l’aide de l’API Fluent.

### <a name="fully-defined-relationships"></a>Relations entièrement définies

Le modèle le plus courant pour les relations est d’avoir des propriétés de navigation définies sur les deux extrémités de la relation et une propriété de clé étrangère définie dans la classe d’entité dépendante.

* Si une paire de propriétés de navigation se trouve entre deux types, ils seront configurés en tant que propriétés de navigation inverse de la même relation.

* Si l’entité dépendante contient une propriété nommée `<primary key property name>`, `<navigation property name><primary key property name>`, ou `<principal entity name><primary key property name>` , puis il sera configuré en tant que la clé étrangère.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> S’il existe plusieurs propriétés de navigation définies entre deux types (autrement dit, plusieurs paires distinctes de navigations qui pointent vers l’autre), alors aucune relation ne sera créée par convention, et vous devez les configurer manuellement pour identifier la propriétés de navigation par deux.

### <a name="no-foreign-key-property"></a>Aucune propriété de clé étrangère

S’il est recommandé d’avoir une propriété de clé étrangère définie dans la classe d’entité dépendante, il n’est pas nécessaire. Si aucune propriété de clé étrangère n’est trouvée, une propriété de clé étrangère de clichés instantanés est introduite avec le nom `<navigation property name><principal key property name>` (consultez [occulter les propriétés](shadow-properties.md) pour plus d’informations).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propriété de Navigation

Notamment qu’une propriété de navigation (aucune navigation inverse et aucune propriété de clé étrangère) est suffisante pour permettre une relation définie par convention. Vous pouvez également avoir une propriété de navigation et une propriété de clé étrangère.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Suppression en cascade

Par convention, la suppression en cascade est définie sur *Cascade* pour les relations requises et *ClientSetNull* pour les relations facultatif. *Cascade* signifie entités dépendantes sont également supprimées. *ClientSetNull* signifie que les entités dépendantes ne sont pas chargées en mémoire reste inchangée et doit être supprimés manuellement ou mis à jour pour pointer vers une entité principale valide. Pour les entités qui sont chargées en mémoire, EF Core tente de définir les propriétés de clé étrangères sur null.

Consultez le [relations obligatoires et facultatifs](#required-and-optional-relationships) section de la différence entre les relations requises et facultatives.

Consultez [suppression en Cascade](../saving/cascade-delete.md) pour plus d’informations sur les différents comportements et les valeurs par défaut utilisés par la convention de suppression.

## <a name="data-annotations"></a>Annotations de données

Il existe deux des annotations de données qui peuvent être utilisées pour configurer des relations, `[ForeignKey]` et `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Vous pouvez utiliser les Annotations de données pour configurer la propriété doit être utilisée comme propriété de clé étrangère pour une relation donnée. Cela est généralement le cas lorsque la propriété de clé étrangère n’est pas découvert par convention.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> Le `[ForeignKey]` annotation peut être placée sur une propriété de navigation dans la relation. Il n’a pas besoin de passer la propriété de navigation dans la classe d’entité dépendante.

### <a name="inverseproperty"></a>[InverseProperty]

Vous pouvez utiliser les Annotations de données pour configurer la façon dont les propriétés de navigation sur les entités principales et dépendantes par deux. Cela est généralement le cas lorsqu’il y a plus d’une paire de propriétés de navigation entre les deux types d’entité.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>API Fluent

Pour configurer une relation dans l’API Fluent, commencez par identifier les propriétés de navigation qui composent la relation. `HasOne`ou `HasMany` identifie la propriété de navigation sur le type d’entité vous sont à partir de la configuration. Vous liez ensuite un appel à `WithOne` ou `WithMany` pour identifier le volet de navigation inverse. `HasOne`/`WithOne`sont utilisés pour les propriétés de navigation de référence et `HasMany` / `WithMany` sont utilisés pour les propriétés de navigation de collection.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Propriété de Navigation

Si vous avez uniquement une propriété de navigation, les surcharges sans paramètre de `WithOne` et `WithMany`. Cela indique qu’il le concept est une référence ou une collection à l’autre extrémité de la relation, mais il n’existe aucune propriété de navigation incluse dans la classe d’entité.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Clé étrangère

Vous pouvez utiliser l’API Fluent pour configurer la propriété doit être utilisée comme propriété de clé étrangère pour une relation donnée.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Le code suivant montre comment configurer une clé étrangère composite.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Vous pouvez utiliser la surcharge de chaîne de `HasForeignKey(...)` pour configurer une propriété de clichés instantanés comme une clé étrangère (voir [occulter les propriétés](shadow-properties.md) pour plus d’informations). Nous vous recommandons d’ajouter explicitement la propriété de clichés instantanés pour le modèle avant de l’utiliser comme une clé étrangère (comme indiqué ci-dessous).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Clé d’entité de sécurité

Si vous souhaitez que la clé étrangère à une propriété autre que la clé primaire, vous pouvez utiliser l’API Fluent pour configurer la propriété de clé principale de la relation. La propriété que vous configurez en tant que le principal est clé automatiquement être configuré en tant qu’une autre clé (voir [clés secondaires](alternate-keys.md) pour plus d’informations).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Le code suivant montre comment configurer une clé principale.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> L’ordre dans lequel vous spécifiez les propriétés de clé principales doit correspondre à l’ordre dans lequel elles sont spécifiées pour la clé étrangère.

### <a name="required-and-optional-relationships"></a>Relations obligatoires et facultatifs

Vous pouvez utiliser l’API Fluent pour déterminer si la relation est requise ou facultative. Il décide si la propriété de clé étrangère est obligatoire ou facultatif. Cela est particulièrement utile lorsque vous utilisez une clé étrangère état de clichés instantanés. Si vous disposez d’une propriété de clé étrangère dans votre classe d’entité, puis le requiredness de la relation est déterminée selon que la propriété de clé étrangère est obligatoire ou facultatif (voir [propriétés obligatoires et facultatifs](required-optional.md) pour plus d’informations plus d’informations).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>Suppression en cascade

Vous pouvez utiliser l’API Fluent pour configurer le comportement de suppression en cascade pour une relation donnée explicitement.

Consultez [suppression en Cascade](../saving/cascade-delete.md) dans la section de l’enregistrement des données pour une présentation détaillée de chaque option.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Autres modèles de relation

### <a name="one-to-one"></a>Un à un

Les relations un à un ont une propriété de navigation de référence sur les deux côtés. Elles suivent les mêmes conventions que les relations un-à-plusieurs, mais un index unique est présentée sur la propriété de clé étrangère pour vous assurer qu’un seul dépendant est lié à chaque principal.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF choisira l’une des entités pour être dépendantes en fonction de sa capacité à détecter une propriété de clé étrangère. Si l’entité incorrecte est choisie comme dépendantes, vous pouvez utiliser l’API Fluent pour corriger ce problème.

Lors de la configuration de la relation avec l’API Fluent, vous utilisez la `HasOne` et `WithOne` méthodes.

Lors de la configuration de la clé étrangère, vous devez spécifier le type d’entité dépendante - Notez le paramètre générique fourni pour `HasForeignKey` dans la liste ci-dessous. Dans une relation un-à-plusieurs, il est clair que l’entité avec le volet de navigation référence dépend et celle de la collection est le principal. Mais ce n’est pas dans une relation un à un - par conséquent, la nécessité pour le définir explicitement.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Plusieurs-à-plusieurs

Relations plusieurs-à-plusieurs sans une classe d’entité pour représenter la table de jointure ne sont pas encore pris en charge. Toutefois, vous pouvez représenter une relation plusieurs-à-plusieurs en incluant une classe d’entité pour la table de jointure et de deux relations un-à-plusieurs distinct mappage.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
