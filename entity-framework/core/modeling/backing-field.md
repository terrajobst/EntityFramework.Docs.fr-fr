---
title: Champs de stockage-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197482"
---
# <a name="backing-fields"></a><span data-ttu-id="7ae58-102">Champs de stockage</span><span class="sxs-lookup"><span data-stu-id="7ae58-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="7ae58-103">Cette fonctionnalité est une nouveauté de EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="7ae58-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="7ae58-104">Les champs de stockage permettent à EF de lire et/ou d’écrire dans un champ plutôt que sur une propriété.</span><span class="sxs-lookup"><span data-stu-id="7ae58-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="7ae58-105">Cela peut être utile lorsque l’encapsulation dans la classe est utilisée pour limiter l’utilisation de et/ou améliorer la sémantique de l’accès aux données par le code d’application, mais la valeur doit être lue et/ou écrite dans la base de données sans utiliser ces restrictions/ améliorations.</span><span class="sxs-lookup"><span data-stu-id="7ae58-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="7ae58-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="7ae58-106">Conventions</span></span>

<span data-ttu-id="7ae58-107">Par Convention, les champs suivants sont découverts en tant que champs de stockage pour une propriété donnée (liste dans l’ordre de priorité).</span><span class="sxs-lookup"><span data-stu-id="7ae58-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="7ae58-108">Les champs sont uniquement détectés pour les propriétés incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="7ae58-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="7ae58-109">Pour plus d’informations sur les propriétés incluses dans le modèle, consultez [inclusion de & exclusion de propriétés](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="7ae58-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="7ae58-110">Lorsqu’un champ de stockage est configuré, EF écrit directement dans ce champ lors de la matérialisation des instances d’entité de la base de données (au lieu d’utiliser l’accesseur Set de propriété).</span><span class="sxs-lookup"><span data-stu-id="7ae58-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="7ae58-111">Si EF doit lire ou écrire la valeur à d’autres moments, il utilisera la propriété si possible.</span><span class="sxs-lookup"><span data-stu-id="7ae58-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="7ae58-112">Par exemple, si EF doit mettre à jour la valeur d’une propriété, il utilisera la méthode setter de propriété si celle-ci est définie.</span><span class="sxs-lookup"><span data-stu-id="7ae58-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="7ae58-113">Si la propriété est en lecture seule, elle écrit dans le champ.</span><span class="sxs-lookup"><span data-stu-id="7ae58-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7ae58-114">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="7ae58-114">Data Annotations</span></span>

<span data-ttu-id="7ae58-115">Les champs de stockage ne peuvent pas être configurés avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="7ae58-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7ae58-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7ae58-116">Fluent API</span></span>

<span data-ttu-id="7ae58-117">Vous pouvez utiliser l’API Fluent pour configurer un champ de stockage pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="7ae58-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="7ae58-118">Contrôle de l’utilisation du champ</span><span class="sxs-lookup"><span data-stu-id="7ae58-118">Controlling when the field is used</span></span>

<span data-ttu-id="7ae58-119">Vous pouvez configurer le moment où EF utilise le champ ou la propriété.</span><span class="sxs-lookup"><span data-stu-id="7ae58-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="7ae58-120">Consultez l' [énumération PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) pour connaître les options prises en charge.</span><span class="sxs-lookup"><span data-stu-id="7ae58-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="7ae58-121">Champs sans propriété</span><span class="sxs-lookup"><span data-stu-id="7ae58-121">Fields without a property</span></span>

<span data-ttu-id="7ae58-122">Vous pouvez également créer une propriété conceptuelle dans votre modèle qui n’a pas de propriété CLR correspondante dans la classe d’entité, mais utilise à la place un champ pour stocker les données dans l’entité.</span><span class="sxs-lookup"><span data-stu-id="7ae58-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="7ae58-123">Cela diffère des [Propriétés Shadow](shadow-properties.md), où les données sont stockées dans le dispositif de suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="7ae58-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="7ae58-124">Cela est généralement utilisé si la classe d’entité utilise des méthodes pour récupérer/définir des valeurs.</span><span class="sxs-lookup"><span data-stu-id="7ae58-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="7ae58-125">Vous pouvez indiquer à EF le nom du champ dans l' `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="7ae58-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="7ae58-126">S’il n’existe aucune propriété avec le nom donné, EF recherche un champ.</span><span class="sxs-lookup"><span data-stu-id="7ae58-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="7ae58-127">Vous pouvez également choisir d’attribuer un nom à la propriété, autre que le nom du champ.</span><span class="sxs-lookup"><span data-stu-id="7ae58-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="7ae58-128">Ce nom est ensuite utilisé lors de la création du modèle, le plus particulièrement, il sera utilisé pour le nom de colonne qui est mappé à dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7ae58-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="7ae58-129">Lorsqu’il n’y a aucune propriété dans la classe d’entité, vous `EF.Property(...)` pouvez utiliser la méthode dans une requête LINQ pour faire référence à la propriété qui fait partie conceptuellement du modèle.</span><span class="sxs-lookup"><span data-stu-id="7ae58-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
