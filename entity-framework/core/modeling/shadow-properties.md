---
title: Propriétés Shadow-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655962"
---
# <a name="shadow-properties"></a><span data-ttu-id="21630-102">Propriétés cachées</span><span class="sxs-lookup"><span data-stu-id="21630-102">Shadow Properties</span></span>

<span data-ttu-id="21630-103">Les propriétés Shadow sont des propriétés qui ne sont pas définies dans votre classe d’entité .NET, mais qui sont définies pour ce type d’entité dans le modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="21630-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="21630-104">La valeur et l’état de ces propriétés sont gérés uniquement dans le dispositif de suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="21630-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="21630-105">Les propriétés Shadow sont utiles lorsque la base de données contient des données qui ne doivent pas être exposées sur les types d’entités mappés.</span><span class="sxs-lookup"><span data-stu-id="21630-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="21630-106">Ils sont le plus souvent utilisés pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entité à l’aide des propriétés de navigation entre les types d’entités.</span><span class="sxs-lookup"><span data-stu-id="21630-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="21630-107">Les valeurs de propriété Shadow peuvent être obtenues et modifiées par le biais de l’API `ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="21630-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="21630-108">Les propriétés Shadow peuvent être référencées dans des requêtes LINQ via la méthode statique `EF.Property`.</span><span class="sxs-lookup"><span data-stu-id="21630-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="21630-109">Conventions</span><span class="sxs-lookup"><span data-stu-id="21630-109">Conventions</span></span>

<span data-ttu-id="21630-110">Les propriétés Shadow peuvent être créées par convention lorsqu’une relation est détectée, mais qu’aucune propriété de clé étrangère n’est trouvée dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="21630-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="21630-111">Dans ce cas, une propriété de clé étrangère Shadow est introduite.</span><span class="sxs-lookup"><span data-stu-id="21630-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="21630-112">La propriété de clé étrangère Shadow est nommée `<navigation property name><principal key property name>` (la navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisée pour le nommage).</span><span class="sxs-lookup"><span data-stu-id="21630-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="21630-113">Si le nom de la propriété de clé principale comprend le nom de la propriété de navigation, le nom sera simplement `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="21630-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="21630-114">S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom du type de principal est utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="21630-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="21630-115">Par exemple, la liste de code suivante entraînera l’introduction d’une propriété Shadow `BlogId` dans l’entité `Post`.</span><span class="sxs-lookup"><span data-stu-id="21630-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a><span data-ttu-id="21630-116">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="21630-116">Data Annotations</span></span>

<span data-ttu-id="21630-117">Les propriétés Shadow ne peuvent pas être créées avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="21630-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="21630-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="21630-118">Fluent API</span></span>

<span data-ttu-id="21630-119">Vous pouvez utiliser l’API Fluent pour configurer les propriétés Shadow.</span><span class="sxs-lookup"><span data-stu-id="21630-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="21630-120">Une fois que vous avez appelé la surcharge de chaîne de `Property` vous pouvez chaîner n’importe quel appel de configuration que vous feriez pour d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="21630-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="21630-121">Si le nom fourni à la méthode `Property` correspond au nom d’une propriété existante (une propriété Shadow ou une propriété Shadow définie sur la classe d’entité), le code configurera cette propriété existante au lieu d’introduire une nouvelle propriété Shadow.</span><span class="sxs-lookup"><span data-stu-id="21630-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
