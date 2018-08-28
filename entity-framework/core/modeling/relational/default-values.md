---
title: Valeurs par défaut - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996250"
---
# <a name="default-values"></a><span data-ttu-id="dff9a-102">Valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="dff9a-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="dff9a-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="dff9a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="dff9a-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="dff9a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="dff9a-105">La valeur par défaut d’une colonne est la valeur qui sera insérée si une nouvelle ligne est insérée, mais aucune valeur n’est spécifiée pour la colonne.</span><span class="sxs-lookup"><span data-stu-id="dff9a-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="dff9a-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="dff9a-106">Conventions</span></span>

<span data-ttu-id="dff9a-107">Par convention, une valeur par défaut n’est pas configurée.</span><span class="sxs-lookup"><span data-stu-id="dff9a-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="dff9a-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="dff9a-108">Data Annotations</span></span>

<span data-ttu-id="dff9a-109">Vous ne pouvez pas définir une valeur par défaut à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="dff9a-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dff9a-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="dff9a-110">Fluent API</span></span>

<span data-ttu-id="dff9a-111">Vous pouvez utiliser l’API Fluent pour spécifier la valeur par défaut pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="dff9a-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="dff9a-112">Vous pouvez également spécifier un fragment SQL qui est utilisé pour calculer la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="dff9a-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
