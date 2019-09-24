---
title: Héritage (base de données relationnelle)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7fb19f9c86d1768967d172c006eb5d894254e0c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196931"
---
# <a name="inheritance-relational-database"></a>Héritage (base de données relationnelle)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.

> [!NOTE]  
> Actuellement, seul le modèle TPH (table par hiérarchie) est implémenté dans EF Core. D’autres modèles courants tels que table par type (TPT) et table par type (TPC) ne sont pas encore disponibles.

## <a name="conventions"></a>Conventions

Par Convention, l’héritage est mappé à l’aide du modèle TPH (table par hiérarchie). TPH utilise une table unique pour stocker les données de tous les types dans la hiérarchie. Une colonne de discriminateur est utilisée pour identifier le type représenté par chaque ligne.

EF Core configure l’héritage uniquement si au moins deux types hérités sont explicitement inclus dans le modèle (pour plus d’informations, consultez [héritage](../inheritance.md) ).

Voici un exemple qui illustre un scénario d’héritage simple et les données stockées dans une table de base de données relationnelle à l’aide du modèle TPH. La colonne de *discriminateur* identifie le type de *blog* qui est stocké dans chaque ligne.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Les colonnes de base de données deviennent automatiquement Nullable si nécessaire lors de l’utilisation du mappage TPH.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs utilisées pour identifier chaque type dans la hiérarchie.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Configuration de la propriété de discriminateur

Dans les exemples ci-dessus, le discriminateur est créé en tant que [propriété Shadow](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie. Étant donné qu’il s’agit d’une propriété dans le modèle, elle peut être configurée comme d’autres propriétés. Par exemple, pour définir la longueur maximale lorsque le discriminateur par défaut par convention est utilisé :

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Le discriminateur peut également être mappé à une propriété CLR réelle dans votre entité. Par exemple :
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Pour combiner ces deux éléments, il est possible de mapper le discriminateur à une véritable propriété et de le configurer :
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
