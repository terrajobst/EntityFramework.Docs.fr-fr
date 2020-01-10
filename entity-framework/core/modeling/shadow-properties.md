---
title: Propriétés Shadow-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781181"
---
# <a name="shadow-properties"></a><span data-ttu-id="ce153-102">Propriétés cachées</span><span class="sxs-lookup"><span data-stu-id="ce153-102">Shadow Properties</span></span>

<span data-ttu-id="ce153-103">Les propriétés Shadow sont des propriétés qui ne sont pas définies dans votre classe d’entité .NET, mais qui sont définies pour ce type d’entité dans le modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="ce153-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="ce153-104">La valeur et l’état de ces propriétés sont gérés uniquement dans le dispositif de suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="ce153-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="ce153-105">Les propriétés Shadow sont utiles lorsque la base de données contient des données qui ne doivent pas être exposées sur les types d’entités mappés.</span><span class="sxs-lookup"><span data-stu-id="ce153-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="ce153-106">Propriétés de l’ombre de la clé étrangère</span><span class="sxs-lookup"><span data-stu-id="ce153-106">Foreign key shadow properties</span></span>

<span data-ttu-id="ce153-107">Les propriétés Shadow sont le plus souvent utilisées pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entité à l’aide des propriétés de navigation entre l’entité modes.</span><span class="sxs-lookup"><span data-stu-id="ce153-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="ce153-108">Par Convention, EF introduit une propriété Shadow lorsqu’une relation est détectée, mais qu’aucune propriété de clé étrangère n’est trouvée dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="ce153-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="ce153-109">La propriété sera nommée `<navigation property name><principal key property name>` (la navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisée pour le nommage).</span><span class="sxs-lookup"><span data-stu-id="ce153-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="ce153-110">Si le nom de la propriété de clé principale comprend le nom de la propriété de navigation, le nom sera simplement `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="ce153-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="ce153-111">S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom du type de principal est utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="ce153-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="ce153-112">Par exemple, la liste de code suivante entraînera l’introduction d’une propriété Shadow `BlogId` dans l’entité `Post` :</span><span class="sxs-lookup"><span data-stu-id="ce153-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="ce153-113">Configuration des propriétés Shadow</span><span class="sxs-lookup"><span data-stu-id="ce153-113">Configuring shadow properties</span></span>

<span data-ttu-id="ce153-114">Vous pouvez utiliser l’API Fluent pour configurer les propriétés Shadow.</span><span class="sxs-lookup"><span data-stu-id="ce153-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="ce153-115">Une fois que vous avez appelé la surcharge de chaîne de `Property`, vous pouvez chaîner n’importe quel appel de configuration que vous feriez pour d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="ce153-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="ce153-116">Dans l’exemple suivant, étant donné que `Blog` n’a aucune propriété CLR nommée `LastUpdated`, une propriété Shadow est créée :</span><span class="sxs-lookup"><span data-stu-id="ce153-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="ce153-117">Si le nom fourni à la méthode `Property` correspond au nom d’une propriété existante (une propriété Shadow ou une propriété Shadow définie sur la classe d’entité), le code configurera cette propriété existante au lieu d’introduire une nouvelle propriété Shadow.</span><span class="sxs-lookup"><span data-stu-id="ce153-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="ce153-118">Accès aux propriétés Shadow</span><span class="sxs-lookup"><span data-stu-id="ce153-118">Accessing shadow properties</span></span>

<span data-ttu-id="ce153-119">Les valeurs de propriété Shadow peuvent être obtenues et modifiées par le biais de l’API `ChangeTracker` :</span><span class="sxs-lookup"><span data-stu-id="ce153-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="ce153-120">Les propriétés Shadow peuvent être référencées dans des requêtes LINQ via la méthode statique `EF.Property` :</span><span class="sxs-lookup"><span data-stu-id="ce153-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
