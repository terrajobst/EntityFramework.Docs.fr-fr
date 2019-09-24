---
title: Propriétés obligatoires et facultatives-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197841"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="8b6e5-102">Propriétés obligatoires et facultatives</span><span class="sxs-lookup"><span data-stu-id="8b6e5-102">Required and Optional Properties</span></span>

<span data-ttu-id="8b6e5-103">Une propriété est considérée comme facultative si elle est valide pour contenir `null`.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="8b6e5-104">Si `null` n’est pas une valeur valide à assigner à une propriété, elle est considérée comme étant une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="8b6e5-105">Lors du mappage à un schéma de base de données relationnelle, les propriétés requises sont créées en tant que colonnes n’acceptant pas les valeurs NULL, et les propriétés facultatives sont créées en tant que colonnes Nullable.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="8b6e5-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="8b6e5-106">Conventions</span></span>

<span data-ttu-id="8b6e5-107">Par Convention, une propriété dont le type .NET peut contenir une valeur null sera configurée comme étant facultative, alors que les propriétés dont le type .NET ne peut pas contenir de valeur null seront configurées selon les besoins.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="8b6e5-108">Par exemple, toutes les propriétés avec des types valeur`int`.NET `decimal`( `bool`,,, etc.) sont configurées en fonction des besoins, et toutes les propriétés avec `decimal?`des `bool?`types valeur .net Nullable (`int?`,,, etc.) sont configuré comme option facultative.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="8b6e5-109">C#8 a introduit une nouvelle fonctionnalité appelée [types de référence Nullable](/dotnet/csharp/tutorials/nullable-reference-types), qui permet d’annoter des types de référence, ce qui indique s’il est valide qu’ils contiennent ou non des valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="8b6e5-110">Cette fonctionnalité est désactivée par défaut et, si elle est activée, elle modifie le comportement de EF Core de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="8b6e5-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="8b6e5-111">Si les types de référence Nullable sont désactivés (valeur par défaut), toutes les propriétés avec des types de référence .NET sont `string`configurées comme étant facultatives par convention (par exemple,).</span><span class="sxs-lookup"><span data-stu-id="8b6e5-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="8b6e5-112">Si les types de référence Nullable sont activés, les propriétés seront configurées en fonction de la C# possibilité de `string?` valeur null de leur type .net : `string` sera configuré comme étant facultatif, tandis que sera configuré comme requis.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="8b6e5-113">L’exemple suivant montre un type d’entité avec des propriétés obligatoires et facultatives, avec la fonctionnalité de référence Nullable désactivée (valeur par défaut) et activée :</span><span class="sxs-lookup"><span data-stu-id="8b6e5-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="8b6e5-114">Sans types référence Nullable (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="8b6e5-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="8b6e5-115">Avec les types de référence Nullable</span><span class="sxs-lookup"><span data-stu-id="8b6e5-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="8b6e5-116">L’utilisation de types de référence Nullable est recommandée, car elle transmet la C# possibilité de valeur null exprimée dans le code à EF Core modèle et à la base de données, et évite l’utilisation de l’API Fluent ou des annotations de données pour exprimer deux fois le même concept.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="8b6e5-117">Soyez prudent lorsque vous activez les types de référence Nullable sur un projet existant : les propriétés de type de référence qui ont été précédemment configurées comme étant facultatives sont maintenant configurées comme obligatoires, sauf si elles sont explicitement annotées comme nullables.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="8b6e5-118">Lors de la gestion d’un schéma de base de données relationnelle, des migrations peuvent être générées, ce qui modifie la possibilité de valeur null de la colonne de base de données.</span><span class="sxs-lookup"><span data-stu-id="8b6e5-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="8b6e5-119">Pour plus d’informations sur les types de référence Nullable et leur utilisation avec EF Core, [consultez la page de documentation dédiée pour cette fonctionnalité](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="8b6e5-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="8b6e5-120">Configuration</span><span class="sxs-lookup"><span data-stu-id="8b6e5-120">Configuration</span></span>

<span data-ttu-id="8b6e5-121">Une propriété qui serait facultative par convention peut être configurée comme suit :</span><span class="sxs-lookup"><span data-stu-id="8b6e5-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="8b6e5-122">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="8b6e5-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="8b6e5-123">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8b6e5-123">Fluent API</span></span>](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***