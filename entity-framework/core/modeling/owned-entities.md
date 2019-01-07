---
title: Appartenant à des Types d’entité - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: fe7e07b8bd483fb3f9b672ee78ef7541f06a21a4
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058771"
---
# <a name="owned-entity-types"></a>Types d’entité détenus

>[!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.0.

EF Core vous permet aux types d’entité de modèle qui peuvent apparaître uniquement sur les propriétés de navigation d’autres types d’entités. Ils sont appelés _types d’entités détenus_. L’entité qui contient un type d’entité détenu est son _propriétaire_.

## <a name="explicit-configuration"></a>Configuration explicite

La propriété entité types sont jamais incluses par EF Core dans le modèle par convention. Vous pouvez utiliser la `OwnsOne` méthode dans `OnModelCreating` ou annotez le type avec `OwnedAttribute` (nouveau dans EF Core 2.1) pour configurer le type comme un type détenu.

Dans cet exemple, `StreetAddress` est un type avec aucune propriété d’identité. Il est utilisé comme propriété du type Order pour spécifier l’adresse d’expédition d’une commande particulière.

Nous pouvons utiliser la `OwnedAttribute` pour le traiter comme une entité détenue lorsque référencé depuis un autre type d’entité :

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Il est également possible d’utiliser le `OwnsOne` méthode dans `OnModelCreating` pour spécifier que le `ShippingAddress` propriété est une entité appartenant à de la `Order` type d’entité et de configurer des facettes supplémentaires si nécessaire.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Si le `ShippingAddress` propriété est privée dans le `Order` type, vous pouvez utiliser la version de chaîne de la `OwnsOne` méthode :

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Consultez le [exemple complet de projet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pour plus de contexte. 

## <a name="implicit-keys"></a>Clés implicites

Configuré avec des types détenus `OwnsOne` ou découverts grâce à une navigation de référence ont toujours une relation un à un avec le propriétaire, par conséquent, ils n’ont pas leurs propres valeurs de clés que les valeurs de clé étrangères sont uniques. Dans l’exemple précédent, le `StreetAddress` type n’a pas besoin de définir une propriété de clé.  

Pour comprendre comment EF Core effectue le suivi de ces objets, il est utile de considérer qu’une clé primaire est créée comme un [occulter la propriété](xref:core/modeling/shadow-properties) pour le type détenu. La valeur de la clé d’une instance du type détenu sera identique à la valeur de la clé de l’instance de propriétaire.

## <a name="collections-of-owned-types"></a>Collections de types détenus

>[!NOTE]
> Cette fonctionnalité est une nouveauté d’EF Core 2.2.

Pour configurer une collection de types détenus `OwnsMany` doit être utilisé dans `OnModelCreating`. Toutefois la clé primaire ne sera pas configurée par convention, donc il doit être spécifié explicitement. Il est courant d’utiliser une clé complexe pour ces types d’entités incorporant la clé étrangère pour le propriétaire et une propriété unique supplémentaire qui peut également être dans l’état de clichés instantanés :

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Mappage de types avec fractionnement de table détenu

Lorsque vous utilisez relationnelles bases de données, par référence de la convention détenus types sont mappés à la même table que le propriétaire. Cela nécessite de fractionnement de la table en deux : certaines colonnes seront utilisées pour stocker les données du propriétaire, et certaines colonnes seront utilisées pour stocker les données de l’entité détenue. Il s’agit d’une fonctionnalité commune appelée fractionnement de table.

> [!TIP]
> Détenus types stockés avec le fractionnement de table peuvent être utilisé de la même façon pour les types complexes comment sont utilisées dans EF6.

Par convention, EF Core nommera les colonnes de la base de données pour les propriétés du type d’entité détenu suivant le modèle _Navigation_OwnedEntityProperty_. Par conséquent le `StreetAddress` propriétés s’affichent dans la table « Orders » avec les noms « ShippingAddress_Street » et « ShippingAddress_City ».

Vous pouvez ajouter la `HasColumnName` méthode pour renommer ces colonnes :

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Partage le même type .NET parmi plusieurs types détenus

Type d’entité détenu peut être du même type .NET en tant qu’un autre type d’entité détenu, par conséquent, le type .NET peuvent ne pas suffire pour identifier un type détenu.

Dans ce cas, la propriété qui pointe du propriétaire à l’entité détenue devient le _définition navigation_ du type d’entité détenu. Du point de vue d’EF Core, la navigation de définition fait partie de l’identité du type en même temps que le type .NET.   

Par exemple, dans la classe suivante `ShippingAddress` et `BillingAddress` sont tous deux du même type .NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Pour comprendre comment EF Core distinguera suivies des instances de ces objets, il peut être utile de considérer que la navigation définie est devenue partie de la clé de l’instance en même temps que la valeur de la clé du propriétaire et le type .NET du type détenu.

## <a name="nested-owned-types"></a>Types détenus imbriqués

Dans cet exemple `OrderDetails` possède `BillingAddress` et `ShippingAddress`, qui sont toutes deux `StreetAddress` types. `OrderDetails` est alors détenu par le type `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Outre les types détenus imbriqués, un type détenu peut référencer une entité normale, il peut être le propriétaire ou une autre entité tant que l’entité détenus est sur le côté dépendant. Cette fonctionnalité définit les types d’entité détenus en dehors des types complexes dans EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Il est possible de chaîner les `OwnsOne` méthode dans un appel fluent afin de configurer ce modèle :

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Il est également possible de réaliser la même chose à l’aide `OwnedAttribute` sur les deux `OrderDetails` et `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Stockage des types détenus dans des tables distinctes

Également Contrairement aux types complexes d’EF6, types détenus peuvent être stockées dans une table distincte du propriétaire. Pour substituer la convention qui mappe un type détenu à la même table en tant que le propriétaire, vous pouvez simplement appeler `ToTable` et fournir un autre nom de table. L’exemple suivant mappe `OrderDetails` et ses deux adresses dans une table distincte de `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Interrogation des types détenus

Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut. Il n’est pas nécessaire d’utiliser le `Include` (méthode), même si les types détenus sont stockées dans une table distincte. Selon le modèle décrit précédemment, la requête suivante obtiendra `Order`, `OrderDetails` et les deux détenus `StreetAddresses` à partir de la base de données :

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitations

Certaines de ces restrictions sont fondamentaux pour comment détenu travail de types d’entité, mais d’autres sont des restrictions que nous pouvons être en mesure de supprimer dans les futures versions :

### <a name="by-design-restrictions"></a>Restrictions par conception
- Vous ne pouvez pas créer un `DbSet<T>` pour un type détenu
- Vous ne pouvez pas appeler `Entity<T>()` avec un type détenu sur `ModelBuilder`

### <a name="current-shortcomings"></a>Défauts actuels
- Hiérarchies d’héritage qui incluent détenus types d’entité ne sont pas pris en charge.
- Les navigations de référence pour détenus types d’entité ne peut pas être null, sauf si elles sont explicitement mappées à une table distincte du propriétaire
- Instances de types d’entité détenus ne peut pas être partagées par plusieurs propriétaires (il s’agit d’un scénario bien connu pour les objets de valeur qui ne peut pas être implémenté à l’aide des types d’entité détenus)

### <a name="shortcomings-in-previous-versions"></a>Lacunes dans les versions précédentes
- Dans EF Core 2.0, navigations à la propriété types d’entité ne peut pas être déclarés dans les types d’entité dérivés, sauf si les entités sont explicitement mappées à une table distincte de la hiérarchie de propriétaire. Cette limitation a été supprimée dans EF Core 2.1
- Dans EF Core 2.0 et 2.1 seule référence navigations vers les types détenus ont été pris en charge. Cette limitation a été supprimée dans EF Core 2.2
