---
title: Propriétés de l’entité-EF Core
description: Comment configurer et mapper des propriétés d’entité à l’aide de Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502466"
---
# <a name="entity-properties"></a><span data-ttu-id="727d4-103">Propriétés d'entité</span><span class="sxs-lookup"><span data-stu-id="727d4-103">Entity Properties</span></span>

<span data-ttu-id="727d4-104">Chaque type d’entité dans votre modèle a un ensemble de propriétés, qui EF Core lit et écrit à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="727d4-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="727d4-105">Si vous utilisez une base de données relationnelle, les propriétés d’entité sont mappées à des colonnes de table.</span><span class="sxs-lookup"><span data-stu-id="727d4-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="727d4-106">Propriétés incluses et exclues</span><span class="sxs-lookup"><span data-stu-id="727d4-106">Included and excluded properties</span></span>

<span data-ttu-id="727d4-107">Par Convention, toutes les propriétés publiques avec un accesseur get et un accesseur Set seront incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="727d4-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="727d4-108">Les propriétés spécifiques peuvent être exclues comme suit :</span><span class="sxs-lookup"><span data-stu-id="727d4-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="727d4-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="727d4-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="727d4-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="727d4-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="727d4-111">Noms des colonnes</span><span class="sxs-lookup"><span data-stu-id="727d4-111">Column names</span></span>

<span data-ttu-id="727d4-112">Par Convention, lors de l’utilisation d’une base de données relationnelle, les propriétés d’entité sont mappées à des colonnes de table portant le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="727d4-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="727d4-113">Si vous préférez configurer vos colonnes avec des noms différents, vous pouvez le faire comme suit :</span><span class="sxs-lookup"><span data-stu-id="727d4-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="727d4-114">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="727d4-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="727d4-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="727d4-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="727d4-116">Types de données de colonne</span><span class="sxs-lookup"><span data-stu-id="727d4-116">Column data types</span></span>

<span data-ttu-id="727d4-117">Lors de l’utilisation d’une base de données relationnelle, le fournisseur de base de données sélectionne un type de données en fonction du type .NET de la propriété.</span><span class="sxs-lookup"><span data-stu-id="727d4-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="727d4-118">Il prend également en compte d’autres métadonnées, telles que la [longueur maximale](#maximum-length)configurée, si la propriété fait partie d’une clé primaire, etc.</span><span class="sxs-lookup"><span data-stu-id="727d4-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="727d4-119">Par exemple, SQL Server mappe des propriétés de `DateTime` à des colonnes `datetime2(7)`, et `string` des propriétés à des colonnes `nvarchar(max)` (ou à `nvarchar(450)` pour les propriétés utilisées comme clé).</span><span class="sxs-lookup"><span data-stu-id="727d4-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="727d4-120">Vous pouvez également configurer vos colonnes pour spécifier un type de données exact pour une colonne.</span><span class="sxs-lookup"><span data-stu-id="727d4-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="727d4-121">Par exemple, le code suivant configure `Url` en tant que chaîne non-Unicode avec une longueur maximale de `200` et `Rating` en tant que valeur décimale avec une précision de `5` et une échelle de `2`:</span><span class="sxs-lookup"><span data-stu-id="727d4-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="727d4-122">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="727d4-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="727d4-123">API Fluent</span><span class="sxs-lookup"><span data-stu-id="727d4-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="727d4-124">Longueur maximale</span><span class="sxs-lookup"><span data-stu-id="727d4-124">Maximum length</span></span>

<span data-ttu-id="727d4-125">La configuration d’une longueur maximale fournit aux fournisseurs de bases de données un indicateur sur le type de données de colonne approprié à choisir pour une propriété donnée.</span><span class="sxs-lookup"><span data-stu-id="727d4-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="727d4-126">La longueur maximale s’applique uniquement aux types de données de tableau, tels que `string` et `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="727d4-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="727d4-127">Entity Framework n’effectue aucune validation de longueur maximale avant de transmettre des données au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="727d4-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="727d4-128">Il revient au fournisseur ou au magasin de données de valider le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="727d4-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="727d4-129">Par exemple, lorsque vous ciblez SQL Server, le dépassement de la longueur maximale entraîne une exception, car le type de données de la colonne sous-jacente n’autorise pas le stockage des données excédentaires.</span><span class="sxs-lookup"><span data-stu-id="727d4-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="727d4-130">Dans l’exemple suivant, la configuration d’une longueur maximale de 500 entraîne la création d’une colonne de type `nvarchar(500)` sur SQL Server :</span><span class="sxs-lookup"><span data-stu-id="727d4-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="727d4-131">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="727d4-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="727d4-132">API Fluent</span><span class="sxs-lookup"><span data-stu-id="727d4-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="727d4-133">Propriétés obligatoires et facultatives</span><span class="sxs-lookup"><span data-stu-id="727d4-133">Required and optional properties</span></span>

<span data-ttu-id="727d4-134">Une propriété est considérée comme facultative si elle est valide pour contenir `null`.</span><span class="sxs-lookup"><span data-stu-id="727d4-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="727d4-135">Si `null` n’est pas une valeur valide à assigner à une propriété, elle est considérée comme étant une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="727d4-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="727d4-136">Lors du mappage à un schéma de base de données relationnelle, les propriétés requises sont créées en tant que colonnes n’acceptant pas les valeurs NULL, et les propriétés facultatives sont créées en tant que colonnes Nullable.</span><span class="sxs-lookup"><span data-stu-id="727d4-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="727d4-137">Conventions</span><span class="sxs-lookup"><span data-stu-id="727d4-137">Conventions</span></span>

<span data-ttu-id="727d4-138">Par Convention, une propriété dont le type .NET peut contenir une valeur null sera configurée comme étant facultative, alors que les propriétés dont le type .NET ne peut pas contenir de valeur null seront configurées selon les besoins.</span><span class="sxs-lookup"><span data-stu-id="727d4-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="727d4-139">Par exemple, toutes les propriétés avec des types valeur .NET (`int`, `decimal`, `bool`, etc.) sont configurées en fonction des besoins, et toutes les propriétés avec des types valeur .NET Nullable (`int?`, `decimal?`, `bool?`, etc.) sont configurées comme facultatives.</span><span class="sxs-lookup"><span data-stu-id="727d4-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="727d4-140">C#8 a introduit une nouvelle fonctionnalité appelée [types de référence Nullable](/dotnet/csharp/tutorials/nullable-reference-types), qui permet d’annoter des types de référence, ce qui indique s’il est valide qu’ils contiennent ou non des valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="727d4-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="727d4-141">Cette fonctionnalité est désactivée par défaut et, si elle est activée, elle modifie le comportement de EF Core de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="727d4-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="727d4-142">Si les types de référence Nullable sont désactivés (valeur par défaut), toutes les propriétés avec des types de référence .NET sont configurées comme étant facultatives par convention (par exemple, `string`).</span><span class="sxs-lookup"><span data-stu-id="727d4-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="727d4-143">Si les types de référence Nullable sont activés, les propriétés seront configurées en fonction de la C# possibilité de valeur null de leur type .net : `string?` sera configuré comme étant facultatif, tandis que `string` sera configuré comme requis.</span><span class="sxs-lookup"><span data-stu-id="727d4-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="727d4-144">L’exemple suivant montre un type d’entité avec des propriétés obligatoires et facultatives, avec la fonctionnalité de référence Nullable désactivée (valeur par défaut) et activée :</span><span class="sxs-lookup"><span data-stu-id="727d4-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="727d4-145">Sans types référence Nullable (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="727d4-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="727d4-146">Avec les types de référence Nullable</span><span class="sxs-lookup"><span data-stu-id="727d4-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="727d4-147">L’utilisation de types de référence Nullable est recommandée, car elle transmet la C# possibilité de valeur null exprimée dans le code à EF Core modèle et à la base de données, et évite l’utilisation de l’API Fluent ou des annotations de données pour exprimer deux fois le même concept.</span><span class="sxs-lookup"><span data-stu-id="727d4-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="727d4-148">Soyez prudent lorsque vous activez les types de référence Nullable sur un projet existant : les propriétés de type de référence qui ont été précédemment configurées comme étant facultatives sont maintenant configurées comme obligatoires, sauf si elles sont explicitement annotées comme nullables.</span><span class="sxs-lookup"><span data-stu-id="727d4-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="727d4-149">Lors de la gestion d’un schéma de base de données relationnelle, des migrations peuvent être générées, ce qui modifie la possibilité de valeur null de la colonne de base de données.</span><span class="sxs-lookup"><span data-stu-id="727d4-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="727d4-150">Pour plus d’informations sur les types de référence Nullable et leur utilisation avec EF Core, [consultez la page de documentation dédiée pour cette fonctionnalité](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="727d4-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="727d4-151">Configuration explicite</span><span class="sxs-lookup"><span data-stu-id="727d4-151">Explicit configuration</span></span>

<span data-ttu-id="727d4-152">Une propriété qui serait facultative par convention peut être configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="727d4-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="727d4-153">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="727d4-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="727d4-154">API Fluent</span><span class="sxs-lookup"><span data-stu-id="727d4-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
