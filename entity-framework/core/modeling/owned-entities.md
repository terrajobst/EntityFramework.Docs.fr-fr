---
title: Types d’entités détenues-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149139"
---
# <a name="owned-entity-types"></a><span data-ttu-id="6d355-102">Types d’entité détenus</span><span class="sxs-lookup"><span data-stu-id="6d355-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="6d355-103">Cette fonctionnalité est une nouveauté de EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="6d355-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="6d355-104">EF Core vous permet de modéliser des types d’entité qui peuvent uniquement apparaître dans les propriétés de navigation d’autres types d’entités.</span><span class="sxs-lookup"><span data-stu-id="6d355-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="6d355-105">Il s’agit de _types d’entités détenues_.</span><span class="sxs-lookup"><span data-stu-id="6d355-105">These are called _owned entity types_.</span></span> <span data-ttu-id="6d355-106">L’entité contenant un type d’entité détenu est son _propriétaire_.</span><span class="sxs-lookup"><span data-stu-id="6d355-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="6d355-107">Les entités détenues sont essentiellement une partie du propriétaire et ne peuvent pas exister sans elles, elles sont conceptuellement similaires aux [agrégats](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="6d355-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="6d355-108">Configuration explicite</span><span class="sxs-lookup"><span data-stu-id="6d355-108">Explicit configuration</span></span>

<span data-ttu-id="6d355-109">Les types d’entités détenus ne sont jamais inclus par EF Core dans le modèle par Convention.</span><span class="sxs-lookup"><span data-stu-id="6d355-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="6d355-110">Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annoter le type `OwnedAttribute` avec (nouveauté de EF Core 2,1) pour configurer le type en tant que type détenu.</span><span class="sxs-lookup"><span data-stu-id="6d355-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="6d355-111">Dans cet exemple, `StreetAddress` est un type sans propriété d’identité.</span><span class="sxs-lookup"><span data-stu-id="6d355-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="6d355-112">Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière.</span><span class="sxs-lookup"><span data-stu-id="6d355-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="6d355-113">Nous pouvons utiliser le `OwnedAttribute` pour le traiter en tant qu’entité possédée lorsqu’elle est référencée à partir d’un autre type d’entité :</span><span class="sxs-lookup"><span data-stu-id="6d355-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="6d355-114">Il est également possible d’utiliser la `OwnsOne` méthode dans `OnModelCreating` pour spécifier que la `ShippingAddress` propriété est une entité appartenant au `Order` type d’entité et pour configurer des facettes supplémentaires, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="6d355-115">Si la `ShippingAddress` propriété est privée dans le `Order` type, vous pouvez utiliser la version de chaîne de `OwnsOne` la méthode :</span><span class="sxs-lookup"><span data-stu-id="6d355-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="6d355-116">Pour plus de contexte, consultez l' [exemple de projet complet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .</span><span class="sxs-lookup"><span data-stu-id="6d355-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="6d355-117">Clés implicites</span><span class="sxs-lookup"><span data-stu-id="6d355-117">Implicit keys</span></span>

<span data-ttu-id="6d355-118">Les types détenus configurés `OwnsOne` ou découverts via une navigation de référence ont toujours une relation un-à-un avec le propriétaire, donc ils n’ont pas besoin de leurs propres valeurs de clés, car les valeurs de clé étrangère sont uniques.</span><span class="sxs-lookup"><span data-stu-id="6d355-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="6d355-119">Dans l’exemple précédent, le `StreetAddress` type n’a pas besoin de définir une propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="6d355-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="6d355-120">Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de savoir qu’une clé primaire est créée en tant que [propriété Shadow](xref:core/modeling/shadow-properties) pour le type détenu.</span><span class="sxs-lookup"><span data-stu-id="6d355-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="6d355-121">La valeur de la clé d’une instance du type détenu sera identique à la valeur de la clé de l’instance propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="6d355-122">Collections de types détenus</span><span class="sxs-lookup"><span data-stu-id="6d355-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="6d355-123">Cette fonctionnalité est une nouveauté d’EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="6d355-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="6d355-124">Pour configurer une collection de types détenus, `OwnsMany` utilisez `OnModelCreating`dans.</span><span class="sxs-lookup"><span data-stu-id="6d355-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="6d355-125">Les types détenus nécessitent une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-125">Owned types need a primary key.</span></span> <span data-ttu-id="6d355-126">S’il n’existe aucune propriété candidate correcte sur le type .NET, EF Core pouvez essayer d’en créer un.</span><span class="sxs-lookup"><span data-stu-id="6d355-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="6d355-127">Toutefois, lorsque des types détenus sont définis par le biais d’une collection, il suffit de créer une propriété Shadow pour agir à la fois comme clé étrangère dans le propriétaire et la clé primaire de l’instance appartenant, comme `OwnsOne`nous le faisons pour : il peut y avoir plusieurs instances de type détenu pour chaque propriétaire et, par conséquent, la clé du propriétaire n’est pas suffisant pour fournir une identité unique pour chaque instance détenue.</span><span class="sxs-lookup"><span data-stu-id="6d355-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="6d355-128">Les deux solutions les plus simples à ce niveau sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="6d355-128">The two most straightforward solutions to this are:</span></span>
- <span data-ttu-id="6d355-129">Définition d’une clé primaire de substitution sur une nouvelle propriété indépendante de la clé étrangère qui pointe vers le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="6d355-130">Les valeurs contenues doivent être uniques parmi tous les propriétaires (par exemple, si {1} le parent {1}a un enfant {2} , alors le {1}parent ne peut pas avoir d’enfant), de sorte que la valeur n’a pas de signification inhérente.</span><span class="sxs-lookup"><span data-stu-id="6d355-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="6d355-131">Étant donné que la clé étrangère ne fait pas partie de la clé primaire, ses valeurs peuvent être modifiées. vous pouvez donc déplacer un enfant d’un parent à un autre, mais cela est généralement dû à une sémantique d’agrégation.</span><span class="sxs-lookup"><span data-stu-id="6d355-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="6d355-132">En utilisant la clé étrangère et une propriété supplémentaire comme clé composite.</span><span class="sxs-lookup"><span data-stu-id="6d355-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="6d355-133">La valeur de propriété supplémentaire ne doit désormais être unique que pour un parent donné (par conséquent {1} , si {1,1} le parent {2} a un enfant, {2,1}le parent peut encore avoir un enfant).</span><span class="sxs-lookup"><span data-stu-id="6d355-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="6d355-134">En faisant de la clé étrangère de la clé primaire, la relation entre le propriétaire et l’entité détenue devient immuable et reflète mieux la sémantique d’agrégation.</span><span class="sxs-lookup"><span data-stu-id="6d355-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="6d355-135">C’est ce que EF Core par défaut.</span><span class="sxs-lookup"><span data-stu-id="6d355-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="6d355-136">Dans cet exemple, nous allons utiliser `Distributor` la classe :</span><span class="sxs-lookup"><span data-stu-id="6d355-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="6d355-137">Par défaut, la clé primaire utilisée pour le type détenu référencé via la `ShippingCenters` propriété `("DistributorId", "Id")` de navigation sera where `"DistributorId"` et `"Id"` est une valeur unique `int` .</span><span class="sxs-lookup"><span data-stu-id="6d355-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="6d355-138">Pour configurer un autre appel `HasKey`de PK :</span><span class="sxs-lookup"><span data-stu-id="6d355-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="6d355-139">Avant de EF Core `WithOwner()` méthode 3,0 n’existait pas, cet appel doit être supprimé.</span><span class="sxs-lookup"><span data-stu-id="6d355-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="6d355-140">Mappage de types détenus avec le fractionnement de table</span><span class="sxs-lookup"><span data-stu-id="6d355-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="6d355-141">Lorsque vous utilisez des bases de données relationnelles, par défaut, les types appartenant à la référence sont mappés à la même table que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="6d355-142">Cela nécessite le fractionnement de la table en deux : certaines colonnes seront utilisées pour stocker les données du propriétaire, et certaines colonnes seront utilisées pour stocker les données de l’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="6d355-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="6d355-143">Il s’agit d’une fonctionnalité courante connue sous le nom de [fractionnement de table](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="6d355-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="6d355-144">Par défaut, EF Core nommera les colonnes de base de données pour les propriétés du type d’entité détenu, en suivant le modèle _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="6d355-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="6d355-145">Par conséquent `StreetAddress` , les propriétés s’affichent dans la table Orders avec les noms « ShippingAddress_Street » et « ShippingAddress_City ».</span><span class="sxs-lookup"><span data-stu-id="6d355-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="6d355-146">Vous pouvez utiliser la `HasColumnName` méthode pour renommer ces colonnes :</span><span class="sxs-lookup"><span data-stu-id="6d355-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="6d355-147">Partage du même type .NET entre plusieurs types détenus</span><span class="sxs-lookup"><span data-stu-id="6d355-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="6d355-148">Un type d’entité détenu peut être du même type .NET qu’un autre type d’entité détenu. par conséquent, le type .NET peut ne pas être suffisant pour identifier un type détenu.</span><span class="sxs-lookup"><span data-stu-id="6d355-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="6d355-149">Dans ce cas, la propriété qui pointe du propriétaire vers l’entité détenue devient la définition de la _navigation_ du type d’entité détenu.</span><span class="sxs-lookup"><span data-stu-id="6d355-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="6d355-150">Du point de vue de EF Core, la navigation de définition fait partie de l’identité du type en même temps que le type .NET.</span><span class="sxs-lookup"><span data-stu-id="6d355-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="6d355-151">Par exemple, dans la classe `ShippingAddress` suivante et `BillingAddress` sont tous deux du même type `StreetAddress`.net :</span><span class="sxs-lookup"><span data-stu-id="6d355-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="6d355-152">Pour comprendre comment EF Core doit distinguer les instances suivies de ces objets, il peut être utile de penser que la navigation de définition est devenue partie intégrante de la clé de l’instance parallèlement à la valeur de la clé du propriétaire et du type .NET du type détenu.</span><span class="sxs-lookup"><span data-stu-id="6d355-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="6d355-153">Types détenus imbriqués</span><span class="sxs-lookup"><span data-stu-id="6d355-153">Nested owned types</span></span>

<span data-ttu-id="6d355-154">Dans cet exemple `OrderDetails` , `BillingAddress` est `ShippingAddress`propriétaire de et, `StreetAddress` qui sont tous deux des types.</span><span class="sxs-lookup"><span data-stu-id="6d355-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="6d355-155">`OrderDetails` est alors détenu par le type `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="6d355-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="6d355-156">En plus des types détenus imbriqués, un type détenu peut faire référence à une entité normale, il peut s’agir du propriétaire ou d’une entité différente tant que l’entité qui en est la propriété est sur le côté dépendant.</span><span class="sxs-lookup"><span data-stu-id="6d355-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="6d355-157">Cette fonctionnalité définit les types d’entités détenues, à l’exception des types complexes dans EF6.</span><span class="sxs-lookup"><span data-stu-id="6d355-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="6d355-158">Il est possible de chaîner `OwnsOne` la méthode dans un appel Fluent pour configurer ce modèle :</span><span class="sxs-lookup"><span data-stu-id="6d355-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="6d355-159">Notez l' `WithOwner` appel utilisé pour configurer la propriété de navigation qui pointe vers le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="6d355-160">Il est possible d’obtenir le résultat à `OwnedAttribute` l’aide `OrderDetails` de `StreetAdress`sur et.</span><span class="sxs-lookup"><span data-stu-id="6d355-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="6d355-161">Stockage des types détenus dans des tables distinctes</span><span class="sxs-lookup"><span data-stu-id="6d355-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="6d355-162">Contrairement aux types complexes EF6, les types détenus peuvent être stockés dans une table distincte du propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="6d355-163">Pour remplacer la Convention qui mappe un type détenu à la même table que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table.</span><span class="sxs-lookup"><span data-stu-id="6d355-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="6d355-164">L’exemple suivant mappe `OrderDetails` et ses deux adresses à une table distincte de : `DetailedOrder`</span><span class="sxs-lookup"><span data-stu-id="6d355-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="6d355-165">Interrogation des types détenus</span><span class="sxs-lookup"><span data-stu-id="6d355-165">Querying owned types</span></span>

<span data-ttu-id="6d355-166">Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="6d355-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="6d355-167">Il n’est pas nécessaire d’utiliser `Include` la méthode, même si les types détenus sont stockés dans une table distincte.</span><span class="sxs-lookup"><span data-stu-id="6d355-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="6d355-168">En fonction du modèle décrit précédemment, la requête suivante est obtenue `Order` `OrderDetails` et les deux sont détenues `StreetAddresses` de la base de données :</span><span class="sxs-lookup"><span data-stu-id="6d355-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="6d355-169">Limitations</span><span class="sxs-lookup"><span data-stu-id="6d355-169">Limitations</span></span>

<span data-ttu-id="6d355-170">Certaines de ces limitations sont essentielles à la façon dont les types d’entités détenus fonctionnent, mais d’autres sont des restrictions que nous pouvons être en mesure de supprimer dans les versions ultérieures :</span><span class="sxs-lookup"><span data-stu-id="6d355-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="6d355-171">Restrictions par conception</span><span class="sxs-lookup"><span data-stu-id="6d355-171">By-design restrictions</span></span>
- <span data-ttu-id="6d355-172">Vous ne pouvez pas `DbSet<T>` créer un pour un type détenu</span><span class="sxs-lookup"><span data-stu-id="6d355-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="6d355-173">Vous ne pouvez `Entity<T>()` pas appeler avec un type détenu sur`ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="6d355-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="6d355-174">Lacunes actuelles</span><span class="sxs-lookup"><span data-stu-id="6d355-174">Current shortcomings</span></span>
- <span data-ttu-id="6d355-175">Les hiérarchies d’héritage qui incluent des types d’entités détenus ne sont pas prises en charge</span><span class="sxs-lookup"><span data-stu-id="6d355-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="6d355-176">Les navigations de référence vers les types d’entité détenus ne peuvent pas avoir la valeur null, sauf si elles sont explicitement mappées à une table distincte du propriétaire</span><span class="sxs-lookup"><span data-stu-id="6d355-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="6d355-177">Les instances de types d’entité possédées ne peuvent pas être partagées par plusieurs propriétaires (il s’agit d’un scénario connu pour les objets de valeur qui ne peuvent pas être implémentés à l’aide des types d’entités détenus)</span><span class="sxs-lookup"><span data-stu-id="6d355-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="6d355-178">Lacunes dans les versions précédentes</span><span class="sxs-lookup"><span data-stu-id="6d355-178">Shortcomings in previous versions</span></span>
- <span data-ttu-id="6d355-179">Dans EF Core 2,0, les navigations vers les types d’entités détenues ne peuvent pas être déclarées dans des types d’entité dérivés, à moins que les entités détenues soient explicitement mappées à une table distincte de la hiérarchie de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="6d355-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="6d355-180">Cette limitation a été supprimée dans EF Core 2,1</span><span class="sxs-lookup"><span data-stu-id="6d355-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="6d355-181">Dans EF Core 2,0 et 2,1, seules les navigations de référence vers les types détenus étaient prises en charge.</span><span class="sxs-lookup"><span data-stu-id="6d355-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="6d355-182">Cette limitation a été supprimée dans EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="6d355-182">This limitation has been removed in EF Core 2.2</span></span>
