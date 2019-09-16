---
title: Nouvelles fonctionnalités d’EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005554"
---
# <a name="new-features-included-in-ef-core-30"></a>Nouvelles fonctionnalités incluses dans EF Core 3,0

La liste suivante présente les nouvelles fonctionnalités majeures planifiées pour EF Core 3.0.

EF Core 3,0 est une version majeure et contient également de nombreuses [modifications avec rupture](xref:core/what-is-new/ef-core-3.0/breaking-changes), qui sont des améliorations d’API qui peuvent avoir un impact négatif sur les applications existantes.  

## <a name="linq-improvements"></a>Améliorations LINQ 

LINQ vous permet d’écrire des requêtes de base de données sans quitter le langage de votre choix, en tirant parti des informations de type enrichi pour offrir IntelliSense et la vérification des types au moment de la compilation.
Toutefois, LINQ vous permet également d’écrire un nombre illimité de requêtes complexes contenant des expressions arbitraires (appels de méthode ou opérations).
La gestion de toutes ces combinaisons a toujours été un défi majeur pour les fournisseurs LINQ.
Dans EF Core 3,0, nous avons réécrit notre implémentation LINQ pour permettre la traduction de plus d’expressions en SQL, afin de générer des requêtes efficaces dans plus de cas, afin d’éviter que des requêtes inefficaces ne soient détectées et de faciliter progressivement l’introduction de nouvelles requêtes. les fonctionnalités et les performances improvementswithout la rupture des applications et des fournisseurs de données existants.

### <a name="client-evaluation"></a>Évaluation sur le client

La principale modification de la conception de EF Core 3,0 a pour fonction de gérer les expressions LINQ qu’il ne peut pas traduire en SQL ou en paramètres :

Dans les premières versions, EF Core simplement déterminer les parties d’une requête qui pouvaient être traduites en SQL et exécuté le reste de la requête sur le client.
Ce type d’exécution côté client peut être souhaitable dans certains cas, mais dans de nombreux cas, il peut entraîner des requêtes inefficaces.
Par exemple, si EF Core 2,2 ne peut pas convertir un prédicat dans `Where()` un appel, il a exécuté une instruction SQL sans filtre, lu toutes les lignes de la base de données, puis les a filtrées en mémoire.
Cela peut être acceptable si la base de données contient un petit nombre de lignes, mais peut entraîner des problèmes de performances significatifs ou même un échec d’application si la base de données contient un grand nombre de lignes.
Dans EF Core 3,0, nous avons restreint l’évaluation du client à uniquement sur la projection de niveau supérieur (le dernier `Select()`appel à).
Lorsque EF Core 3,0 détecte des expressions qui ne peuvent pas être traduites ailleurs dans la requête, il lève une exception Runtime.

## <a name="cosmos-db-support"></a>Prise en charge de Cosmos DB 

Le fournisseur Cosmos DB pour EF Core permet aux développeurs familiarisés avec le modèle de programme EF de cibler facilement Azure Cosmos DB en tant que base de données d’application.
L’objectif est de rendre certains des avantages de Cosmos DB (par exemple la distribution mondiale, la disponibilité « Always On », la scalabilité élastique et la faible latence) encore plus accessibles aux développeurs .NET.
Le fournisseur proposera la plupart des fonctionnalités d’EF Core, comme le suivi automatique des modifications, les conversions de valeurs et LINQ, sur l’API SQL dans Cosmos DB.

## <a name="c-80-support"></a>Prise en charge de C# 8.0

EF Core 3,0 tire parti de certaines des nouvelles fonctionnalités de C# 8,0 :

### <a name="asynchronous-streams"></a>Flux asynchrones

Les résultats de requête asynchrones sont maintenant exposés à `IAsyncEnumerable<T>` l’aide de la nouvelle interface `await foreach`standard et peuvent être utilisés à l’aide de.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>Types références Nullables 

Lorsque cette nouvelle fonctionnalité est activée dans votre code, EF Core peut être à l’origine de la possibilité de valeur null des propriétés des types référence (de types primitifs comme des propriétés de type chaîne ou de navigation) pour déterminer la possibilité de valeur null des colonnes et des relations dans la base de données.

## <a name="interception"></a>Interception

La nouvelle API d’interception de EF Core 3,0 permet d’observer et de modifier les résultats des opérations de base de données de bas niveau qui se produisent dans le cadre du fonctionnement normal de EF Core, telles que l’ouverture de connexions, les transactions initating et l’exécution de commandes. 

## <a name="reverse-engineering-of-database-views"></a>Ingénierie à rebours des vues de base de données

Les types d’entité sans clés (précédemment appelés [types de requêtes](xref:core/modeling/query-types)) représentent des données qui peuvent être lues à partir de la base de données, mais qui ne peuvent pas être mises à jour.
Cette caractéristique permet de mapper les vues de base de données dans la plupart des scénarios, c’est pourquoi nous avons automatisé la création de types d’entité sans clés lors de l’ingénierie inverse des vues de base de données.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives

À partir d’EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, il est possible d’ajouter un `Order` sans `OrderDetails` et toutes les propriétés `OrderDetails` à l’exception de la clé primaire sont mappées à des colonnes de type nullable.

Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 sur .NET Core

nous sommes conscients du fait que de nombreuses applications utilisent d’anciennes versions d’EF et qu’une migration vers EF Core dans l’unique objectif de tirer parti de .NET Core représente parfois un effort considérable.
Pour cette raison, nous avons activé la version newewst d’EF 6 pour s’exécuter sur .NET Core 3,0.
Il existe certaines limitations, par exemple :
- De nouveaux fournisseurs doivent fonctionner sur .NET Core
- La prise en charge spatiale avec SQL Server ne sera pas activée.

## <a name="postponed-features"></a>Fonctionnalités reportées

Certaines fonctionnalités initialement planifiées pour EF Core 3,0 ont été reportées dans les versions ultérieures : 

- Possibilité de ignorer des parties d’un modèle dans des migrations, suivies par [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entités de conteneurs de propriétés, suivies par deux problèmes distincts : [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sur les entités de type partagé et les [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sur la prise en charge du mappage de propriétés indexées.
