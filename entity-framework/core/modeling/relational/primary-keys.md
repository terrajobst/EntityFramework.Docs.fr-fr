---
title: Clés primaires - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052719"
---
# <a name="primary-keys"></a><span data-ttu-id="0aea6-102">Clés primaires</span><span class="sxs-lookup"><span data-stu-id="0aea6-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="0aea6-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="0aea6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0aea6-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="0aea6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0aea6-105">Une contrainte de clé primaire est introduite pour la clé de chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="0aea6-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="0aea6-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="0aea6-106">Conventions</span></span>

<span data-ttu-id="0aea6-107">Par convention, la clé primaire dans la base de données est nommée `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="0aea6-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0aea6-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="0aea6-108">Data Annotations</span></span>

<span data-ttu-id="0aea6-109">Aucun des aspects spécifiques de la base de données relationnelles d’une clé primaire ne peuvent être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="0aea6-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0aea6-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="0aea6-110">Fluent API</span></span>

<span data-ttu-id="0aea6-111">Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé primaire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0aea6-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
