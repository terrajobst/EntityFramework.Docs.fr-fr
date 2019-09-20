---
title: Nouvelles fonctionnalités d’EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d938f17daecd5031147951d0018602c5635de41d
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149098"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="e5dc1-102">Nouvelles fonctionnalités incluses dans EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="e5dc1-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="e5dc1-103">La liste suivante présente les nouvelles fonctionnalités majeures planifiées pour EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="e5dc1-104">EF Core 3,0 est une version majeure et contient également de nombreuses [modifications avec rupture](xref:core/what-is-new/ef-core-3.0/breaking-changes), qui sont des améliorations d’API qui peuvent avoir un impact négatif sur les applications existantes.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="e5dc1-105">Améliorations LINQ</span><span class="sxs-lookup"><span data-stu-id="e5dc1-105">LINQ improvements</span></span> 

<span data-ttu-id="e5dc1-106">LINQ vous permet d’écrire des requêtes de base de données sans quitter le langage de votre choix, en tirant parti des informations de type enrichi pour offrir IntelliSense et la vérification des types au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="e5dc1-107">Toutefois, LINQ vous permet également d’écrire un nombre illimité de requêtes complexes contenant des expressions arbitraires (appels de méthode ou opérations).</span><span class="sxs-lookup"><span data-stu-id="e5dc1-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="e5dc1-108">La gestion de toutes ces combinaisons a toujours été un défi majeur pour les fournisseurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="e5dc1-109">Dans EF Core 3,0, nous avons réécrit notre implémentation LINQ pour permettre la traduction de plus d’expressions en SQL, afin de générer des requêtes efficaces dans plus de cas, afin d’éviter que des requêtes inefficaces ne soient détectées et de faciliter progressivement l’introduction de nouvelles requêtes. les fonctionnalités et les performances improvementswithout la rupture des applications et des fournisseurs de données existants.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="e5dc1-110">Évaluation sur le client</span><span class="sxs-lookup"><span data-stu-id="e5dc1-110">Client evaluation</span></span>

<span data-ttu-id="e5dc1-111">La principale modification de la conception de EF Core 3,0 a pour fonction de gérer les expressions LINQ qu’il ne peut pas traduire en SQL ou en paramètres :</span><span class="sxs-lookup"><span data-stu-id="e5dc1-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="e5dc1-112">Dans les premières versions, EF Core simplement déterminer les parties d’une requête qui pouvaient être traduites en SQL et exécuté le reste de la requête sur le client.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="e5dc1-113">Ce type d’exécution côté client peut être souhaitable dans certains cas, mais dans de nombreux cas, il peut entraîner des requêtes inefficaces.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="e5dc1-114">Par exemple, si EF Core 2,2 ne peut pas convertir un prédicat dans `Where()` un appel, il a exécuté une instruction SQL sans filtre, lu toutes les lignes de la base de données, puis les a filtrées en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="e5dc1-115">Cela peut être acceptable si la base de données contient un petit nombre de lignes, mais peut entraîner des problèmes de performances significatifs ou même un échec d’application si la base de données contient un grand nombre de lignes.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="e5dc1-116">Dans EF Core 3,0, nous avons restreint l’évaluation du client à uniquement sur la projection de niveau supérieur (le dernier `Select()`appel à).</span><span class="sxs-lookup"><span data-stu-id="e5dc1-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="e5dc1-117">Lorsque EF Core 3,0 détecte des expressions qui ne peuvent pas être traduites ailleurs dans la requête, il lève une exception Runtime.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="e5dc1-118">Prise en charge de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e5dc1-118">Cosmos DB support</span></span> 

<span data-ttu-id="e5dc1-119">Le fournisseur Cosmos DB pour EF Core permet aux développeurs familiarisés avec le modèle de programme EF de cibler facilement Azure Cosmos DB en tant que base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="e5dc1-120">L’objectif est de rendre certains des avantages de Cosmos DB (par exemple la distribution mondiale, la disponibilité « Always On », la scalabilité élastique et la faible latence) encore plus accessibles aux développeurs .NET.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="e5dc1-121">Le fournisseur proposera la plupart des fonctionnalités d’EF Core, comme le suivi automatique des modifications, les conversions de valeurs et LINQ, sur l’API SQL dans Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="e5dc1-122">Prise en charge de C# 8.0</span><span class="sxs-lookup"><span data-stu-id="e5dc1-122">C# 8.0 support</span></span>

<span data-ttu-id="e5dc1-123">EF Core 3,0 tire parti de certaines des nouvelles fonctionnalités de C# 8,0 :</span><span class="sxs-lookup"><span data-stu-id="e5dc1-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="e5dc1-124">Flux asynchrones</span><span class="sxs-lookup"><span data-stu-id="e5dc1-124">Asynchronous streams</span></span>

<span data-ttu-id="e5dc1-125">Les résultats de requête asynchrones sont maintenant exposés à `IAsyncEnumerable<T>` l’aide de la nouvelle interface `await foreach`standard et peuvent être utilisés à l’aide de.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="e5dc1-126">Types références Nullables</span><span class="sxs-lookup"><span data-stu-id="e5dc1-126">Nullable reference types</span></span> 

<span data-ttu-id="e5dc1-127">Lorsque cette nouvelle fonctionnalité est activée dans votre code, EF Core peut être à l’origine de la possibilité de valeur null des propriétés des types référence (de types primitifs comme des propriétés de type chaîne ou de navigation) pour déterminer la possibilité de valeur null des colonnes et des relations dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="e5dc1-128">Interception</span><span class="sxs-lookup"><span data-stu-id="e5dc1-128">Interception</span></span>

<span data-ttu-id="e5dc1-129">La nouvelle API d’interception de EF Core 3,0 permet d’observer et de modifier les résultats des opérations de base de données de bas niveau qui se produisent dans le cadre du fonctionnement normal de EF Core, telles que l’ouverture de connexions, les transactions initating et l’exécution de commandes.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="e5dc1-130">Ingénierie à rebours des vues de base de données</span><span class="sxs-lookup"><span data-stu-id="e5dc1-130">Reverse engineering of database views</span></span>

<span data-ttu-id="e5dc1-131">Les types d’entité sans clés (précédemment appelés [types de requêtes](xref:core/modeling/keyless-entity-types)) représentent des données qui peuvent être lues à partir de la base de données, mais qui ne peuvent pas être mises à jour.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-131">Entity types without keys (previously known as [query types](xref:core/modeling/keyless-entity-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="e5dc1-132">Cette caractéristique permet de mapper les vues de base de données dans la plupart des scénarios, c’est pourquoi nous avons automatisé la création de types d’entité sans clés lors de l’ingénierie inverse des vues de base de données.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="e5dc1-133">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="e5dc1-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="e5dc1-134">À partir d’EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, il est possible d’ajouter un `Order` sans `OrderDetails` et toutes les propriétés `OrderDetails` à l’exception de la clé primaire sont mappées à des colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="e5dc1-135">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="e5dc1-136">EF 6.3 sur .NET Core</span><span class="sxs-lookup"><span data-stu-id="e5dc1-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="e5dc1-137">nous sommes conscients du fait que de nombreuses applications utilisent d’anciennes versions d’EF et qu’une migration vers EF Core dans l’unique objectif de tirer parti de .NET Core représente parfois un effort considérable.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="e5dc1-138">Pour cette raison, nous avons activé la version newewst d’EF 6 pour s’exécuter sur .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="e5dc1-139">Il existe certaines limitations, par exemple :</span><span class="sxs-lookup"><span data-stu-id="e5dc1-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="e5dc1-140">De nouveaux fournisseurs doivent fonctionner sur .NET Core</span><span class="sxs-lookup"><span data-stu-id="e5dc1-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="e5dc1-141">La prise en charge spatiale avec SQL Server ne sera pas activée.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="e5dc1-142">Fonctionnalités reportées</span><span class="sxs-lookup"><span data-stu-id="e5dc1-142">Postponed features</span></span>

<span data-ttu-id="e5dc1-143">Certaines fonctionnalités initialement planifiées pour EF Core 3,0 ont été reportées dans les versions ultérieures :</span><span class="sxs-lookup"><span data-stu-id="e5dc1-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="e5dc1-144">Possibilité de ignorer des parties d’un modèle dans des migrations, suivies comme [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="e5dc1-144">Ability to ingore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="e5dc1-145">Entités de conteneurs de propriétés, suivies comme deux problèmes distincts : [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sur les entités de type partagé et les [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sur la prise en charge du mappage de propriété indexée.</span><span class="sxs-lookup"><span data-stu-id="e5dc1-145">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
