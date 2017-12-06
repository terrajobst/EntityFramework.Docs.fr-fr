---
title: "Propriétés de l’ombre - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a>Propriétés de l’ombre

Propriétés de clichés instantanés sont ne sont pas définis dans votre classe d’entité .NET mais qui sont définies pour ce type d’entité dans le modèle EF Core. La valeur et l’état de ces propriétés est conservée dans le traceur de modifications.

Propriétés de l’ombre sont utiles lorsqu’il existe des données dans la base de données qui ne doit pas être exposée sur les types d’entité mappée. Elles sont souvent utilisées pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entités à l’aide des propriétés de navigation entre les types d’entités.

Les valeurs de propriété de clichés instantanés peuvent être obtenues et modifiés par le biais du `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Propriétés de clichés instantanés peuvent être référencées dans les requêtes LINQ via la `EF.Property` méthode statique.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Conventions

Occulter les propriétés peuvent être créées par convention, lorsqu’une relation est découvert, mais aucune propriété de clé étrangère ne se trouve dans la classe d’entité dépendante. Dans ce cas, une propriété de clé étrangère de clichés instantanés est introduite. La propriété de clé étrangère de clichés instantanés est nommée `<navigation property name><principal key property name>` (le volet de navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisé pour l’attribution de noms). Si le nom de la propriété de clé principale inclut le nom de la propriété de navigation, puis le nom sera simplement `<principal key property name>`. S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom de type principal est utilisé à la place.

Par exemple, le code suivant entraîne une `BlogId` est présentée à la propriété shadow le `Post` entité.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

## <a name="data-annotations"></a>Annotations de données

Occulter les propriétés ne peuvent pas être créées avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer les propriétés de l’ombre. Une fois que vous avez appelé la surcharge de chaîne de `Property` vous pouvez chaîner des appels de configuration vous le feriez pour d’autres propriétés.

Si le nom fourni pour le `Property` méthode correspond au nom d’une propriété existante (une propriété de clichés instantanés ou défini sur la classe d’entité), puis le code configure cette propriété existante au lieu d’introduire une nouvelle propriété de clichés instantanés.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
