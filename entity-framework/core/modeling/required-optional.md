---
title: Propriétés obligatoires ou facultatives - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995495"
---
# <a name="required-and-optional-properties"></a>Propriétés obligatoires et facultatifs

Une propriété est considéré comme facultative s’il est valide pour pouvoir contenir `null`. Si `null` n’est pas une valeur valide pour être affectée à une propriété, puis il est considéré comme une propriété obligatoire.

## <a name="conventions"></a>Conventions

Par convention, une propriété dont le type CLR peut contenir la valeur null sera configurée comme étant facultatif (`string`, `int?`, `byte[]`, etc..). Les propriétés dont le type CLR ne peut pas contenir de valeur null seront configurées en fonction des besoins (`int`, `decimal`, `bool`, etc..).

> [!NOTE]  
> Une propriété dont le type CLR ne peut pas contenir de valeur null ne peut pas être configurée comme facultatifs. La propriété sera toujours considéré comme exigé par Entity Framework.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour indiquer qu’une propriété est obligatoire.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour indiquer qu’une propriété est obligatoire.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
