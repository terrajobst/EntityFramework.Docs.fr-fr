---
title: Propriétaire de Types d’entités - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="c8bb1-102">Types d’entités appartenant à</span><span class="sxs-lookup"><span data-stu-id="c8bb1-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="c8bb1-103">Cette fonctionnalité est une nouveauté dans EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="c8bb1-104">EF Core vous permet de types d’entités de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="c8bb1-105">Ils sont appelés _détenus des types d’entités_.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-105">These are called _owned entity types_.</span></span> <span data-ttu-id="c8bb1-106">Est de l’entité qui contient un type d’entité appartenant à son _propriétaire_.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="c8bb1-107">Configuration explicite</span><span class="sxs-lookup"><span data-stu-id="c8bb1-107">Explicit configuration</span></span>

<span data-ttu-id="c8bb1-108">La propriété entité types sont jamais inclus par cœur de EF dans le modèle par convention.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="c8bb1-109">Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttribute` (nouveau dans EF Core 2.1) pour configurer le type comme un type lui appartenant.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="c8bb1-110">Dans cet exemple, StreetAddress est un type avec aucune propriété d’identité.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="c8bb1-111">Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="c8bb1-112">Dans `OnModelCreating`, nous utilisons le `OwnsOne` méthode pour spécifier que la propriété ShippingAddress est une entité propriétaire du type de commande.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="c8bb1-113">Si la propriété ShippingAddress est privée dans le type de commande, vous pouvez utiliser la version de chaîne de la `OwnsOne` méthode :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="c8bb1-114">Dans cet exemple, nous utilisons le `OwnedAttribute` pour obtenir le même objectif :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="c8bb1-115">Clés implicites</span><span class="sxs-lookup"><span data-stu-id="c8bb1-115">Implicit keys</span></span>

<span data-ttu-id="c8bb1-116">Dans EF Core 2.0 et 2.1, seules les propriétés de navigation de référence peuvent pointer vers appartenant à des types.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="c8bb1-117">Collections de types détenus ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="c8bb1-118">La propriété référence de ces types ont toujours le type de relation avec le propriétaire, par conséquent, leurs valeurs de clé n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="c8bb1-119">Dans l’exemple précédent, le type StreetAddress n’a pas besoin de définir une propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="c8bb1-120">Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [shadow, propriété](xref:core/modeling/shadow-properties) pour le type de lui appartenant.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="c8bb1-121">La valeur de la clé d’une instance du type détenue sera identique à la valeur de la clé de l’instance de propriétaire.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="c8bb1-122">Propriété types avec fractionnement de la table de mappage</span><span class="sxs-lookup"><span data-stu-id="c8bb1-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="c8bb1-123">Lorsque vous utilisez des bases de données relationnelles, par convention détenue types sont mappés à la même table en tant que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="c8bb1-124">Cela nécessite de fractionnement de la table en deux : certaines colonnes permet de stocker les données du propriétaire, et certaines colonnes permet de stocker des données de l’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="c8bb1-125">Il s’agit d’une fonctionnalité courante connue en tant que le fractionnement de la table.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="c8bb1-126">Détenus stockés avec le fractionnement de la table de types peuvent être utilisées similaire aux types de complexité dans EF6.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="c8bb1-127">Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité appartenant à suivre le modèle de _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="c8bb1-128">Par conséquent, les propriétés de StreetAddress s’affichent dans la table Orders avec les noms ShippingAddress_Street et ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="c8bb1-129">Vous pouvez ajouter la `HasColumnName` méthode pour renommer ces colonnes.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="c8bb1-130">Dans le cas où StreetAddress est une propriété publique, les mappages serait</span><span class="sxs-lookup"><span data-stu-id="c8bb1-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="c8bb1-131">Partage le même type .NET entre plusieurs types d’enfants</span><span class="sxs-lookup"><span data-stu-id="c8bb1-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="c8bb1-132">Un type d’entité détenue peut être du même type .NET en tant que type d’entité appartenant à un autre, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type lui appartenant.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="c8bb1-133">Dans ce cas, la propriété pointez auprès du directeur de l’entité appartenant à devient le _définition navigation_ du type d’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="c8bb1-134">Du point de vue du noyau de EF, le volet de navigation définition fait partie de l’identité du type en même temps que le type .NET.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="c8bb1-135">Par exemple, dans la classe suivante, ShippingAddress et BillingAddress sont du même type .NET, StreetAddress :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="c8bb1-136">Afin de comprendre comment EF Core permet de distinguer les marques d’instances de ces objets, il peut être utile de considérer que la définition de la navigation est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="c8bb1-137">Appartenant à des types imbriqués</span><span class="sxs-lookup"><span data-stu-id="c8bb1-137">Nested owned types</span></span>

<span data-ttu-id="c8bb1-138">Dans cet exemple OrderDetails possède BillingAddress et ShippingAddress, qui sont des types StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="c8bb1-139">Puis OrderDetails est détenue par le type de commande.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-139">Then OrderDetails is owned by the Order type.</span></span>

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="c8bb1-140">Il est possible de chaîne de la `OwnsOne` méthode dans un mappage fluent pour configurer ce modèle :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="c8bb1-141">Il est possible de réaliser la même chose à l’aide `OwnedAttribute` sur OrderDetails et StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="c8bb1-142">En plus des types imbriqués détenus, un type détenu peut référencer une entité normale.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="c8bb1-143">Dans l’exemple suivant, le pays est une entité normale (c'est-à-dire sans propriétaire) :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="c8bb1-144">Cette fonctionnalité définit les types d’entité détenue indépendamment des types complexes dans EF6.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="c8bb1-145">Le stockage des types détenus dans des tables distinctes</span><span class="sxs-lookup"><span data-stu-id="c8bb1-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="c8bb1-146">Plus à la différence des types complexes EF6, appartenant à des types peuvent être stockés dans une table distincte du propriétaire.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="c8bb1-147">Pour remplacer la convention qui mappe un type appartenant à la même table que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="c8bb1-148">L’exemple suivant mappera OrderDetails et ses deux adresses dans une table distincte à partir de la commande :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="c8bb1-149">Interrogation appartenant à des types</span><span class="sxs-lookup"><span data-stu-id="c8bb1-149">Querying owned types</span></span>

<span data-ttu-id="c8bb1-150">Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="c8bb1-151">Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockés dans une table distincte.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="c8bb1-152">Selon le modèle décrit précédemment, la requête suivante extrait ordre, OrderDetails et le StreeAddresses appartenant à deux pour toutes les commandes en attente à partir de la base de données :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="c8bb1-153">Limitations</span><span class="sxs-lookup"><span data-stu-id="c8bb1-153">Limitations</span></span>

<span data-ttu-id="c8bb1-154">Voici quelques limitations des types d’entité détenue.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="c8bb1-155">Certaines de ces limitations sont fondamentaux pour comment détenue travail de types, mais que d’autres est point-à-temps restrictions que nous souhaiterions à supprimer dans les futures versions :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="c8bb1-156">Défauts en cours</span><span class="sxs-lookup"><span data-stu-id="c8bb1-156">Current shortcomings</span></span>
- <span data-ttu-id="c8bb1-157">L’héritage des types détenus n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="c8bb1-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="c8bb1-158">Appartenant à des types ne peuvent pas être pointés par une propriété de navigation de collection</span><span class="sxs-lookup"><span data-stu-id="c8bb1-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="c8bb1-159">Dans la mesure où ils utilisent le fractionnement de table par défaut détenus types présentent également les restrictions suivantes, sauf si explicitement mappées à une autre table :</span><span class="sxs-lookup"><span data-stu-id="c8bb1-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="c8bb1-160">Ils ne peuvent pas être détenus par un type dérivé</span><span class="sxs-lookup"><span data-stu-id="c8bb1-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="c8bb1-161">La propriété de navigation définition ne peut pas être définie avec la valeur null (c'est-à-dire appartenant types sur la même table sont toujours requis)</span><span class="sxs-lookup"><span data-stu-id="c8bb1-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="c8bb1-162">Restrictions de par sa conception</span><span class="sxs-lookup"><span data-stu-id="c8bb1-162">By-design restrictions</span></span>
- <span data-ttu-id="c8bb1-163">Vous ne pouvez pas créer un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="c8bb1-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="c8bb1-164">Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="c8bb1-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
