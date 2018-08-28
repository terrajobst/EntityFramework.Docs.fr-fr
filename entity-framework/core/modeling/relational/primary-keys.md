---
title: Clés primaires - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998066"
---
# <a name="primary-keys"></a><span data-ttu-id="171e0-102">Clés primaires</span><span class="sxs-lookup"><span data-stu-id="171e0-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="171e0-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="171e0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="171e0-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="171e0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="171e0-105">Une contrainte de clé primaire est introduite pour la clé de chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="171e0-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="171e0-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="171e0-106">Conventions</span></span>

<span data-ttu-id="171e0-107">Par convention, la clé primaire dans la base de données est nommée `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="171e0-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="171e0-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="171e0-108">Data Annotations</span></span>

<span data-ttu-id="171e0-109">Aucune base de données relationnelle des aspects spécifiques de clé primaire ne peuvent être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="171e0-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="171e0-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="171e0-110">Fluent API</span></span>

<span data-ttu-id="171e0-111">Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé primaire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="171e0-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
