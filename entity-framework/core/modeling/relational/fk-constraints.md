---
title: Contraintes de clé étrangère - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993986"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="39d6e-102">Contraintes de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="39d6e-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="39d6e-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="39d6e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="39d6e-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="39d6e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="39d6e-105">Une contrainte de clé étrangère est introduite pour chaque relation dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="39d6e-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="39d6e-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="39d6e-106">Conventions</span></span>

<span data-ttu-id="39d6e-107">Par convention, les contraintes de clé étrangère sont nommés `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="39d6e-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="39d6e-108">Pour les clés étrangères composites `<foreign key property name>` devient une liste séparée par des traits de soulignement des noms de propriété de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="39d6e-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="39d6e-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="39d6e-109">Data Annotations</span></span>

<span data-ttu-id="39d6e-110">Les noms de contrainte de clé étrangère ne peut pas être configurés à l’aide des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="39d6e-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="39d6e-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="39d6e-111">Fluent API</span></span>

<span data-ttu-id="39d6e-112">Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé étrangère d’une relation.</span><span class="sxs-lookup"><span data-stu-id="39d6e-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
