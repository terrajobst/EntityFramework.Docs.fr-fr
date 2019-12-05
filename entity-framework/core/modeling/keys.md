---
title: Clés (principale)-EF Core
description: Comment configurer des clés pour des types d’entités lors de l’utilisation de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824623"
---
# <a name="keys-primary"></a><span data-ttu-id="383cf-103">Clés (primaires)</span><span class="sxs-lookup"><span data-stu-id="383cf-103">Keys (primary)</span></span>

<span data-ttu-id="383cf-104">Une clé sert d’identificateur unique principal pour chaque instance d’entité.</span><span class="sxs-lookup"><span data-stu-id="383cf-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="383cf-105">Lorsque vous utilisez une base de données relationnelle, cela correspond au concept d’une *clé primaire*.</span><span class="sxs-lookup"><span data-stu-id="383cf-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="383cf-106">Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (pour plus d’informations, consultez [autres clés](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="383cf-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="383cf-107">L’une des méthodes suivantes peut être utilisée pour configurer/créer une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="383cf-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="383cf-108">Conventions</span><span class="sxs-lookup"><span data-stu-id="383cf-108">Conventions</span></span>

<span data-ttu-id="383cf-109">Par défaut, une propriété nommée `Id` ou `<type name>Id` sera configurée comme clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="383cf-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="383cf-110">Les [types d’entités détenues](xref:core/modeling/owned-entities) utilisent des règles différentes pour définir des clés.</span><span class="sxs-lookup"><span data-stu-id="383cf-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="383cf-111">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="383cf-111">Data Annotations</span></span>

<span data-ttu-id="383cf-112">Vous pouvez utiliser des annotations de données pour configurer une seule propriété en tant que clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="383cf-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="383cf-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="383cf-113">Fluent API</span></span>

<span data-ttu-id="383cf-114">Vous pouvez utiliser l’API Fluent pour configurer une propriété unique en tant que clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="383cf-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="383cf-115">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé d’une entité (appelée clé composite).</span><span class="sxs-lookup"><span data-stu-id="383cf-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="383cf-116">Les clés composites ne peuvent être configurées qu’à l’aide des conventions de l’API Fluent qui ne configureront jamais de clé composite, et vous ne pourrez pas utiliser d’annotations de données pour en configurer une.</span><span class="sxs-lookup"><span data-stu-id="383cf-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="383cf-117">Types et valeurs de clé</span><span class="sxs-lookup"><span data-stu-id="383cf-117">Key types and values</span></span>

<span data-ttu-id="383cf-118">EF Core prend en charge l’utilisation de propriétés de n’importe quel type primitif comme clé primaire, y compris `string`, `Guid`, `byte[]` et autres.</span><span class="sxs-lookup"><span data-stu-id="383cf-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="383cf-119">Mais toutes les bases de données ne les prennent pas en charge.</span><span class="sxs-lookup"><span data-stu-id="383cf-119">But not all databases support them.</span></span> <span data-ttu-id="383cf-120">Dans certains cas, les valeurs de clé peuvent être converties automatiquement en un type pris en charge ; sinon, la conversion doit être [spécifiée manuellement](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="383cf-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="383cf-121">Les propriétés de clé doivent toujours avoir une valeur non définie par défaut lors de l’ajout d’une nouvelle entité au contexte, mais certains types sont [générés par la base de données](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="383cf-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="383cf-122">Dans ce cas, EF tente de générer une valeur temporaire lorsque l’entité est ajoutée à des fins de suivi.</span><span class="sxs-lookup"><span data-stu-id="383cf-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="383cf-123">Une fois [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) appelé, la valeur temporaire est remplacée par la valeur générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="383cf-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="383cf-124">Si une propriété de clé a une valeur générée par la base de données et qu’une valeur non définie par défaut est spécifiée lors de l’ajout d’une entité, EF suppose que l’entité existe déjà dans la base de données et essaiera de la mettre à jour au lieu d’en insérer une nouvelle.</span><span class="sxs-lookup"><span data-stu-id="383cf-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="383cf-125">Pour éviter cela, désactivez la génération de valeur ou consultez [comment spécifier des valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="383cf-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>