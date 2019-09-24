---
title: Relations-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197652"
---
# <a name="relationships"></a>Relations

Une relation définit la relation entre deux entités. Dans une base de données relationnelle, il est représenté par une contrainte de clé étrangère.

> [!NOTE]  
> La plupart des exemples de cet article utilisent une relation un-à-plusieurs pour illustrer les concepts. Pour obtenir des exemples de relations un-à-un et plusieurs-à-plusieurs, consultez la section [autres modèles de relation](#other-relationship-patterns) à la fin de l’article.

## <a name="definition-of-terms"></a>Définition des termes

Il existe un certain nombre de termes utilisés pour décrire les relations

* **Entité dépendante :** Il s’agit de l’entité qui contient la ou les propriétés de clé étrangère. Parfois appelé « enfant » de la relation.

* **Entité principale :** Il s’agit de l’entité qui contient la ou les propriétés de clé primaire/secondaire. Parfois appelé « parent » de la relation.

* **Clé étrangère :** Propriété (s) de l’entité dépendante utilisée pour stocker les valeurs de la propriété de clé principale à laquelle l’entité est associée.

* **Clé principale :** La ou les propriétés qui identifient de façon unique l’entité principale. Il peut s’agir de la clé primaire ou d’une clé secondaire.

* **Propriété de navigation :** Propriété définie sur le principal et/ou l’entité dépendante qui contient une ou plusieurs références aux entités associées.

  * **Propriété de navigation de la collection :** Propriété de navigation qui contient des références à de nombreuses entités associées.

  * **Propriété de navigation de référence :** Propriété de navigation qui contient une référence à une entité associée unique.

  * **Propriété de navigation inverse :** Lorsque vous discutez d’une propriété de navigation particulière, ce terme fait référence à la propriété de navigation à l’autre extrémité de la relation.

La liste de code suivante montre une relation un-à-plusieurs `Blog` entre et`Post`

* `Post`est l’entité dépendante

* `Blog`est l’entité principale

* `Post.BlogId`est la clé étrangère

* `Blog.BlogId`est la clé principale (dans le cas présent, il s’agit d’une clé primaire plutôt que d’une clé secondaire)

* `Post.Blog`est une propriété de navigation de référence

* `Blog.Posts`est une propriété de navigation de collection

* `Post.Blog`est la propriété de navigation inverse `Blog.Posts` de (et vice versa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Conventions

Par Convention, une relation est créée lorsqu’une propriété de navigation est détectée sur un type. Une propriété est considérée comme une propriété de navigation si le type vers lequel elle pointe ne peut pas être mappé en tant que type scalaire par le fournisseur de base de données actuel.

> [!NOTE]  
> Les relations découvertes par convention cibleront toujours la clé primaire de l’entité principale. Pour cibler une clé secondaire, une configuration supplémentaire doit être effectuée à l’aide de l’API Fluent.

### <a name="fully-defined-relationships"></a>Relations entièrement définies

Le modèle le plus courant pour les relations est d’avoir des propriétés de navigation définies aux deux extrémités de la relation et une propriété de clé étrangère définie dans la classe d’entité dépendante.

* Si une paire de propriétés de navigation est trouvée entre deux types, ils sont configurés en tant que propriétés de navigation inverse de la même relation.

* Si l’entité dépendante contient une `<primary key property name>`propriété `<navigation property name><primary key property name>`nommée, `<principal entity name><primary key property name>` , ou si elle est configurée en tant que clé étrangère.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> S’il existe plusieurs propriétés de navigation définies entre deux types (autrement dit, plusieurs paires distinctes de navigation qui pointent les unes vers les autres), aucune relation ne sera créée par Convention et vous devrez les configurer manuellement pour identifier la façon dont le les propriétés de navigation sont couplées.

### <a name="no-foreign-key-property"></a>Aucune propriété de clé étrangère

Bien qu’il soit recommandé d’avoir une propriété de clé étrangère définie dans la classe d’entité dépendante, elle n’est pas obligatoire. Si aucune propriété de clé étrangère n’est trouvée, une propriété de clé étrangère Shadow est introduite `<navigation property name><principal key property name>` avec le nom (pour plus d’informations, consultez [Propriétés Shadow](shadow-properties.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propriété de navigation unique

L’inclusion d’une seule propriété de navigation (aucune navigation inverse et aucune propriété de clé étrangère) suffit à avoir une relation définie par Convention. Vous pouvez également avoir une propriété de navigation unique et une propriété de clé étrangère.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Suppression en cascade

Par Convention, la suppression en cascade sera définie sur *cascade* pour les relations requises et *ClientSetNull* pour les relations facultatives. *Cascade* signifie que les entités dépendantes sont également supprimées. *ClientSetNull* signifie que les entités dépendantes qui ne sont pas chargées en mémoire restent inchangées et doivent être supprimées manuellement ou mises à jour pour pointer vers une entité principale valide. Pour les entités chargées en mémoire, EF Core tente de définir les propriétés de clé étrangère sur la valeur null.

Consultez la section [relations obligatoires et facultatives](#required-and-optional-relationships) pour connaître la différence entre les relations obligatoires et facultatives.

Pour plus d’informations sur les différents comportements de suppression et les valeurs par défaut utilisées par Convention, consultez [suppression en cascade](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Annotations de données

Il existe deux annotations de données qui peuvent être utilisées pour configurer `[ForeignKey]` des `[InverseProperty]`relations, et. Celles-ci sont disponibles `System.ComponentModel.DataAnnotations.Schema` dans l’espace de noms.

### <a name="foreignkey"></a>ForeignKey

Vous pouvez utiliser les annotations de données pour configurer la propriété à utiliser comme propriété de clé étrangère pour une relation donnée. Cela s’effectue généralement lorsque la propriété de clé étrangère n’est pas détectée par Convention.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> L' `[ForeignKey]` annotation peut être placée sur l’une des propriétés de navigation de la relation. Elle n’a pas besoin de se trouver dans la propriété de navigation de la classe d’entité dépendante.

### <a name="inverseproperty"></a>[InverseProperty]

Vous pouvez utiliser les annotations de données pour configurer la combinaison des propriétés de navigation sur les entités dépendantes et principales. Cela s’effectue généralement quand il existe plusieurs paires de propriétés de navigation entre deux types d’entités.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>API Fluent

Pour configurer une relation dans l’API Fluent, commencez par identifier les propriétés de navigation qui composent la relation. `HasOne`ou `HasMany` identifie la propriété de navigation sur le type d’entité sur lequel vous commencez la configuration. Vous enchaînez ensuite un `WithOne` appel `WithMany` à ou pour identifier la navigation inverse. `HasOne`/`WithOne`sont utilisés pour les propriétés de navigation `HasMany` de référence et / `WithMany` sont utilisés pour les propriétés de navigation de collection.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Propriété de navigation unique

Si vous n’avez qu’une seule propriété de navigation, il y a des surcharges sans paramètre de `WithOne` et. `WithMany` Cela indique qu’il existe conceptuellement une référence ou une collection à l’autre extrémité de la relation, mais qu’il n’y a aucune propriété de navigation incluse dans la classe d’entité.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Clé étrangère

Vous pouvez utiliser l’API Fluent pour configurer la propriété à utiliser comme propriété de clé étrangère pour une relation donnée.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

La liste de code suivante montre comment configurer une clé étrangère composite.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Vous pouvez utiliser la surcharge de chaîne `HasForeignKey(...)` de pour configurer une propriété Shadow comme clé étrangère (pour plus d’informations, consultez [Propriétés Shadow](shadow-properties.md) ). Nous vous recommandons d’ajouter explicitement la propriété Shadow au modèle avant de l’utiliser comme clé étrangère (comme indiqué ci-dessous).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Sans propriété de navigation

Vous n’avez pas nécessairement besoin de fournir une propriété de navigation. Vous pouvez simplement fournir une clé étrangère d’un côté de la relation.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Clé principale

Si vous souhaitez que la clé étrangère fasse référence à une propriété autre que la clé primaire, vous pouvez utiliser l’API Fluent pour configurer la propriété de clé principale de la relation. La propriété que vous configurez comme clé principale est automatiquement configurée en tant que clé secondaire (pour plus d’informations, consultez [clés alternatives](alternate-keys.md) ).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

L’exemple de code suivant montre comment configurer une clé principale composite.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> L’ordre dans lequel vous spécifiez les propriétés de clé principale doit correspondre à l’ordre dans lequel elles sont spécifiées pour la clé étrangère.

### <a name="required-and-optional-relationships"></a>Relations obligatoires et facultatives

Vous pouvez utiliser l’API Fluent pour déterminer si la relation est obligatoire ou facultative. Au final, cela contrôle si la propriété de clé étrangère est obligatoire ou facultative. Cela est particulièrement utile lorsque vous utilisez une clé étrangère d’État Shadow. Si vous avez une propriété de clé étrangère dans votre classe d’entité, le caractère requis de la relation est déterminé selon que la propriété de clé étrangère est obligatoire ou facultative (pour plus d’informations, consultez [propriétés requises et facultatives](required-optional.md) ).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

Vous pouvez utiliser l’API Fluent pour configurer explicitement le comportement de suppression en cascade pour une relation donnée.

Pour une présentation détaillée de chaque option, consultez [suppression en cascade](../saving/cascade-delete.md) dans la section enregistrement des données.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

### <a name="one-to-one"></a>Un-à-un

Les relations un-à-un ont une propriété de navigation de référence des deux côtés. Ils suivent les mêmes conventions que les relations un-à-plusieurs, mais un index unique est introduit sur la propriété de clé étrangère pour s’assurer qu’un seul dépendant est lié à chaque principal.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> EF choisit l’une des entités à dépendre en fonction de sa capacité à détecter une propriété de clé étrangère. Si l’entité incorrecte est choisie comme dépendant, vous pouvez utiliser l’API Fluent pour corriger ce problème.

Quand vous configurez la relation avec l’API Fluent, vous `HasOne` utilisez `WithOne` les méthodes et.

Quand vous configurez la clé étrangère, vous devez spécifier le type d’entité dépendant. Notez le paramètre `HasForeignKey` générique fourni à dans la liste ci-dessous. Dans une relation un-à-plusieurs, il est clair que l’entité avec la navigation de référence est la dépendance et celle avec la collection est le principal. Mais ce n’est pas le cas dans une relation un-à-un, c’est pourquoi il est nécessaire de la définir explicitement.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

Les relations plusieurs-à-plusieurs sans classe d’entité pour représenter la table de jointure ne sont pas encore prises en charge. Toutefois, vous pouvez représenter une relation plusieurs-à-plusieurs en incluant une classe d’entité pour la table de jointure et en mappant deux relations un-à-plusieurs distinctes.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
