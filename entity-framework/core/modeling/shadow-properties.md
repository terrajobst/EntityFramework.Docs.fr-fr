---
title: Propriétés Shadow-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197704"
---
# <a name="shadow-properties"></a><span data-ttu-id="d74af-102">Propriétés Shadow</span><span class="sxs-lookup"><span data-stu-id="d74af-102">Shadow Properties</span></span>

<span data-ttu-id="d74af-103">Les propriétés Shadow sont des propriétés qui ne sont pas définies dans votre classe d’entité .NET, mais qui sont définies pour ce type d’entité dans le modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="d74af-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="d74af-104">La valeur et l’état de ces propriétés sont gérés uniquement dans le dispositif de suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="d74af-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="d74af-105">Les propriétés Shadow sont utiles lorsque la base de données contient des données qui ne doivent pas être exposées sur les types d’entités mappés.</span><span class="sxs-lookup"><span data-stu-id="d74af-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="d74af-106">Ils sont le plus souvent utilisés pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entité à l’aide des propriétés de navigation entre les types d’entités.</span><span class="sxs-lookup"><span data-stu-id="d74af-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="d74af-107">Les valeurs de propriété Shadow peuvent être obtenues et modifiées `ChangeTracker` par le biais de l’API.</span><span class="sxs-lookup"><span data-stu-id="d74af-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="d74af-108">Les propriétés Shadow peuvent être référencées dans des requêtes LINQ `EF.Property` via la méthode statique.</span><span class="sxs-lookup"><span data-stu-id="d74af-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="d74af-109">Conventions</span><span class="sxs-lookup"><span data-stu-id="d74af-109">Conventions</span></span>

<span data-ttu-id="d74af-110">Les propriétés Shadow peuvent être créées par convention lorsqu’une relation est détectée, mais qu’aucune propriété de clé étrangère n’est trouvée dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="d74af-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="d74af-111">Dans ce cas, une propriété de clé étrangère Shadow est introduite.</span><span class="sxs-lookup"><span data-stu-id="d74af-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="d74af-112">La propriété de clé étrangère Shadow sera nommée `<navigation property name><principal key property name>` (la navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisée pour le nommage).</span><span class="sxs-lookup"><span data-stu-id="d74af-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="d74af-113">Si le nom de la propriété de clé principale comprend le nom de la propriété de navigation, le nom `<principal key property name>`sera simplement.</span><span class="sxs-lookup"><span data-stu-id="d74af-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="d74af-114">S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom du type de principal est utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="d74af-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="d74af-115">Par exemple, la liste de code suivante entraînera l' `BlogId` introduction d’une propriété Shadow à `Post` l’entité.</span><span class="sxs-lookup"><span data-stu-id="d74af-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="d74af-116">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="d74af-116">Data Annotations</span></span>

<span data-ttu-id="d74af-117">Les propriétés Shadow ne peuvent pas être créées avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="d74af-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d74af-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d74af-118">Fluent API</span></span>

<span data-ttu-id="d74af-119">Vous pouvez utiliser l’API Fluent pour configurer les propriétés Shadow.</span><span class="sxs-lookup"><span data-stu-id="d74af-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="d74af-120">Une fois que vous avez appelé la surcharge `Property` de chaîne de, vous pouvez chaîner n’importe quel appel de configuration que vous feriez pour d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="d74af-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="d74af-121">Si le nom fourni à la `Property` méthode correspond au nom d’une propriété existante (une propriété Shadow ou une propriété Shadow définie sur la classe d’entité), le code configurera cette propriété existante au lieu d’introduire une nouvelle propriété Shadow.</span><span class="sxs-lookup"><span data-stu-id="d74af-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
