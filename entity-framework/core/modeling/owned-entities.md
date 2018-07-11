---
title: Appartenant à des Types d’entité - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 476a1dcaadcd99eba0cd4f5f0ac40c32a97af5c9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949425"
---
# <a name="owned-entity-types"></a><span data-ttu-id="bb141-102">Types d’entité détenus</span><span class="sxs-lookup"><span data-stu-id="bb141-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="bb141-103">Cette fonctionnalité est une nouveauté dans EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bb141-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="bb141-104">EF Core vous permet aux types d’entité de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités.</span><span class="sxs-lookup"><span data-stu-id="bb141-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="bb141-105">Ils sont appelés _types d’entités détenus_.</span><span class="sxs-lookup"><span data-stu-id="bb141-105">These are called _owned entity types_.</span></span> <span data-ttu-id="bb141-106">L’entité qui contient un type d’entité détenu est son _propriétaire_.</span><span class="sxs-lookup"><span data-stu-id="bb141-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="bb141-107">Configuration explicite</span><span class="sxs-lookup"><span data-stu-id="bb141-107">Explicit configuration</span></span>

<span data-ttu-id="bb141-108">La propriété entité types sont jamais incluses par EF Core dans le modèle par convention.</span><span class="sxs-lookup"><span data-stu-id="bb141-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="bb141-109">Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttribute` (nouveau dans EF Core 2.1) pour configurer le type comme un type détenu.</span><span class="sxs-lookup"><span data-stu-id="bb141-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="bb141-110">Dans cet exemple, StreetAddress est un type avec aucune propriété d’identité.</span><span class="sxs-lookup"><span data-stu-id="bb141-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="bb141-111">Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière.</span><span class="sxs-lookup"><span data-stu-id="bb141-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="bb141-112">Dans `OnModelCreating`, nous utilisons le `OwnsOne` méthode pour spécifier que la propriété ShippingAddress est une entité à propriétaire du type de commande.</span><span class="sxs-lookup"><span data-stu-id="bb141-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="bb141-113">Si la propriété ShippingAddress est privée dans le type de commande, vous pouvez utiliser la version de chaîne de la `OwnsOne` méthode :</span><span class="sxs-lookup"><span data-stu-id="bb141-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="bb141-114">Dans cet exemple, nous utilisons le `OwnedAttribute` pour atteindre le même objectif :</span><span class="sxs-lookup"><span data-stu-id="bb141-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="bb141-115">Clés implicites</span><span class="sxs-lookup"><span data-stu-id="bb141-115">Implicit keys</span></span>

<span data-ttu-id="bb141-116">Dans EF Core 2.0 et 2.1, seules les propriétés de navigation de référence peuvent pointer vers les types détenus.</span><span class="sxs-lookup"><span data-stu-id="bb141-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="bb141-117">Collections de types détenus ne sont pas prises en charge.</span><span class="sxs-lookup"><span data-stu-id="bb141-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="bb141-118">Ces référence détenu types ont toujours une relation un à un avec le propriétaire, par conséquent, ils n’ont pas leurs propres valeurs de clés.</span><span class="sxs-lookup"><span data-stu-id="bb141-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="bb141-119">Dans l’exemple précédent, le type StreetAddress est inutile de définir une propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="bb141-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="bb141-120">Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [occulter la propriété](xref:core/modeling/shadow-properties) pour le type détenu.</span><span class="sxs-lookup"><span data-stu-id="bb141-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="bb141-121">La valeur de la clé d’une instance du type détenu sera identique à la valeur de la clé de l’instance de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="bb141-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="bb141-122">Mappage de types avec fractionnement de table détenu</span><span class="sxs-lookup"><span data-stu-id="bb141-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="bb141-123">Lorsque vous utilisez des bases de données relationnelles, par convention types détenue sont mappés à la même table en tant que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="bb141-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="bb141-124">Cela nécessite de fractionnement de la table en deux : certaines colonnes seront utilisées pour stocker les données du propriétaire, et certaines colonnes seront utilisées pour stocker les données de l’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="bb141-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="bb141-125">Il s’agit d’une fonctionnalité commune appelée fractionnement de table.</span><span class="sxs-lookup"><span data-stu-id="bb141-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="bb141-126">Détenus types stockés avec le fractionnement de table peuvent être utilisé très semblable aux types complexes comment sont utilisées dans EF6.</span><span class="sxs-lookup"><span data-stu-id="bb141-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="bb141-127">Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité détenu suivant le modèle _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="bb141-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="bb141-128">Par conséquent, les propriétés StreetAddress apparaîtra dans la table Orders avec les noms ShippingAddress_Street et ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="bb141-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="bb141-129">Vous pouvez ajouter la `HasColumnName` méthode pour renommer ces colonnes.</span><span class="sxs-lookup"><span data-stu-id="bb141-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="bb141-130">Dans le cas où StreetAddress est une propriété publique, les mappages serait</span><span class="sxs-lookup"><span data-stu-id="bb141-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="bb141-131">Partage le même type .NET parmi plusieurs types détenus</span><span class="sxs-lookup"><span data-stu-id="bb141-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="bb141-132">Type d’entité détenu peut être du même type .NET en tant qu’un autre type d’entité détenu, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type détenu.</span><span class="sxs-lookup"><span data-stu-id="bb141-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="bb141-133">Dans ce cas, la propriété qui pointe du propriétaire à l’entité détenue devient le _définition navigation_ du type d’entité détenu.</span><span class="sxs-lookup"><span data-stu-id="bb141-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="bb141-134">Du point de vue d’EF Core, la navigation de définition fait partie de l’identité du type en même temps que le type .NET.</span><span class="sxs-lookup"><span data-stu-id="bb141-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="bb141-135">Par exemple, dans la classe suivante, ShippingAddress et BillingAddress sont tous deux du même type .NET, StreetAddress :</span><span class="sxs-lookup"><span data-stu-id="bb141-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="bb141-136">Pour comprendre comment EF Core distinguera suivies des instances de ces objets, il peut être utile de considérer que la navigation définie est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.</span><span class="sxs-lookup"><span data-stu-id="bb141-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="bb141-137">Types détenus imbriqués</span><span class="sxs-lookup"><span data-stu-id="bb141-137">Nested owned types</span></span>

<span data-ttu-id="bb141-138">Dans cet exemple OrderDetails possède BillingAddress et ShippingAddress, qui sont les deux types de StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="bb141-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="bb141-139">OrderDetails est détenu par le type de commande.</span><span class="sxs-lookup"><span data-stu-id="bb141-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="bb141-140">Il est possible de chaîner les `OwnsOne` méthode dans un mappage fluent pour configurer ce modèle :</span><span class="sxs-lookup"><span data-stu-id="bb141-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="bb141-141">Il est possible de réaliser la même chose à l’aide `OwnedAttribute` sur OrderDetails et StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="bb141-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="bb141-142">En plus des types détenus imbriqués, un type détenu peut référencer une entité ordinaire.</span><span class="sxs-lookup"><span data-stu-id="bb141-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="bb141-143">Dans l’exemple suivant, le pays est une entité non détenus normale :</span><span class="sxs-lookup"><span data-stu-id="bb141-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="bb141-144">Cette fonctionnalité définit les types d’entité détenus en dehors des types complexes dans EF6.</span><span class="sxs-lookup"><span data-stu-id="bb141-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="bb141-145">Stockage des types détenus dans des tables distinctes</span><span class="sxs-lookup"><span data-stu-id="bb141-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="bb141-146">Également Contrairement aux types complexes d’EF6, types détenus peuvent être stockées dans une table distincte du propriétaire.</span><span class="sxs-lookup"><span data-stu-id="bb141-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="bb141-147">Pour substituer la convention qui mappe un type détenu à la même table en tant que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table.</span><span class="sxs-lookup"><span data-stu-id="bb141-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="bb141-148">L’exemple suivant correspondent OrderDetails et ses deux adresses à une table distincte de commande :</span><span class="sxs-lookup"><span data-stu-id="bb141-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="bb141-149">Interrogation des types détenus</span><span class="sxs-lookup"><span data-stu-id="bb141-149">Querying owned types</span></span>

<span data-ttu-id="bb141-150">Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="bb141-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="bb141-151">Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockées dans une table distincte.</span><span class="sxs-lookup"><span data-stu-id="bb141-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="bb141-152">Selon le modèle décrit précédemment, la requête suivante extrait ordre, OrderDetails et les deux StreetAddresses détenus pour toutes les commandes en attente à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="bb141-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="bb141-153">Limitations</span><span class="sxs-lookup"><span data-stu-id="bb141-153">Limitations</span></span>

<span data-ttu-id="bb141-154">Certaines de ces restrictions sont fondamentaux pour comment détenu travail de types d’entité, mais d’autres sont des restrictions que nous pouvons être en mesure de supprimer dans les futures versions :</span><span class="sxs-lookup"><span data-stu-id="bb141-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="bb141-155">Lacunes dans les versions précédentes</span><span class="sxs-lookup"><span data-stu-id="bb141-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="bb141-156">Dans EF Core 2.0, navigations à la propriété types d’entité ne peut pas être déclarés dans les types d’entité dérivés, sauf si les entités sont explicitement mappées à une table distincte de la hiérarchie de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="bb141-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="bb141-157">Cette limitation a été supprimée dans EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="bb141-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="bb141-158">Défauts actuels</span><span class="sxs-lookup"><span data-stu-id="bb141-158">Current shortcomings</span></span>
- <span data-ttu-id="bb141-159">Hiérarchies d’héritage qui incluent détenus types d’entité ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="bb141-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="bb141-160">Types d’entité détenus ne peut pas être pointés par une propriété de navigation de collection (seule référence navigations sont actuellement gérées)</span><span class="sxs-lookup"><span data-stu-id="bb141-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="bb141-161">Navigations à détenus types d’entité ne peut pas être null, sauf si elles sont explicitement mappées à une table distincte du propriétaire</span><span class="sxs-lookup"><span data-stu-id="bb141-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="bb141-162">Instances de types d’entité détenus ne peut pas être partagées par plusieurs propriétaires (il s’agit d’un scénario bien connu pour les objets de valeur qui ne peut pas être implémenté à l’aide des types d’entité détenus)</span><span class="sxs-lookup"><span data-stu-id="bb141-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="bb141-163">Restrictions par conception</span><span class="sxs-lookup"><span data-stu-id="bb141-163">By-design restrictions</span></span>
- <span data-ttu-id="bb141-164">Vous ne pouvez pas créer un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="bb141-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="bb141-165">Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="bb141-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
