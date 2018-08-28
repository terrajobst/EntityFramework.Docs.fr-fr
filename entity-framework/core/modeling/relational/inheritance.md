---
title: Héritage (base de données relationnelle) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996320"
---
# <a name="inheritance-relational-database"></a>Héritage (base de données relationnelle)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.

> [!NOTE]  
> Actuellement, uniquement au modèle de table par hiérarchie (TPH) est implémenté dans EF Core. Autres modèles courants comme table par type (TPT) et table-par-type concret (TPC) ne sont pas encore disponibles.

## <a name="conventions"></a>Conventions

Par convention, l’héritage sera mappé à l’aide du modèle de table par hiérarchie (TPH). TPH utilise une seule table pour stocker les données pour tous les types dans la hiérarchie. Une colonne de discriminateur est utilisée pour identifier le type de chaque ligne représente.

EF Core sera uniquement installer héritage si deux ou plusieurs des types hérités sont explicitement inclus dans le modèle (voir [héritage](../inheritance.md) pour plus d’informations).

Voici un exemple montrant un scénario d’héritage simple et les données stockées dans une table de base de données relationnelle en utilisant le modèle TPH. Le *discriminateur* colonne identifie le type de *Blog* sont stockées dans chaque ligne.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
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

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs qui sont utilisées pour identifier chaque type dans la hiérarchie.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

Dans les exemples ci-dessus, le discriminateur est créé comme un [occulter la propriété](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie. Dans la mesure où il s’agit d’une propriété dans le modèle, il peut être configuré comme d’autres propriétés. Par exemple, pour définir la longueur maximale lorsque la valeur par défaut, les discriminateur par convention est utilisée :

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Le discriminateur peut également être mappé à une propriété CLR réelle de votre entité. Exemple :
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

Combinant ces deux opérations, il est possible de mapper le discriminateur pour une véritable propriété et le configurer :
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
