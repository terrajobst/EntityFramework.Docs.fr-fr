---
title: "Nouveautés d’EF Core 2.1 - EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 1e5e9839bae1e5da4082d90c02d098bb3b2b43bd
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="815a5-102">Nouvelles fonctionnalités d’EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="815a5-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="815a5-103">Il s’agit encore d’une préversion.</span><span class="sxs-lookup"><span data-stu-id="815a5-103">This release is still in preview.</span></span>

<span data-ttu-id="815a5-104">Outre un nombre important de petites améliorations et plus d’une centaine de correctifs produit, EF Core 2.1 inclut plusieurs nouvelles fonctionnalités :</span><span class="sxs-lookup"><span data-stu-id="815a5-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="815a5-105">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="815a5-105">Lazy loading</span></span>
<span data-ttu-id="815a5-106">EF Core contient désormais les blocs de construction nécessaires pour permettre à quiconque de créer des classes d’entité capables de charger leurs propriétés de navigation à la demande.</span><span class="sxs-lookup"><span data-stu-id="815a5-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="815a5-107">Nous avons également introduit un nouveau package, Microsoft.EntityFrameworkCore.Proxies, qui tire parti de ces blocs de construction pour produire des classes proxy de chargement différé basées sur des classes d’entité ayant subi des modifications minimales (par exemple, des classes avec des propriétés de navigation virtuelles).</span><span class="sxs-lookup"><span data-stu-id="815a5-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="815a5-108">Pour plus d’informations sur cette rubrique, lisez la [section sur le chargement différé](xref:core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="815a5-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="815a5-109">Paramètres dans les constructeurs d’entité</span><span class="sxs-lookup"><span data-stu-id="815a5-109">Parameters in entity constructors</span></span>
<span data-ttu-id="815a5-110">L’un des blocs de construction nécessaires pour le chargement différé prend désormais en charge la création d’entités qui acceptent des paramètres dans leurs constructeurs.</span><span class="sxs-lookup"><span data-stu-id="815a5-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="815a5-111">Vous pouvez utiliser des paramètres pour injecter des valeurs de propriété, des délégués de chargement différé et des services.</span><span class="sxs-lookup"><span data-stu-id="815a5-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="815a5-112">Pour plus d’informations sur cette rubrique, lisez la [section sur le constructeur d’entité avec des paramètres](xref:core/modeling/constructors).</span><span class="sxs-lookup"><span data-stu-id="815a5-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="815a5-113">Conversions de valeurs</span><span class="sxs-lookup"><span data-stu-id="815a5-113">Value conversions</span></span>
<span data-ttu-id="815a5-114">Jusqu’à présent, EF Core pouvait uniquement mapper les propriétés de types pris en charge nativement par le fournisseur de base de données sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="815a5-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="815a5-115">Les valeurs étaient copiées entre les colonnes et les propriétés sans aucune transformation.</span><span class="sxs-lookup"><span data-stu-id="815a5-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="815a5-116">À compter d’EF Core 2.1, les conversions de valeurs peuvent être appliquées pour transformer les valeurs obtenues à partir de colonnes avant leur application aux propriétés, et vice versa.</span><span class="sxs-lookup"><span data-stu-id="815a5-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="815a5-117">Nous avons un certain nombre de conversions qui peuvent être appliquées par convention, le cas échéant, ainsi qu’une API de configuration explicite qui permet d’inscrire des conversions personnalisées entre des colonnes et des propriétés.</span><span class="sxs-lookup"><span data-stu-id="815a5-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="815a5-118">Parmi les applications de cette fonctionnalité, citons les suivantes :</span><span class="sxs-lookup"><span data-stu-id="815a5-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="815a5-119">Stockage des enums sous forme de chaînes</span><span class="sxs-lookup"><span data-stu-id="815a5-119">Storing enums as strings</span></span>
- <span data-ttu-id="815a5-120">Mappage d’entiers non signés avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="815a5-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="815a5-121">Chiffrement et déchiffrement automatiques des valeurs de propriété</span><span class="sxs-lookup"><span data-stu-id="815a5-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="815a5-122">Pour plus d’informations sur cette rubrique, lisez la [section sur les conversions de valeurs](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="815a5-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="815a5-123">Traduction LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="815a5-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="815a5-124">Avant la version 2.1, l’opérateur LINQ GroupBy dans EF Core était toujours évalué en mémoire.</span><span class="sxs-lookup"><span data-stu-id="815a5-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="815a5-125">Il est désormais possible de le traduire en clause SQL GROUP BY dans les scénarios les plus courants.</span><span class="sxs-lookup"><span data-stu-id="815a5-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="815a5-126">Cet exemple illustre une requête avec GroupBy utilisée pour calculer différentes fonctions d’agrégation :</span><span class="sxs-lookup"><span data-stu-id="815a5-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="815a5-127">La traduction SQL correspondante ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="815a5-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="815a5-128">Amorçage des données</span><span class="sxs-lookup"><span data-stu-id="815a5-128">Data Seeding</span></span>
<span data-ttu-id="815a5-129">Avec la nouvelle version, il sera possible de fournir des données initiales pour remplir une base de données.</span><span class="sxs-lookup"><span data-stu-id="815a5-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="815a5-130">Contrairement à ce qui se passe dans EF6, l’amorçage des données est associé à un type d’entité dans le cadre de la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="815a5-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="815a5-131">Les migrations EF Core peuvent automatiquement calculer les opérations d’insertion, de mise à jour ou de suppression à appliquer au moment de la mise à niveau de la base de données vers une nouvelle version du modèle.</span><span class="sxs-lookup"><span data-stu-id="815a5-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="815a5-132">Par exemple, utilisez ceci pour configurer les données d’amorçage d’un Post dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="815a5-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="815a5-133">Pour plus d’informations sur cette rubrique, lisez la [section sur l’amorçage des données](xref:core/modeling/data-seeding).</span><span class="sxs-lookup"><span data-stu-id="815a5-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="815a5-134">Types de requêtes</span><span class="sxs-lookup"><span data-stu-id="815a5-134">Query types</span></span>
<span data-ttu-id="815a5-135">Un modèle EF Core peut désormais inclure des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="815a5-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="815a5-136">Contrairement aux types d’entités, les types de requêtes ne sont pas associés à une définition de clé et ne peuvent pas être insérés, supprimés ou mis à jour (autrement dit, ils sont en lecture seule). Toutefois, ils peuvent être retournés directement par des requêtes.</span><span class="sxs-lookup"><span data-stu-id="815a5-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="815a5-137">Voici quelques scénarios d’usages pour les types de requêtes :</span><span class="sxs-lookup"><span data-stu-id="815a5-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="815a5-138">Mappage à des vues sans clés primaires</span><span class="sxs-lookup"><span data-stu-id="815a5-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="815a5-139">Mappage à des tables sans clés primaires</span><span class="sxs-lookup"><span data-stu-id="815a5-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="815a5-140">Mappage à des requêtes définies dans le modèle</span><span class="sxs-lookup"><span data-stu-id="815a5-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="815a5-141">Utilisation comme type de retour pour les requêtes `FromSql()`</span><span class="sxs-lookup"><span data-stu-id="815a5-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="815a5-142">Pour plus d’informations sur cette rubrique, lisez la [section sur les types de requêtes](xref:core/modeling/query-types).</span><span class="sxs-lookup"><span data-stu-id="815a5-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="815a5-143">Include pour les types dérivés</span><span class="sxs-lookup"><span data-stu-id="815a5-143">Include for derived types</span></span>
<span data-ttu-id="815a5-144">Vous pouvez désormais spécifier des propriétés de navigation définies uniquement sur des types dérivés lors de l’écriture d’expressions pour la méthode `Include`.</span><span class="sxs-lookup"><span data-stu-id="815a5-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="815a5-145">Pour la version fortement typée de `Include`, nous prenons en charge l’utilisation d’un cast explicite ou de l’opérateur `as`.</span><span class="sxs-lookup"><span data-stu-id="815a5-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="815a5-146">Nous prenons également en charge le référencement des noms de propriété de navigation définis sur des types dérivés dans la version chaîne de `Include` :</span><span class="sxs-lookup"><span data-stu-id="815a5-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="815a5-147">Pour plus d’informations sur cette rubrique, lisez la [section sur Include avec des types dérivés](xref:core/querying/related-data#include-on-derived-types).</span><span class="sxs-lookup"><span data-stu-id="815a5-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="815a5-148">Prise en charge de System.Transactions</span><span class="sxs-lookup"><span data-stu-id="815a5-148">System.Transactions support</span></span>
<span data-ttu-id="815a5-149">Vous pouvez désormais utiliser les fonctionnalités System.Transactions, notamment TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="815a5-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="815a5-150">Celles-ci fonctionnent sur le .NET Framework et .NET Core avec des fournisseurs de base de données compatibles.</span><span class="sxs-lookup"><span data-stu-id="815a5-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="815a5-151">Pour plus d’informations sur cette rubrique, lisez la [section sur System.Transactions](xref:core/saving/transactions#using-systemtransactions).</span><span class="sxs-lookup"><span data-stu-id="815a5-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="815a5-152">Meilleur classement des colonnes dans la migration initiale</span><span class="sxs-lookup"><span data-stu-id="815a5-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="815a5-153">Après avoir passé en revue les commentaires des clients, nous avons mis à jour les migrations de manière à générer initialement des colonnes pour les tables dans le même ordre que les propriétés déclarées dans les classes.</span><span class="sxs-lookup"><span data-stu-id="815a5-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="815a5-154">Notez qu’EF Core ne peut pas changer l’ordre si de nouveaux membres sont ajoutés après la création de la table initiale.</span><span class="sxs-lookup"><span data-stu-id="815a5-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="815a5-155">Optimisation des sous-requêtes corrélées</span><span class="sxs-lookup"><span data-stu-id="815a5-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="815a5-156">Nous avons amélioré la traduction des requêtes pour éviter l’exécution de requêtes SQL « N + 1 » dans de nombreux scénarios courants dans lesquels l’utilisation d’une propriété de navigation dans la projection débouche sur la jointure des données de la requête racine avec les données d’une sous-requête corrélée.</span><span class="sxs-lookup"><span data-stu-id="815a5-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="815a5-157">L’optimisation nécessite la mise en mémoire tampon des résultats à partir de la sous-requête. Nous vous demandons par ailleurs de modifier la requête pour procéder à l’adhésion du nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="815a5-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="815a5-158">Par exemple, la requête suivante est normalement traduite en une seule requête pour Customers, plus N requêtes séparées pour Orders (où « N » est le nombre de clients retournés) :</span><span class="sxs-lookup"><span data-stu-id="815a5-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="815a5-159">En incluant `ToList()` au bon endroit, vous indiquez que la mise en mémoire tampon est appropriée pour Orders, ce qui déclenche l’optimisation :</span><span class="sxs-lookup"><span data-stu-id="815a5-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="815a5-160">Notez que cette requête est traduite en deux requêtes SQL seulement : une pour Customers et la suivante pour Orders.</span><span class="sxs-lookup"><span data-stu-id="815a5-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

### <a name="ownedattribute"></a><span data-ttu-id="815a5-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="815a5-161">OwnedAttribute</span></span>

<span data-ttu-id="815a5-162">Il est désormais possible de configurer des [types d’entités détenus](xref:core/modeling/owned-entities) en annotant simplement le type avec `[Owned]` et en veillant à ce que l’entité propriétaire soit ajoutée au modèle :</span><span class="sxs-lookup"><span data-stu-id="815a5-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="database-provider-compatibility"></a><span data-ttu-id="815a5-163">Compatibilité des fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="815a5-163">Database provider compatibility</span></span>

<span data-ttu-id="815a5-164">De par sa conception, EF Core 2.1 est compatible avec les fournisseurs de base de données créés pour EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="815a5-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="815a5-165">Certaines des fonctionnalités décrites ci-dessus, comme les conversions de valeurs, nécessitent un fournisseur mis à jour. En revanche, d’autres fonctionnalités comme le chargement différé sont compatibles avec les fournisseurs existants.</span><span class="sxs-lookup"><span data-stu-id="815a5-165">While some of the features described above (e.g. value conversions) require an updated provider others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="815a5-166">Si vous vous heurtez à des incompatibilités inattendues ou à d’autres problèmes liés aux nouvelles fonctionnalités, ou si vous avez des commentaires à propos de ces nouveautés, utilisez [notre système de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/new) pour nous en faire part.</span><span class="sxs-lookup"><span data-stu-id="815a5-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
