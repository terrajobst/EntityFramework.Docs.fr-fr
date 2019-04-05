---
title: Nouvelles fonctionnalités d’EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58867955"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="90953-102">Nouvelles fonctionnalités incluses dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="90953-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90953-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="90953-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="90953-104">La liste suivante présente les nouvelles fonctionnalités majeures planifiées pour EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="90953-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="90953-105">La plupart de ces fonctionnalités ne sont pas dans la préversion actuelle, mais seront disponibles à mesure que nous progresserons vers la version RTM.</span><span class="sxs-lookup"><span data-stu-id="90953-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="90953-106">La raison est que nous nous concentrons d’abord sur l’implémentation des [changements cassants](xref:core/what-is-new/ef-core-3.0/breaking-changes) planifiés.</span><span class="sxs-lookup"><span data-stu-id="90953-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="90953-107">La plupart de ces changements cassants sont des améliorations apportées à EF Core.</span><span class="sxs-lookup"><span data-stu-id="90953-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="90953-108">Beaucoup d’autres sont nécessaires pour révéler d’autres améliorations.</span><span class="sxs-lookup"><span data-stu-id="90953-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="90953-109">Pour une liste complète des corrections de bogues et des améliorations en cours de développement, vous pouvez visualiser [cette requête dans notre suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="90953-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="90953-110">Améliorations LINQ</span><span class="sxs-lookup"><span data-stu-id="90953-110">LINQ improvements</span></span> 

[<span data-ttu-id="90953-111">Suivi du problème n° 12795</span><span class="sxs-lookup"><span data-stu-id="90953-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="90953-112">Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.</span><span class="sxs-lookup"><span data-stu-id="90953-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="90953-113">LINQ permet d’écrire des requêtes de base de données sans abandonner son langage de prédilection, en tirant parti de riches informations de type pour bénéficier d’IntelliSense et du contrôle de type au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="90953-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="90953-114">Cependant, LINQ offre également la possibilité d’écrire un nombre illimité de requêtes complexes, ce qui a toujours représenté un défi de taille pour les fournisseurs LINQ.</span><span class="sxs-lookup"><span data-stu-id="90953-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="90953-115">Dans les premières versions d’EF Core, nous avons résolu ce problème d’une part en identifiant les parties traduisibles en SQL d’une requête, et d’autre part en exécutant le reste de la requête en mémoire sur le client.</span><span class="sxs-lookup"><span data-stu-id="90953-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="90953-116">Si l’exécution côté client est souhaitable dans certaines situations, elle peut aboutir dans la majorité des cas à des requêtes inefficaces, non identifiées comme telles avant le déploiement en production de l’application.</span><span class="sxs-lookup"><span data-stu-id="90953-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="90953-117">Dans EF Core 3.0, nous avons l’intention de changer en profondeur le fonctionnement de notre implémentation LINQ et nos modes de test.</span><span class="sxs-lookup"><span data-stu-id="90953-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="90953-118">L’objectif est multiple : la rendre plus robuste (par exemple, pour éviter d’interrompre les requêtes dans les versions de correctif), traduire correctement plus d’expressions en SQL, générer des requêtes efficaces dans davantage de cas et empêcher celles qui ne le sont pas de passer inaperçues.</span><span class="sxs-lookup"><span data-stu-id="90953-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="90953-119">Prise en charge de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="90953-119">Cosmos DB support</span></span> 

[<span data-ttu-id="90953-120">Suivi du problème n° 8443</span><span class="sxs-lookup"><span data-stu-id="90953-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="90953-121">Cette fonctionnalité est dans la préversion actuelle, mais n’est pas encore finalisée.</span><span class="sxs-lookup"><span data-stu-id="90953-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="90953-122">Nous travaillons sur un fournisseur Cosmos DB pour EF Core, pour permettre aux développeurs habitués au modèle de programmation EF de cibler facilement Azure Cosmos DB comme base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="90953-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="90953-123">L’objectif est de rendre certains des avantages de Cosmos DB (par exemple la distribution mondiale, la disponibilité « Always On », la scalabilité élastique et la faible latence) encore plus accessibles aux développeurs .NET.</span><span class="sxs-lookup"><span data-stu-id="90953-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="90953-124">Le fournisseur proposera la plupart des fonctionnalités d’EF Core, comme le suivi automatique des modifications, les conversions de valeurs et LINQ, sur l’API SQL dans Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="90953-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="90953-125">Nous avons commencé ce travail avant EF Core 2.2, et [nous avons publié quelques préversions du fournisseur](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="90953-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="90953-126">Nous prévoyons de continuer à développer le fournisseur en parallèle d’EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="90953-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="90953-127">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="90953-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="90953-128">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="90953-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="90953-129">Cette fonctionnalité sera introduite dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="90953-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="90953-130">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="90953-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="90953-131">À partir d’EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, il est possible d’ajouter un `Order` sans `OrderDetails` et toutes les propriétés `OrderDetails` à l’exception de la clé primaire sont mappées à des colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="90953-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties except the primary key will be mapped to nullable columns.</span></span>
<span data-ttu-id="90953-132">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="90953-132">When querying EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="90953-133">Prise en charge de C# 8.0</span><span class="sxs-lookup"><span data-stu-id="90953-133">C# 8.0 support</span></span>

<span data-ttu-id="90953-134">[Suivi de problème n°12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Suivi de problème n°10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="90953-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="90953-135">Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.</span><span class="sxs-lookup"><span data-stu-id="90953-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="90953-136">Nous voulons que nos clients bénéficient avec EF Core d’une partie des [nouvelles fonctionnalités de C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) comme les flux asynchrones (notamment `await foreach`) et les types référence nullables.</span><span class="sxs-lookup"><span data-stu-id="90953-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="90953-137">Ingénierie à rebours des vues de base de données</span><span class="sxs-lookup"><span data-stu-id="90953-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="90953-138">Suivi du problème no 1679</span><span class="sxs-lookup"><span data-stu-id="90953-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="90953-139">Cette fonctionnalité n’est pas dans la préversion actuelle.</span><span class="sxs-lookup"><span data-stu-id="90953-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="90953-140">Les [types de requêtes](xref:core/modeling/query-types), introduits dans EF Core 2.1 et considérés comme des types d’entités sans clé dans EF Core 3.0, représentent des données qui peuvent être lues à partir de la base de données mais ne peuvent pas être mises à jour.</span><span class="sxs-lookup"><span data-stu-id="90953-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="90953-141">Cette caractéristique fait d’eux un excellent choix pour les vues de base de données dans la plupart des scénarios. Nous envisageons donc d’automatiser la création des types d’entités sans clé lors de l’ingénierie à rebours des vues de bases de données.</span><span class="sxs-lookup"><span data-stu-id="90953-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="90953-142">Entités conteneurs des propriétés</span><span class="sxs-lookup"><span data-stu-id="90953-142">Property bag entities</span></span>

<span data-ttu-id="90953-143">[Suivi de problème n°13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) et [n°9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="90953-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="90953-144">Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.</span><span class="sxs-lookup"><span data-stu-id="90953-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="90953-145">cette fonctionnalité permet de créer des entités qui stockent les données dans des propriétés indexées au lieu de propriétés standard, et d’utiliser des instances de la même classe .NET (potentiellement aussi simple que `Dictionary<string, object>`) pour représenter différents types d’entités dans le même modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="90953-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="90953-146">Cette fonctionnalité est un tremplin pour prendre en charge les relations plusieurs-à-plusieurs sans entité de jointure ([problème no 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)) – l’une des améliorations les plus demandées pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="90953-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="90953-147">EF 6.3 sur .NET Core</span><span class="sxs-lookup"><span data-stu-id="90953-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="90953-148">Suivi du problème EF6 n° 271</span><span class="sxs-lookup"><span data-stu-id="90953-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="90953-149">Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.</span><span class="sxs-lookup"><span data-stu-id="90953-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="90953-150">nous sommes conscients du fait que de nombreuses applications utilisent d’anciennes versions d’EF et qu’une migration vers EF Core dans l’unique objectif de tirer parti de .NET Core représente parfois un effort considérable.</span><span class="sxs-lookup"><span data-stu-id="90953-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="90953-151">C’est pourquoi nous adapterons la prochaine version d’EF 6 de façon à ce qu’elle s’exécute sur .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="90953-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="90953-152">L’objectif est de faciliter le portage d’applications avec aussi peu de modifications que possible.</span><span class="sxs-lookup"><span data-stu-id="90953-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="90953-153">Il va y avoir certaines limitations.</span><span class="sxs-lookup"><span data-stu-id="90953-153">There are going to be some limitations.</span></span> <span data-ttu-id="90953-154">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="90953-154">For example:</span></span>
- <span data-ttu-id="90953-155">Les nouveaux fournisseurs devront fonctionner avec les autres bases de données, en plus de la prise en charge de SQL Server incluse sur .NET Core.</span><span class="sxs-lookup"><span data-stu-id="90953-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="90953-156">La prise en charge spatiale avec SQL Server ne sera pas activée.</span><span class="sxs-lookup"><span data-stu-id="90953-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="90953-157">Notez également qu’il n’y a pas de nouvelles fonctionnalités planifiées pour EF 6 à ce stade.</span><span class="sxs-lookup"><span data-stu-id="90953-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
