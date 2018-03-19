---
title: "Nouveautés d’EF Core 2.1 - EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a>Nouvelles fonctionnalités d’EF Core 2.1
> [!NOTE]  
> Il s’agit encore d’une préversion.

Outre un nombre important de petites améliorations et plus d’une centaine de correctifs produit, EF Core 2.1 inclut plusieurs nouvelles fonctionnalités :

## <a name="lazy-loading"></a>Chargement différé
EF Core contient désormais les blocs de construction nécessaires pour permettre à quiconque de créer des classes d’entité capables de charger leurs propriétés de navigation à la demande. Nous avons également introduit un nouveau package, Microsoft.EntityFrameworkCore.Proxies, qui tire parti de ces blocs de construction pour produire des classes proxy de chargement différé basées sur des classes d’entité ayant subi des modifications minimales (par exemple, des classes avec des propriétés de navigation virtuelles).

Pour plus d’informations sur cette rubrique, lisez la [section sur le chargement différé](xref:core/querying/related-data#lazy-loading).

## <a name="parameters-in-entity-constructors"></a>Paramètres dans les constructeurs d’entité
L’un des blocs de construction nécessaires pour le chargement différé prend désormais en charge la création d’entités qui acceptent des paramètres dans leurs constructeurs. Vous pouvez utiliser des paramètres pour injecter des valeurs de propriété, des délégués de chargement différé et des services.

Pour plus d’informations sur cette rubrique, lisez la [section sur le constructeur d’entité avec des paramètres](xref:core/modeling/constructors).

## <a name="value-conversions"></a>Conversions de valeurs
Jusqu’à présent, EF Core pouvait uniquement mapper les propriétés de types pris en charge nativement par le fournisseur de base de données sous-jacent. Les valeurs étaient copiées entre les colonnes et les propriétés sans aucune transformation. À compter d’EF Core 2.1, les conversions de valeurs peuvent être appliquées pour transformer les valeurs obtenues à partir de colonnes avant leur application aux propriétés, et vice versa. Nous avons un certain nombre de conversions qui peuvent être appliquées par convention, le cas échéant, ainsi qu’une API de configuration explicite qui permet d’inscrire des conversions personnalisées entre des colonnes et des propriétés. Parmi les applications de cette fonctionnalité, citons les suivantes :

- Stockage des enums sous forme de chaînes
- Mappage d’entiers non signés avec SQL Server
- Chiffrement et déchiffrement automatiques des valeurs de propriété

Pour plus d’informations sur cette rubrique, lisez la [section sur les conversions de valeurs](xref:core/modeling/value-conversions).  

## <a name="linq-groupby-translation"></a>Traduction LINQ GroupBy
Avant la version 2.1, l’opérateur LINQ GroupBy dans EF Core était toujours évalué en mémoire. Il est désormais possible de le traduire en clause SQL GROUP BY dans les scénarios les plus courants.

Cet exemple illustre une requête avec GroupBy utilisée pour calculer différentes fonctions d’agrégation :

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

La traduction SQL correspondante ressemble à ceci :

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Amorçage des données
Avec la nouvelle version, il sera possible de fournir des données initiales pour remplir une base de données. Contrairement à ce qui se passe dans EF6, l’amorçage des données est associé à un type d’entité dans le cadre de la configuration du modèle. Les migrations EF Core peuvent automatiquement calculer les opérations d’insertion, de mise à jour ou de suppression à appliquer au moment de la mise à niveau de la base de données vers une nouvelle version du modèle.

Par exemple, utilisez ceci pour configurer les données d’amorçage d’un Post dans `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

Pour plus d’informations sur cette rubrique, lisez la [section sur l’amorçage des données](xref:core/modeling/data-seeding).  

## <a name="query-types"></a>Types de requêtes
Un modèle EF Core peut désormais inclure des types de requêtes. Contrairement aux types d’entités, les types de requêtes ne sont pas associés à une définition de clé et ne peuvent pas être insérés, supprimés ou mis à jour (autrement dit, ils sont en lecture seule). Toutefois, ils peuvent être retournés directement par des requêtes. Voici quelques scénarios d’usages pour les types de requêtes :

- Mappage à des vues sans clés primaires
- Mappage à des tables sans clés primaires
- Mappage à des requêtes définies dans le modèle
- Utilisation comme type de retour pour les requêtes `FromSql()`

Pour plus d’informations sur cette rubrique, lisez la [section sur les types de requêtes](xref:core/modeling/query-types).

## <a name="include-for-derived-types"></a>Include pour les types dérivés
Vous pouvez désormais spécifier des propriétés de navigation définies uniquement sur des types dérivés lors de l’écriture d’expressions pour la méthode `Include`. Pour la version fortement typée de `Include`, nous prenons en charge l’utilisation d’un cast explicite ou de l’opérateur `as`. Nous prenons également en charge le référencement des noms de propriété de navigation définis sur des types dérivés dans la version chaîne de `Include` :

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Pour plus d’informations sur cette rubrique, lisez la [section sur Include avec des types dérivés](xref:core/querying/related-data#include-on-derived-types).

## <a name="systemtransactions-support"></a>Prise en charge de System.Transactions
Vous pouvez désormais utiliser les fonctionnalités System.Transactions, notamment TransactionScope. Celles-ci fonctionnent sur le .NET Framework et .NET Core avec des fournisseurs de base de données compatibles.

Pour plus d’informations sur cette rubrique, lisez la [section sur System.Transactions](xref:core/saving/transactions#using-systemtransactions).

## <a name="better-column-ordering-in-initial-migration"></a>Meilleur classement des colonnes dans la migration initiale
Après avoir passé en revue les commentaires des clients, nous avons mis à jour les migrations de manière à générer initialement des colonnes pour les tables dans le même ordre que les propriétés déclarées dans les classes. Notez qu’EF Core ne peut pas changer l’ordre si de nouveaux membres sont ajoutés après la création de la table initiale.

## <a name="optimization-of-correlated-subqueries"></a>Optimisation des sous-requêtes corrélées
Nous avons amélioré la traduction des requêtes pour éviter l’exécution de requêtes SQL « N + 1 » dans de nombreux scénarios courants dans lesquels l’utilisation d’une propriété de navigation dans la projection débouche sur la jointure des données de la requête racine avec les données d’une sous-requête corrélée. L’optimisation nécessite la mise en mémoire tampon des résultats à partir de la sous-requête. Nous vous demandons par ailleurs de modifier la requête pour procéder à l’adhésion du nouveau comportement.

Par exemple, la requête suivante est normalement traduite en une seule requête pour Customers, plus N requêtes séparées pour Orders (où « N » est le nombre de clients retournés) :

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

En incluant `ToList()` au bon endroit, vous indiquez que la mise en mémoire tampon est appropriée pour Orders, ce qui déclenche l’optimisation :

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Notez que cette requête est traduite en deux requêtes SQL seulement : une pour Customers et la suivante pour Orders.

## <a name="ownedattribute"></a>OwnedAttribute

Il est désormais possible de configurer des [types d’entités détenus](xref:core/modeling/owned-entities) en annotant simplement le type avec `[Owned]` et en veillant à ce que l’entité propriétaire soit ajoutée au modèle :

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

## <a name="database-provider-compatibility"></a>Compatibilité des fournisseurs de base de données

De par sa conception, EF Core 2.1 est compatible avec les fournisseurs de base de données créés pour EF Core 2.0. Certaines des fonctionnalités décrites ci-dessus, comme les conversions de valeurs, nécessitent un fournisseur mis à jour. En revanche, d’autres fonctionnalités comme le chargement différé sont compatibles avec les fournisseurs existants.

> [!TIP]
> Si vous vous heurtez à des incompatibilités inattendues ou à d’autres problèmes liés aux nouvelles fonctionnalités, ou si vous avez des commentaires à propos de ces nouveautés, utilisez [notre système de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/new) pour nous en faire part.
