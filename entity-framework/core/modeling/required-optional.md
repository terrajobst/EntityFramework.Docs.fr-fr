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
# <a name="required-and-optional-properties"></a><span data-ttu-id="35b4a-102">Propriétés obligatoires et facultatifs</span><span class="sxs-lookup"><span data-stu-id="35b4a-102">Required and Optional Properties</span></span>

<span data-ttu-id="35b4a-103">Une propriété est considéré comme facultative s’il est valide pour pouvoir contenir `null`.</span><span class="sxs-lookup"><span data-stu-id="35b4a-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="35b4a-104">Si `null` n’est pas une valeur valide pour être affectée à une propriété, puis il est considéré comme une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="35b4a-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="35b4a-105">Conventions</span><span class="sxs-lookup"><span data-stu-id="35b4a-105">Conventions</span></span>

<span data-ttu-id="35b4a-106">Par convention, une propriété dont le type CLR peut contenir la valeur null sera configurée comme étant facultatif (`string`, `int?`, `byte[]`, etc..).</span><span class="sxs-lookup"><span data-stu-id="35b4a-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="35b4a-107">Les propriétés dont le type CLR ne peut pas contenir de valeur null seront configurées en fonction des besoins (`int`, `decimal`, `bool`, etc..).</span><span class="sxs-lookup"><span data-stu-id="35b4a-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="35b4a-108">Une propriété dont le type CLR ne peut pas contenir de valeur null ne peut pas être configurée comme facultatifs.</span><span class="sxs-lookup"><span data-stu-id="35b4a-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="35b4a-109">La propriété sera toujours considéré comme exigé par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="35b4a-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="35b4a-110">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="35b4a-110">Data Annotations</span></span>

<span data-ttu-id="35b4a-111">Vous pouvez utiliser des Annotations de données pour indiquer qu’une propriété est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="35b4a-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="35b4a-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="35b4a-112">Fluent API</span></span>

<span data-ttu-id="35b4a-113">Vous pouvez utiliser l’API Fluent pour indiquer qu’une propriété est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="35b4a-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
