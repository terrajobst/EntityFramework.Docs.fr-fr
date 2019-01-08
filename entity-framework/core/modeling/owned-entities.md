---
title: Appartenant à des Types d’entité - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069202"
---
# <a name="owned-entity-types"></a><span data-ttu-id="95150-102">Types d’entité détenus</span><span class="sxs-lookup"><span data-stu-id="95150-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="95150-103">Cette fonctionnalité est une nouveauté dans EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="95150-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="95150-104">EF Core vous permet aux types d’entité de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités.</span><span class="sxs-lookup"><span data-stu-id="95150-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="95150-105">Ils sont appelés _types d’entités détenus_.</span><span class="sxs-lookup"><span data-stu-id="95150-105">These are called _owned entity types_.</span></span> <span data-ttu-id="95150-106">L’entité qui contient un type d’entité détenu est son _propriétaire_.</span><span class="sxs-lookup"><span data-stu-id="95150-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="95150-107">Configuration explicite</span><span class="sxs-lookup"><span data-stu-id="95150-107">Explicit configuration</span></span>

<span data-ttu-id="95150-108">La propriété entité types sont jamais incluses par EF Core dans le modèle par convention.</span><span class="sxs-lookup"><span data-stu-id="95150-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="95150-109">Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttribute` (nouveau dans EF Core 2.1) pour configurer le type comme un type détenu.</span><span class="sxs-lookup"><span data-stu-id="95150-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="95150-110">Dans cet exemple, `StreetAddress` est un type avec aucune propriété d’identité.</span><span class="sxs-lookup"><span data-stu-id="95150-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="95150-111">Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière.</span><span class="sxs-lookup"><span data-stu-id="95150-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="95150-112">Nous pouvons utiliser la `OwnedAttribute` pour le traiter comme une entité détenue lorsque référencé depuis un autre type d’entité :</span><span class="sxs-lookup"><span data-stu-id="95150-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="95150-113">Il est également possible d’utiliser le `OwnsOne` méthode dans `OnModelCreating` pour spécifier que le `ShippingAddress` propriété est une entité appartenant à de la `Order` type d’entité et de configurer des facettes supplémentaires si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="95150-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="95150-114">Si le `ShippingAddress` propriété est privée dans le `Order` type, vous pouvez utiliser la version de chaîne de la `OwnsOne` méthode :</span><span class="sxs-lookup"><span data-stu-id="95150-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="95150-115">Consultez le [exemple complet de projet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pour plus de contexte.</span><span class="sxs-lookup"><span data-stu-id="95150-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="95150-116">Clés implicites</span><span class="sxs-lookup"><span data-stu-id="95150-116">Implicit keys</span></span>

<span data-ttu-id="95150-117">Configuré avec des types détenus `OwnsOne` ou découverts grâce à une navigation de référence ont toujours une relation un à un avec le propriétaire, par conséquent, ils n’ont pas leurs propres valeurs de clés que les valeurs de clé étrangères sont uniques.</span><span class="sxs-lookup"><span data-stu-id="95150-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="95150-118">Dans l’exemple précédent, le `StreetAddress` type n’a pas besoin de définir une propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="95150-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="95150-119">Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [occulter la propriété](xref:core/modeling/shadow-properties) pour le type détenu.</span><span class="sxs-lookup"><span data-stu-id="95150-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="95150-120">La valeur de la clé d’une instance du type détenu sera identique à la valeur de la clé de l’instance de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="95150-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="95150-121">Collections de types détenus</span><span class="sxs-lookup"><span data-stu-id="95150-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="95150-122">Cette fonctionnalité est une nouveauté d’EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="95150-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="95150-123">Pour configurer une collection de types détenus `OwnsMany` doit être utilisé dans `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="95150-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="95150-124">Toutefois la clé primaire ne sera pas configurée par convention, donc il doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="95150-124">However the primary key will not be configured by convention, so it needs to be specified explicitly.</span></span> <span data-ttu-id="95150-125">Il est courant d’utiliser une clé complexe pour ces types d’entités incorporant la clé étrangère pour le propriétaire et une propriété unique supplémentaire qui peut également être dans l’état de clichés instantanés :</span><span class="sxs-lookup"><span data-stu-id="95150-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="95150-126">Mappage de types avec fractionnement de table détenu</span><span class="sxs-lookup"><span data-stu-id="95150-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="95150-127">Lorsque vous utilisez relationnelles bases de données, par référence de la convention détenus types sont mappés à la même table que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="95150-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="95150-128">Cela nécessite de fractionnement de la table en deux : certaines colonnes seront utilisées pour stocker les données du propriétaire, et certaines colonnes seront utilisées pour stocker les données de l’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="95150-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="95150-129">Il s’agit d’une fonctionnalité commune appelée fractionnement de table.</span><span class="sxs-lookup"><span data-stu-id="95150-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="95150-130">Détenus types stockés avec le fractionnement de table peuvent être utilisé de la même façon pour les types complexes comment sont utilisées dans EF6.</span><span class="sxs-lookup"><span data-stu-id="95150-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="95150-131">Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité détenu suivant le modèle _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="95150-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="95150-132">Par conséquent le `StreetAddress` propriétés s’affichent dans la table « Orders » avec les noms « ShippingAddress_Street » et « ShippingAddress_City ».</span><span class="sxs-lookup"><span data-stu-id="95150-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="95150-133">Vous pouvez ajouter la `HasColumnName` méthode pour renommer ces colonnes :</span><span class="sxs-lookup"><span data-stu-id="95150-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="95150-134">Partage le même type .NET parmi plusieurs types détenus</span><span class="sxs-lookup"><span data-stu-id="95150-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="95150-135">Type d’entité détenu peut être du même type .NET en tant qu’un autre type d’entité détenu, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type détenu.</span><span class="sxs-lookup"><span data-stu-id="95150-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="95150-136">Dans ce cas, la propriété qui pointe du propriétaire à l’entité détenue devient le _définition navigation_ du type d’entité détenu.</span><span class="sxs-lookup"><span data-stu-id="95150-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="95150-137">Du point de vue d’EF Core, la navigation de définition fait partie de l’identité du type en même temps que le type .NET.</span><span class="sxs-lookup"><span data-stu-id="95150-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="95150-138">Par exemple, dans la classe suivante `ShippingAddress` et `BillingAddress` sont tous deux du même type .NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="95150-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="95150-139">Pour comprendre comment EF Core distinguera suivies des instances de ces objets, il peut être utile de considérer que la navigation définie est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.</span><span class="sxs-lookup"><span data-stu-id="95150-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="95150-140">Types détenus imbriqués</span><span class="sxs-lookup"><span data-stu-id="95150-140">Nested owned types</span></span>

<span data-ttu-id="95150-141">Dans cet exemple `OrderDetails` possède `BillingAddress` et `ShippingAddress`, qui sont toutes deux `StreetAddress` types.</span><span class="sxs-lookup"><span data-stu-id="95150-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="95150-142">`OrderDetails` est alors détenu par le type `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="95150-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="95150-143">Outre les types détenus imbriqués, un type détenu peut référencer une entité normale, il peut être le propriétaire ou une autre entité tant que l’entité détenus est sur le côté dépendant.</span><span class="sxs-lookup"><span data-stu-id="95150-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="95150-144">Cette fonctionnalité définit les types d’entité détenus en dehors des types complexes dans EF6.</span><span class="sxs-lookup"><span data-stu-id="95150-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="95150-145">Il est possible de chaîner les `OwnsOne` méthode dans un appel fluent afin de configurer ce modèle :</span><span class="sxs-lookup"><span data-stu-id="95150-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="95150-146">Il est également possible de réaliser la même chose à l’aide `OwnedAttribute` sur les deux `OrderDetails` et `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="95150-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="95150-147">Stockage des types détenus dans des tables distinctes</span><span class="sxs-lookup"><span data-stu-id="95150-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="95150-148">Également Contrairement aux types complexes d’EF6, types détenus peuvent être stockées dans une table distincte du propriétaire.</span><span class="sxs-lookup"><span data-stu-id="95150-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="95150-149">Pour substituer la convention qui mappe un type détenu à la même table en tant que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table.</span><span class="sxs-lookup"><span data-stu-id="95150-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="95150-150">L’exemple suivant mappe `OrderDetails` et ses deux adresses dans une table distincte de `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="95150-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="95150-151">Interrogation des types détenus</span><span class="sxs-lookup"><span data-stu-id="95150-151">Querying owned types</span></span>

<span data-ttu-id="95150-152">Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="95150-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="95150-153">Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockées dans une table distincte.</span><span class="sxs-lookup"><span data-stu-id="95150-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="95150-154">Selon le modèle décrit précédemment, la requête suivante obtiendra `Order`, `OrderDetails` et les deux détenus `StreetAddresses` à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="95150-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="95150-155">Limitations</span><span class="sxs-lookup"><span data-stu-id="95150-155">Limitations</span></span>

<span data-ttu-id="95150-156">Certaines de ces restrictions sont fondamentaux pour comment détenu travail de types d’entité, mais d’autres sont des restrictions que nous pouvons être en mesure de supprimer dans les futures versions :</span><span class="sxs-lookup"><span data-stu-id="95150-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="95150-157">Restrictions par conception</span><span class="sxs-lookup"><span data-stu-id="95150-157">By-design restrictions</span></span>
- <span data-ttu-id="95150-158">Vous ne pouvez pas créer un `DbSet<T>` pour un type détenu</span><span class="sxs-lookup"><span data-stu-id="95150-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="95150-159">Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="95150-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="95150-160">Défauts actuels</span><span class="sxs-lookup"><span data-stu-id="95150-160">Current shortcomings</span></span>
- <span data-ttu-id="95150-161">Hiérarchies d’héritage qui incluent détenus types d’entité ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="95150-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="95150-162">Les navigations de référence pour détenus types d’entité ne peut pas être null, sauf si elles sont explicitement mappées à une table distincte du propriétaire</span><span class="sxs-lookup"><span data-stu-id="95150-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="95150-163">Instances de types d’entité détenus ne peut pas être partagées par plusieurs propriétaires (il s’agit d’un scénario bien connu pour les objets de valeur qui ne peut pas être implémenté à l’aide des types d’entité détenus)</span><span class="sxs-lookup"><span data-stu-id="95150-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="95150-164">Lacunes dans les versions précédentes</span><span class="sxs-lookup"><span data-stu-id="95150-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="95150-165">Dans EF Core 2.0, navigations à la propriété types d’entité ne peut pas être déclarés dans les types d’entité dérivés, sauf si les entités sont explicitement mappées à une table distincte de la hiérarchie de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="95150-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="95150-166">Cette limitation a été supprimée dans EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="95150-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="95150-167">Dans EF Core 2.0 et 2.1 seule référence navigations vers les types détenus ont été pris en charge.</span><span class="sxs-lookup"><span data-stu-id="95150-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="95150-168">Cette limitation a été supprimée dans EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="95150-168">This limitation has been removed in EF Core 2.2</span></span>
