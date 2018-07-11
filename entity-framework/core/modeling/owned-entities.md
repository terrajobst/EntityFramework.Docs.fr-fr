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
# <a name="owned-entity-types"></a>Types d’entité détenus

>[!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.0.

EF Core vous permet aux types d’entité de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités. Ils sont appelés _types d’entités détenus_. L’entité qui contient un type d’entité détenu est son _propriétaire_.

## <a name="explicit-configuration"></a>Configuration explicite

La propriété entité types sont jamais incluses par EF Core dans le modèle par convention. Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttribute` (nouveau dans EF Core 2.1) pour configurer le type comme un type détenu.

Dans cet exemple, StreetAddress est un type avec aucune propriété d’identité. Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière. Dans `OnModelCreating`, nous utilisons le `OwnsOne` méthode pour spécifier que la propriété ShippingAddress est une entité à propriétaire du type de commande.

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

Si la propriété ShippingAddress est privée dans le type de commande, vous pouvez utiliser la version de chaîne de la `OwnsOne` méthode :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

Dans cet exemple, nous utilisons le `OwnedAttribute` pour atteindre le même objectif :

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

## <a name="implicit-keys"></a>Clés implicites

Dans EF Core 2.0 et 2.1, seules les propriétés de navigation de référence peuvent pointer vers les types détenus. Collections de types détenus ne sont pas prises en charge. Ces référence détenu types ont toujours une relation un à un avec le propriétaire, par conséquent, ils n’ont pas leurs propres valeurs de clés. Dans l’exemple précédent, le type StreetAddress est inutile de définir une propriété de clé.  

Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [occulter la propriété](xref:core/modeling/shadow-properties) pour le type détenu. La valeur de la clé d’une instance du type détenu sera identique à la valeur de la clé de l’instance de propriétaire.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mappage de types avec fractionnement de table détenu

Lorsque vous utilisez des bases de données relationnelles, par convention types détenue sont mappés à la même table en tant que le propriétaire. Cela nécessite de fractionnement de la table en deux : certaines colonnes seront utilisées pour stocker les données du propriétaire, et certaines colonnes seront utilisées pour stocker les données de l’entité détenue. Il s’agit d’une fonctionnalité commune appelée fractionnement de table.

> [!TIP]
> Détenus types stockés avec le fractionnement de table peuvent être utilisé très semblable aux types complexes comment sont utilisées dans EF6.

Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité détenu suivant le modèle _EntityProperty_OwnedEntityProperty_. Par conséquent, les propriétés StreetAddress apparaîtra dans la table Orders avec les noms ShippingAddress_Street et ShippingAddress_City.

Vous pouvez ajouter la `HasColumnName` méthode pour renommer ces colonnes. Dans le cas où StreetAddress est une propriété publique, les mappages serait

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Partage le même type .NET parmi plusieurs types détenus

Type d’entité détenu peut être du même type .NET en tant qu’un autre type d’entité détenu, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type détenu.

Dans ce cas, la propriété qui pointe du propriétaire à l’entité détenue devient le _définition navigation_ du type d’entité détenu. Du point de vue d’EF Core, la navigation de définition fait partie de l’identité du type en même temps que le type .NET.   

Par exemple, dans la classe suivante, ShippingAddress et BillingAddress sont tous deux du même type .NET, StreetAddress :

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Pour comprendre comment EF Core distinguera suivies des instances de ces objets, il peut être utile de considérer que la navigation définie est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.

## <a name="nested-owned-types"></a>Types détenus imbriqués

Dans cet exemple OrderDetails possède BillingAddress et ShippingAddress, qui sont les deux types de StreetAddress. OrderDetails est détenu par le type de commande.

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

Il est possible de chaîner les `OwnsOne` méthode dans un mappage fluent pour configurer ce modèle :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Il est possible de réaliser la même chose à l’aide `OwnedAttribute` sur OrderDetails et StreetAdress.

En plus des types détenus imbriqués, un type détenu peut référencer une entité ordinaire. Dans l’exemple suivant, le pays est une entité non détenus normale :

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Cette fonctionnalité définit les types d’entité détenus en dehors des types complexes dans EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Stockage des types détenus dans des tables distinctes

Également Contrairement aux types complexes d’EF6, types détenus peuvent être stockées dans une table distincte du propriétaire. Pour substituer la convention qui mappe un type détenu à la même table en tant que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table. L’exemple suivant correspondent OrderDetails et ses deux adresses à une table distincte de commande :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Interrogation des types détenus

Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut. Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockées dans une table distincte. Selon le modèle décrit précédemment, la requête suivante extrait ordre, OrderDetails et les deux StreetAddresses détenus pour toutes les commandes en attente à partir de la base de données :

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitations

Certaines de ces restrictions sont fondamentaux pour comment détenu travail de types d’entité, mais d’autres sont des restrictions que nous pouvons être en mesure de supprimer dans les futures versions :

### <a name="shortcomings-in-previous-versions"></a>Lacunes dans les versions précédentes
- Dans EF Core 2.0, navigations à la propriété types d’entité ne peut pas être déclarés dans les types d’entité dérivés, sauf si les entités sont explicitement mappées à une table distincte de la hiérarchie de propriétaire. Cette limitation a été supprimée dans EF Core 2.1

### <a name="current-shortcomings"></a>Défauts actuels
- Hiérarchies d’héritage qui incluent détenus types d’entité ne sont pas pris en charge.
- Types d’entité détenus ne peut pas être pointés par une propriété de navigation de collection (seule référence navigations sont actuellement gérées)
- Navigations à détenus types d’entité ne peut pas être null, sauf si elles sont explicitement mappées à une table distincte du propriétaire
- Instances de types d’entité détenus ne peut pas être partagées par plusieurs propriétaires (il s’agit d’un scénario bien connu pour les objets de valeur qui ne peut pas être implémenté à l’aide des types d’entité détenus)

### <a name="by-design-restrictions"></a>Restrictions par conception
- Vous ne pouvez pas créer un `DbSet<T>`
- Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`
