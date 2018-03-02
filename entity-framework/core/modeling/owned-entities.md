---
title: "Propriétaire de Types d’entités - EF Core"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a>Types d’entités appartenant à

>[!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.0.

EF Core vous permet de types d’entités de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités. Ils sont appelés _détenus des types d’entités_. Est de l’entité qui contient un type d’entité appartenant à son _propriétaire_.

## <a name="explicit-configuration"></a>Configuration explicite

La propriété entité types sont jamais inclus par cœur de EF dans le modèle par convention. Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttrbibute` (nouveau dans EF Core 2.1) pour configurer le type comme un type lui appartenant.

Dans cet exemple, StreetAddress est un type avec aucune propriété d’identité. Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière. Dans `OnModelCreating`, nous utilisons le `OwnsOne` méthode pour spécifier que la propriété ShippingAddress est une entité propriétaire du type de commande.

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

Dans cet exemple, nous utilisons le `OwnedAttribute` pour obtenir le même objectif :

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

Dans EF Core 2.0 et 2.1, seules les propriétés de navigation de référence peuvent pointer vers appartenant à des types. Collections de types détenus ne sont pas pris en charge. La propriété référence de ces types ont toujours le type de relation avec le propriétaire, par conséquent, leurs valeurs de clé n’est nécessaire. Dans l’exemple précédent, le type StreetAddress n’a pas besoin de définir une propriété de clé.  

Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [shadow, propriété](xref:core/modeling/shadow-properties) pour le type de lui appartenant. La valeur de la clé d’une instance du type détenue sera identique à la valeur de la clé de l’instance de propriétaire.      

## <a name="mapping-owned-types-with-table-splitting"></a>Propriété types avec fractionnement de la table de mappage

Lorsque vous utilisez des bases de données relationnelles, par convention détenue types sont mappés à la même table en tant que le propriétaire. Cela nécessite de fractionnement de la table en deux : certaines colonnes permet de stocker les données du propriétaire, et certaines colonnes permet de stocker des données de l’entité détenue. Il s’agit d’une fonctionnalité courante connue en tant que le fractionnement de la table.

> [!TIP]
> Détenus stockés avec le fractionnement de la table de types peuvent être utilisées similaire aux types de complexité dans EF6.

Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité appartenant à suivre le modèle de _EntityProperty_OwnedEntityProperty_. Par conséquent, les propriétés de StreetAddress s’affichent dans la table Orders avec les noms ShippingAddress_Street et ShippingAddress_City.

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

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Partage le même type .NET entre plusieurs types d’enfants

Un type d’entité détenue peut être du même type .NET en tant que type d’entité appartenant à un autre, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type lui appartenant.

Dans ce cas, la propriété pointez auprès du directeur de l’entité appartenant à devient le _définition navigation_ du type d’entité détenue. Du point de vue du noyau de EF, le volet de navigation définition fait partie de l’identité du type en même temps que le type .NET.   

Par exemple, dans la classe suivante, ShippingAddress et BillingAddress sont du même type .NET, StreetAddress :

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Afin de comprendre comment EF Core permet de distinguer les marques d’instances de ces objets, il peut être utile de considérer que la définition de la navigation est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.

## <a name="nested-owned-types"></a>Appartenant à des types imbriqués

Dans cet exemple OrderDetails possède BillingAddress et ShippingAddress, qui sont des types StreetAddress. Puis OrderDetails est détenue par le type de commande.

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

Il est possible de chaîne de la `OwnsOne` méthode dans un mappage fluent pour configurer ce modèle :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Il est possible de réaliser la même chose à l’aide `OwnedAttribute` sur OrderDetails et StreetAdress.

En plus des types imbriqués détenus, un type détenu peut référencer une entité normale. Dans l’exemple suivant, le pays est une entité normale (c'est-à-dire sans propriétaire) :

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Cette fonctionnalité définit les types d’entité détenue indépendamment des types complexes dans EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Le stockage des types détenus dans des tables distinctes

Plus à la différence des types complexes EF6, appartenant à des types peuvent être stockés dans une table distincte du propriétaire. Pour remplacer la convention qui mappe un type appartenant à la même table que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table. L’exemple suivant mappera OrderDetails et ses deux adresses dans une table distincte à partir de la commande :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Interrogation appartenant à des types

Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut. Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockés dans une table distincte. Selon le modèle décrit précédemment, la requête suivante extrait ordre, OrderDetails et le StreeAddresses appartenant à deux pour toutes les commandes en attente à partir de la base de données :

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitations

Voici quelques limitations des types d’entité détenue. Certaines de ces limitations sont fondamentaux pour comment détenue travail de types, mais que d’autres est point-à-temps restrictions que nous souhaiterions à supprimer dans les futures versions :

### <a name="current-shortcomings"></a>Défauts en cours
- L’héritage des types détenus n’est pas pris en charge.
- Appartenant à des types ne peuvent pas être pointés par une propriété de navigation de collection
- Dans la mesure où ils utilisent le fractionnement de table par défaut détenus types présentent également les restrictions suivantes, sauf si explicitement mappées à une autre table :
   - Ils ne peuvent pas être détenus par un type dérivé
   - La propriété de navigation définition ne peut pas être définie avec la valeur null (c'est-à-dire appartenant types sur la même table sont toujours requis)

### <a name="by-design-restrictions"></a>Restrictions de par sa conception
- Vous ne pouvez pas créer un `DbSet<T>`
- Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`
