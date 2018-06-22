---
title: Propriétés de l’ombre - EF Core
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
ms.locfileid: "26053549"
---
# <a name="shadow-properties"></a><span data-ttu-id="a3138-102">Propriétés de l’ombre</span><span class="sxs-lookup"><span data-stu-id="a3138-102">Shadow Properties</span></span>

<span data-ttu-id="a3138-103">Propriétés de clichés instantanés sont ne sont pas définis dans votre classe d’entité .NET mais qui sont définies pour ce type d’entité dans le modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="a3138-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="a3138-104">La valeur et l’état de ces propriétés est conservée dans le traceur de modifications.</span><span class="sxs-lookup"><span data-stu-id="a3138-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="a3138-105">Propriétés de l’ombre sont utiles lorsqu’il existe des données dans la base de données qui ne doit pas être exposée sur les types d’entité mappée.</span><span class="sxs-lookup"><span data-stu-id="a3138-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="a3138-106">Elles sont souvent utilisées pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entités à l’aide des propriétés de navigation entre les types d’entités.</span><span class="sxs-lookup"><span data-stu-id="a3138-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="a3138-107">Les valeurs de propriété de clichés instantanés peuvent être obtenues et modifiés par le biais du `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="a3138-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="a3138-108">Propriétés de clichés instantanés peuvent être référencées dans les requêtes LINQ via la `EF.Property` méthode statique.</span><span class="sxs-lookup"><span data-stu-id="a3138-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="a3138-109">Conventions</span><span class="sxs-lookup"><span data-stu-id="a3138-109">Conventions</span></span>

<span data-ttu-id="a3138-110">Occulter les propriétés peuvent être créées par convention, lorsqu’une relation est découvert, mais aucune propriété de clé étrangère ne se trouve dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="a3138-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="a3138-111">Dans ce cas, une propriété de clé étrangère de clichés instantanés est introduite.</span><span class="sxs-lookup"><span data-stu-id="a3138-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="a3138-112">La propriété de clé étrangère de clichés instantanés est nommée `<navigation property name><principal key property name>` (le volet de navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisé pour l’attribution de noms).</span><span class="sxs-lookup"><span data-stu-id="a3138-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="a3138-113">Si le nom de la propriété de clé principale inclut le nom de la propriété de navigation, puis le nom sera simplement `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a3138-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="a3138-114">S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom de type principal est utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="a3138-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="a3138-115">Par exemple, le code suivant entraîne une `BlogId` est présentée à la propriété shadow le `Post` entité.</span><span class="sxs-lookup"><span data-stu-id="a3138-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a3138-116">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a3138-116">Data Annotations</span></span>

<span data-ttu-id="a3138-117">Occulter les propriétés ne peuvent pas être créées avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="a3138-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a3138-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a3138-118">Fluent API</span></span>

<span data-ttu-id="a3138-119">Vous pouvez utiliser l’API Fluent pour configurer les propriétés de l’ombre.</span><span class="sxs-lookup"><span data-stu-id="a3138-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="a3138-120">Une fois que vous avez appelé la surcharge de chaîne de `Property` vous pouvez chaîner des appels de configuration vous le feriez pour d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="a3138-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="a3138-121">Si le nom fourni pour le `Property` méthode correspond au nom d’une propriété existante (une propriété de clichés instantanés ou défini sur la classe d’entité), puis le code configure cette propriété existante au lieu d’introduire une nouvelle propriété de clichés instantanés.</span><span class="sxs-lookup"><span data-stu-id="a3138-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
