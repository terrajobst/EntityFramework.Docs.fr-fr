---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: fd593b2832a5a6ffe27cd4493127b5d405f684ba
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914125"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="f41cb-102">Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="f41cb-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f41cb-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="f41cb-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="f41cb-104">Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="f41cb-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="f41cb-105">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="f41cb-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="f41cb-106">Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="f41cb-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="f41cb-107">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="f41cb-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="f41cb-108">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="f41cb-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="f41cb-109">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-110">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-110">Old behavior</span></span>**

<span data-ttu-id="f41cb-111">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="f41cb-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="f41cb-112">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="f41cb-113">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-113">New behavior</span></span>**

<span data-ttu-id="f41cb-114">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="f41cb-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="f41cb-115">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="f41cb-116">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-116">Why</span></span>**

<span data-ttu-id="f41cb-117">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="f41cb-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="f41cb-118">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="f41cb-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="f41cb-119">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="f41cb-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="f41cb-120">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="f41cb-121">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="f41cb-122">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="f41cb-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="f41cb-123">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-123">Mitigations</span></span>**

<span data-ttu-id="f41cb-124">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="f41cb-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="f41cb-125">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f41cb-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="f41cb-126">Annonces de suivi du problème n° 325</span><span class="sxs-lookup"><span data-stu-id="f41cb-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="f41cb-127">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="f41cb-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="f41cb-128">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-128">Old behavior</span></span>**

<span data-ttu-id="f41cb-129">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f41cb-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="f41cb-130">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-130">New behavior</span></span>**

<span data-ttu-id="f41cb-131">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="f41cb-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="f41cb-132">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-132">Why</span></span>**

<span data-ttu-id="f41cb-133">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="f41cb-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="f41cb-134">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="f41cb-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="f41cb-135">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="f41cb-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="f41cb-136">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="f41cb-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="f41cb-137">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-137">Mitigations</span></span>**

<span data-ttu-id="f41cb-138">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="f41cb-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="f41cb-139">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="f41cb-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="f41cb-140">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="f41cb-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="f41cb-141">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-142">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-142">Old behavior</span></span>**

<span data-ttu-id="f41cb-143">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="f41cb-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="f41cb-144">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-144">New behavior</span></span>**

<span data-ttu-id="f41cb-145">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f41cb-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="f41cb-146">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="f41cb-147">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="f41cb-148">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="f41cb-149">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="f41cb-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="f41cb-150">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-150">Why</span></span>**

<span data-ttu-id="f41cb-151">Les surcharges de méthode comme celle-ci favorisent grandement l’appel accidentel de la méthode de chaîne brute au lieu de la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="f41cb-152">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="f41cb-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="f41cb-153">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-153">Mitigations</span></span>**

<span data-ttu-id="f41cb-154">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="f41cb-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="f41cb-155">L’exécution de requête est enregistrée dans le journal au niveau du débogage</span><span class="sxs-lookup"><span data-stu-id="f41cb-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="f41cb-156">Suivi du problème no 14523</span><span class="sxs-lookup"><span data-stu-id="f41cb-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="f41cb-157">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-158">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-158">Old behavior</span></span>**

<span data-ttu-id="f41cb-159">Avant EF Core 3.0, l’exécution des requêtes et autres commandes était enregistrée au niveau `Info`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="f41cb-160">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-160">New behavior</span></span>**

<span data-ttu-id="f41cb-161">À compter d’EF Core 3.0, la journalisation de l’exécution des commandes et du SQL s’effectue au niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="f41cb-162">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-162">Why</span></span>**

<span data-ttu-id="f41cb-163">Ce changement a été apporté afin de réduire le bruit au niveau de journal `Info`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="f41cb-164">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-164">Mitigations</span></span>**

<span data-ttu-id="f41cb-165">Cet événement de journalisation est défini par `RelationalEventId.CommandExecuting` avec l’ID d’événement 20100.</span><span class="sxs-lookup"><span data-stu-id="f41cb-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="f41cb-166">Pour reconnecter SQL au niveau `Info`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="f41cb-167">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="f41cb-168">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="f41cb-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="f41cb-169">Suivi du problème no 12378</span><span class="sxs-lookup"><span data-stu-id="f41cb-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="f41cb-170">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="f41cb-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="f41cb-171">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-171">Old behavior</span></span>**

<span data-ttu-id="f41cb-172">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="f41cb-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="f41cb-173">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="f41cb-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="f41cb-174">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-174">New behavior</span></span>**

<span data-ttu-id="f41cb-175">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="f41cb-176">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-176">Why</span></span>**

<span data-ttu-id="f41cb-177">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="f41cb-178">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-178">Mitigations</span></span>**

<span data-ttu-id="f41cb-179">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="f41cb-180">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="f41cb-180">This can be avoided by:</span></span>
* <span data-ttu-id="f41cb-181">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="f41cb-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="f41cb-182">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="f41cb-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="f41cb-183">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="f41cb-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="f41cb-184">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="f41cb-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="f41cb-185">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="f41cb-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="f41cb-186">Suivi du problème no 14616</span><span class="sxs-lookup"><span data-stu-id="f41cb-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="f41cb-187">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-188">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-188">Old behavior</span></span>**

<span data-ttu-id="f41cb-189">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="f41cb-190">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-190">New behavior</span></span>**

<span data-ttu-id="f41cb-191">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="f41cb-192">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="f41cb-193">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="f41cb-194">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-194">Why</span></span>**

<span data-ttu-id="f41cb-195">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="f41cb-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="f41cb-196">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-196">Mitigations</span></span>**

<span data-ttu-id="f41cb-197">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="f41cb-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="f41cb-198">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="f41cb-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="f41cb-199">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="f41cb-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="f41cb-200">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="f41cb-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="f41cb-201">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="f41cb-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="f41cb-202">Suivi du problème no 10114</span><span class="sxs-lookup"><span data-stu-id="f41cb-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="f41cb-203">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-204">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-204">Old behavior</span></span>**

<span data-ttu-id="f41cb-205">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="f41cb-206">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-206">New behavior</span></span>**

<span data-ttu-id="f41cb-207">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="f41cb-208">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="f41cb-209">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-209">Why</span></span>**

<span data-ttu-id="f41cb-210">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="f41cb-211">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-211">Mitigations</span></span>**

<span data-ttu-id="f41cb-212">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="f41cb-213">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="f41cb-214">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="f41cb-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="f41cb-215">Suivi du problème no 14194</span><span class="sxs-lookup"><span data-stu-id="f41cb-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="f41cb-216">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-217">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-217">Old behavior</span></span>**

<span data-ttu-id="f41cb-218">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="f41cb-219">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="f41cb-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="f41cb-220">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-220">New behavior</span></span>**

<span data-ttu-id="f41cb-221">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="f41cb-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="f41cb-222">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="f41cb-223">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-223">Why</span></span>**

<span data-ttu-id="f41cb-224">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="f41cb-225">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="f41cb-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="f41cb-226">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="f41cb-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="f41cb-227">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-227">Mitigations</span></span>**

<span data-ttu-id="f41cb-228">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="f41cb-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="f41cb-229">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="f41cb-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="f41cb-230">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="f41cb-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="f41cb-231">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="f41cb-232">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="f41cb-233">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="f41cb-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="f41cb-234">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="f41cb-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="f41cb-235">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-236">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-236">Old behavior</span></span>**

<span data-ttu-id="f41cb-237">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="f41cb-238">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-238">New behavior</span></span>**

<span data-ttu-id="f41cb-239">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="f41cb-240">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="f41cb-241">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="f41cb-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="f41cb-242">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="f41cb-243">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-243">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="f41cb-244">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="f41cb-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="f41cb-245">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-245">Why</span></span>**

<span data-ttu-id="f41cb-246">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="f41cb-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="f41cb-247">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="f41cb-248">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-248">Mitigations</span></span>**

<span data-ttu-id="f41cb-249">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f41cb-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="f41cb-250">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="f41cb-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="f41cb-251">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="f41cb-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="f41cb-252">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-253">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-253">Old behavior</span></span>**

<span data-ttu-id="f41cb-254">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="f41cb-254">Consider the following model:</span></span>
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
<span data-ttu-id="f41cb-255">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="f41cb-256">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-256">New behavior</span></span>**

<span data-ttu-id="f41cb-257">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="f41cb-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="f41cb-258">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="f41cb-259">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-259">Mitigations</span></span>**

<span data-ttu-id="f41cb-260">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="f41cb-261">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="f41cb-262">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="f41cb-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="f41cb-263">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="f41cb-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="f41cb-264">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-265">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-265">Old behavior</span></span>**

<span data-ttu-id="f41cb-266">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="f41cb-266">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="f41cb-267">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="f41cb-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="f41cb-268">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-268">New behavior</span></span>**

<span data-ttu-id="f41cb-269">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="f41cb-270">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="f41cb-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="f41cb-271">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-271">Why</span></span>**

<span data-ttu-id="f41cb-272">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f41cb-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="f41cb-273">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-273">Mitigations</span></span>**

<span data-ttu-id="f41cb-274">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="f41cb-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="f41cb-275">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="f41cb-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="f41cb-276">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="f41cb-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="f41cb-277">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="f41cb-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="f41cb-278">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-279">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-279">Old behavior</span></span>**

<span data-ttu-id="f41cb-280">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="f41cb-280">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="f41cb-281">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="f41cb-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="f41cb-282">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-282">New behavior</span></span>**

<span data-ttu-id="f41cb-283">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="f41cb-284">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-284">Why</span></span>**

<span data-ttu-id="f41cb-285">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="f41cb-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="f41cb-286">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-286">Mitigations</span></span>**

<span data-ttu-id="f41cb-287">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="f41cb-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="f41cb-288">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="f41cb-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="f41cb-289">Suivi du problème no 13274</span><span class="sxs-lookup"><span data-stu-id="f41cb-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="f41cb-290">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-291">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-291">Old behavior</span></span>**

<span data-ttu-id="f41cb-292">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="f41cb-292">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="f41cb-293">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="f41cb-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="f41cb-294">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="f41cb-295">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-295">New behavior</span></span>**

<span data-ttu-id="f41cb-296">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="f41cb-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="f41cb-297">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="f41cb-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="f41cb-298">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-298">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**<span data-ttu-id="f41cb-299">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-299">Why</span></span>**

<span data-ttu-id="f41cb-300">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="f41cb-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="f41cb-301">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-301">Mitigations</span></span>**

<span data-ttu-id="f41cb-302">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="f41cb-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="f41cb-303">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f41cb-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="f41cb-304">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="f41cb-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="f41cb-305">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-306">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-306">Old behavior</span></span>**

<span data-ttu-id="f41cb-307">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="f41cb-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**<span data-ttu-id="f41cb-308">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-308">New behavior</span></span>**

<span data-ttu-id="f41cb-309">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="f41cb-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="f41cb-310">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-310">Why</span></span>**

<span data-ttu-id="f41cb-311">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="f41cb-312">Le nouveau comportement correspond aussi à EF6.</span><span class="sxs-lookup"><span data-stu-id="f41cb-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="f41cb-313">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-313">Mitigations</span></span>**

<span data-ttu-id="f41cb-314">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="f41cb-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="f41cb-315">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="f41cb-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="f41cb-316">Suivi du problème no 6872</span><span class="sxs-lookup"><span data-stu-id="f41cb-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="f41cb-317">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-318">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-318">Old behavior</span></span>**

<span data-ttu-id="f41cb-319">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f41cb-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="f41cb-320">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-320">New behavior</span></span>**

<span data-ttu-id="f41cb-321">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f41cb-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="f41cb-322">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="f41cb-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="f41cb-323">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-323">Why</span></span>**

<span data-ttu-id="f41cb-324">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f41cb-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="f41cb-325">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-325">Mitigations</span></span>**

<span data-ttu-id="f41cb-326">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="f41cb-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="f41cb-327">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="f41cb-328">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="f41cb-328">Backing fields are used by default</span></span>

[<span data-ttu-id="f41cb-329">Suivi du problème no 12430</span><span class="sxs-lookup"><span data-stu-id="f41cb-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="f41cb-330">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="f41cb-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="f41cb-331">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-331">Old behavior</span></span>**

<span data-ttu-id="f41cb-332">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="f41cb-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="f41cb-333">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="f41cb-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="f41cb-334">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-334">New behavior</span></span>**

<span data-ttu-id="f41cb-335">À compter d’EF Core 3.0, si le champ de stockage d’une propriété est connu, cette propriété sera toujours lu et écrite à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="f41cb-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="f41cb-336">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="f41cb-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="f41cb-337">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-337">Why</span></span>**

<span data-ttu-id="f41cb-338">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="f41cb-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="f41cb-339">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-339">Mitigations</span></span>**

<span data-ttu-id="f41cb-340">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété dans l’API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="f41cb-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="f41cb-341">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="f41cb-342">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="f41cb-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="f41cb-343">Suivi du problème no 12523</span><span class="sxs-lookup"><span data-stu-id="f41cb-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="f41cb-344">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-345">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-345">Old behavior</span></span>**

<span data-ttu-id="f41cb-346">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="f41cb-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="f41cb-347">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="f41cb-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="f41cb-348">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-348">New behavior</span></span>**

<span data-ttu-id="f41cb-349">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="f41cb-350">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-350">Why</span></span>**

<span data-ttu-id="f41cb-351">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="f41cb-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="f41cb-352">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-352">Mitigations</span></span>**

<span data-ttu-id="f41cb-353">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="f41cb-354">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="f41cb-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="f41cb-355">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="f41cb-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="f41cb-356">Suivi du problème no 14756</span><span class="sxs-lookup"><span data-stu-id="f41cb-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="f41cb-357">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-358">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-358">Old behavior</span></span>**

<span data-ttu-id="f41cb-359">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f41cb-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="f41cb-360">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-360">New behavior</span></span>**

<span data-ttu-id="f41cb-361">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f41cb-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="f41cb-362">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-362">Why</span></span>**

<span data-ttu-id="f41cb-363">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="f41cb-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="f41cb-364">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="f41cb-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="f41cb-365">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-365">Mitigations</span></span>**

<span data-ttu-id="f41cb-366">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f41cb-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="f41cb-367">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="f41cb-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="f41cb-368">Suivi du problème no 13552</span><span class="sxs-lookup"><span data-stu-id="f41cb-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="f41cb-369">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-370">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-370">Old behavior</span></span>**

<span data-ttu-id="f41cb-371">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="f41cb-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="f41cb-372">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="f41cb-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="f41cb-373">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-373">New behavior</span></span>**

<span data-ttu-id="f41cb-374">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="f41cb-375">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="f41cb-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="f41cb-376">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="f41cb-377">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="f41cb-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="f41cb-378">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-378">Why</span></span>**

<span data-ttu-id="f41cb-379">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="f41cb-380">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-380">Mitigations</span></span>**

<span data-ttu-id="f41cb-381">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="f41cb-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="f41cb-382">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="f41cb-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="f41cb-383">Suivi du problème no 14617</span><span class="sxs-lookup"><span data-stu-id="f41cb-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="f41cb-384">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-385">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-385">Old behavior</span></span>**

<span data-ttu-id="f41cb-386">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="f41cb-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="f41cb-387">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="f41cb-388">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-388">New behavior</span></span>**

<span data-ttu-id="f41cb-389">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="f41cb-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="f41cb-390">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-390">Why</span></span>**

<span data-ttu-id="f41cb-391">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="f41cb-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="f41cb-392">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-392">Mitigations</span></span>**

<span data-ttu-id="f41cb-393">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="f41cb-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="f41cb-394">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="f41cb-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="f41cb-395">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="f41cb-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="f41cb-396">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="f41cb-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="f41cb-397">Suivi du problème no 14698</span><span class="sxs-lookup"><span data-stu-id="f41cb-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="f41cb-398">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-399">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-399">Old behavior</span></span>**

<span data-ttu-id="f41cb-400">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="f41cb-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="f41cb-401">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-401">New behavior</span></span>**

<span data-ttu-id="f41cb-402">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="f41cb-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="f41cb-403">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-403">Why</span></span>**

<span data-ttu-id="f41cb-404">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="f41cb-405">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-405">Mitigations</span></span>**

<span data-ttu-id="f41cb-406">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="f41cb-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="f41cb-407">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="f41cb-407">This isn't common.</span></span>
<span data-ttu-id="f41cb-408">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="f41cb-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="f41cb-409">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="f41cb-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="f41cb-410">IDbContextOptionsExtensionWithDebugInfo fusionnée dans IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="f41cb-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="f41cb-411">Suivi du problème no 13552</span><span class="sxs-lookup"><span data-stu-id="f41cb-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="f41cb-412">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-413">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="f41cb-414">était une interface facultative supplémentaire étendue à partir de `IDbContextOptionsExtension` pour éviter de créer un changement cassant dans l’interface durant le cycle de mise en production 2.x.</span><span class="sxs-lookup"><span data-stu-id="f41cb-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="f41cb-415">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-415">New behavior</span></span>**

<span data-ttu-id="f41cb-416">Les interfaces sont maintenant fusionnées dans `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="f41cb-417">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-417">Why</span></span>**

<span data-ttu-id="f41cb-418">Ce changement a été apporté car les interfaces ne font conceptuellement qu’une.</span><span class="sxs-lookup"><span data-stu-id="f41cb-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="f41cb-419">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-419">Mitigations</span></span>**

<span data-ttu-id="f41cb-420">Toute implémentation de `IDbContextOptionsExtension` devra être mise à jour pour prendre en charge le nouveau membre.</span><span class="sxs-lookup"><span data-stu-id="f41cb-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="f41cb-421">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="f41cb-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="f41cb-422">Suivi du problème no 12780</span><span class="sxs-lookup"><span data-stu-id="f41cb-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="f41cb-423">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-424">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-424">Old behavior</span></span>**

<span data-ttu-id="f41cb-425">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="f41cb-426">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="f41cb-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="f41cb-427">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="f41cb-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="f41cb-428">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-428">New behavior</span></span>**

<span data-ttu-id="f41cb-429">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f41cb-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="f41cb-430">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="f41cb-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="f41cb-431">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="f41cb-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="f41cb-432">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="f41cb-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="f41cb-433">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-433">Why</span></span>**

<span data-ttu-id="f41cb-434">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="f41cb-435">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-435">Mitigations</span></span>**

<span data-ttu-id="f41cb-436">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="f41cb-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="f41cb-437">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="f41cb-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="f41cb-438">Suivi du problème no 10236</span><span class="sxs-lookup"><span data-stu-id="f41cb-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="f41cb-439">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-440">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-440">Old behavior</span></span>**

<span data-ttu-id="f41cb-441">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="f41cb-442">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-442">New behavior</span></span>**

<span data-ttu-id="f41cb-443">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="f41cb-444">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-444">Why</span></span>**

<span data-ttu-id="f41cb-445">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="f41cb-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="f41cb-446">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-446">Mitigations</span></span>**

<span data-ttu-id="f41cb-447">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="f41cb-448">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="f41cb-449">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="f41cb-450">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="f41cb-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="f41cb-451">Suivi du problème no 9171</span><span class="sxs-lookup"><span data-stu-id="f41cb-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="f41cb-452">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-453">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-453">Old behavior</span></span>**

<span data-ttu-id="f41cb-454">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="f41cb-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="f41cb-455">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="f41cb-456">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="f41cb-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="f41cb-457">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f41cb-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="f41cb-458">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-458">New behavior</span></span>**

<span data-ttu-id="f41cb-459">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="f41cb-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="f41cb-460">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-460">Why</span></span>**

<span data-ttu-id="f41cb-461">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="f41cb-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="f41cb-462">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-462">Mitigations</span></span>**

<span data-ttu-id="f41cb-463">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="f41cb-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="f41cb-464">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="f41cb-464">This is not common.</span></span>
<span data-ttu-id="f41cb-465">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f41cb-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="f41cb-466">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="f41cb-467">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="f41cb-467">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="f41cb-468">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="f41cb-468">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="f41cb-469">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-469">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-470">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-470">Old behavior</span></span>**

<span data-ttu-id="f41cb-471">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="f41cb-471">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` <span data-ttu-id="f41cb-472">(et les classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="f41cb-472">(and deriving classes)</span></span>

**<span data-ttu-id="f41cb-473">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-473">New behavior</span></span>**

<span data-ttu-id="f41cb-474">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="f41cb-474">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

**<span data-ttu-id="f41cb-475">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-475">Why</span></span>**

<span data-ttu-id="f41cb-476">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="f41cb-476">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

**<span data-ttu-id="f41cb-477">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-477">Mitigations</span></span>**

<span data-ttu-id="f41cb-478">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="f41cb-478">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="f41cb-479">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="f41cb-479">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="f41cb-480">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-480">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="f41cb-481">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="f41cb-481">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="f41cb-482">Suivi du problème no 9913</span><span class="sxs-lookup"><span data-stu-id="f41cb-482">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="f41cb-483">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="f41cb-483">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="f41cb-484">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-484">Old behavior</span></span>**

<span data-ttu-id="f41cb-485">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="f41cb-485">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="f41cb-486">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-486">New behavior</span></span>**

<span data-ttu-id="f41cb-487">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="f41cb-487">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="f41cb-488">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-488">Why</span></span>**

<span data-ttu-id="f41cb-489">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="f41cb-489">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="f41cb-490">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-490">Mitigations</span></span>**

<span data-ttu-id="f41cb-491">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="f41cb-491">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="f41cb-492">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="f41cb-492">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="f41cb-493">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="f41cb-493">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="f41cb-494">Suivi du problème no 11811</span><span class="sxs-lookup"><span data-stu-id="f41cb-494">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="f41cb-495">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-495">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-496">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-496">Old behavior</span></span>**

<span data-ttu-id="f41cb-497">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f41cb-497">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="f41cb-498">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-498">New behavior</span></span>**

<span data-ttu-id="f41cb-499">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="f41cb-499">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="f41cb-500">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-500">Why</span></span>**

<span data-ttu-id="f41cb-501">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="f41cb-501">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="f41cb-502">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="f41cb-502">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="f41cb-503">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-503">Mitigations</span></span>**

<span data-ttu-id="f41cb-504">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="f41cb-504">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="f41cb-505">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="f41cb-505">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="f41cb-506">Suivi du problème no 12366</span><span class="sxs-lookup"><span data-stu-id="f41cb-506">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="f41cb-507">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-507">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-508">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-508">Old behavior</span></span>**

<span data-ttu-id="f41cb-509">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-509">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="f41cb-510">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-510">New behavior</span></span>**

<span data-ttu-id="f41cb-511">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="f41cb-511">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="f41cb-512">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-512">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="f41cb-513">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-513">Why</span></span>**

<span data-ttu-id="f41cb-514">Ce changement a été apporté afin de consolider l’API pour les index avec `Includes` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="f41cb-514">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="f41cb-515">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-515">Mitigations</span></span>**

<span data-ttu-id="f41cb-516">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f41cb-516">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="f41cb-517">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="f41cb-517">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="f41cb-518">Suivi du problème no 12151</span><span class="sxs-lookup"><span data-stu-id="f41cb-518">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="f41cb-519">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="f41cb-519">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="f41cb-520">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-520">Old behavior</span></span>**

<span data-ttu-id="f41cb-521">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="f41cb-521">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="f41cb-522">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-522">New behavior</span></span>**

<span data-ttu-id="f41cb-523">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="f41cb-523">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="f41cb-524">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-524">Why</span></span>**

<span data-ttu-id="f41cb-525">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="f41cb-525">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="f41cb-526">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-526">Mitigations</span></span>**

<span data-ttu-id="f41cb-527">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="f41cb-527">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="f41cb-528">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="f41cb-528">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="f41cb-529">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="f41cb-529">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="f41cb-530">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-530">Old behavior</span></span>**

<span data-ttu-id="f41cb-531">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-531">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="f41cb-532">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-532">New behavior</span></span>**

<span data-ttu-id="f41cb-533">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-533">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="f41cb-534">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-534">Why</span></span>**

<span data-ttu-id="f41cb-535">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-535">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="f41cb-536">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-536">Mitigations</span></span>**

<span data-ttu-id="f41cb-537">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-537">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f41cb-538">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="f41cb-538">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f41cb-539">Suivi du problème #15078</span><span class="sxs-lookup"><span data-stu-id="f41cb-539">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="f41cb-540">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-541">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-541">Old behavior</span></span>**

<span data-ttu-id="f41cb-542">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="f41cb-542">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="f41cb-543">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-543">New behavior</span></span>**

<span data-ttu-id="f41cb-544">Maintenant, les valeurs GUID sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="f41cb-544">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="f41cb-545">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-545">Why</span></span>**

<span data-ttu-id="f41cb-546">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="f41cb-546">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="f41cb-547">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="f41cb-547">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="f41cb-548">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-548">Mitigations</span></span>**

<span data-ttu-id="f41cb-549">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="f41cb-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="f41cb-550">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="f41cb-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="f41cb-551">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="f41cb-551">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f41cb-552">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="f41cb-552">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f41cb-553">Suivi du problème no 15020</span><span class="sxs-lookup"><span data-stu-id="f41cb-553">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="f41cb-554">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-555">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-555">Old behavior</span></span>**

<span data-ttu-id="f41cb-556">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="f41cb-556">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="f41cb-557">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="f41cb-557">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="f41cb-558">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-558">New behavior</span></span>**

<span data-ttu-id="f41cb-559">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="f41cb-559">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="f41cb-560">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-560">Why</span></span>**

<span data-ttu-id="f41cb-561">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="f41cb-561">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="f41cb-562">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-562">Mitigations</span></span>**

<span data-ttu-id="f41cb-563">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="f41cb-563">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="f41cb-564">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="f41cb-564">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="f41cb-565">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="f41cb-565">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="f41cb-566">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="f41cb-566">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="f41cb-567">Suivi du problème no 12978</span><span class="sxs-lookup"><span data-stu-id="f41cb-567">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="f41cb-568">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-568">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-569">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-569">Old behavior</span></span>**

<span data-ttu-id="f41cb-570">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="f41cb-570">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="f41cb-571">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-571">New behavior</span></span>**

<span data-ttu-id="f41cb-572">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="f41cb-572">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="f41cb-573">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-573">Why</span></span>**

<span data-ttu-id="f41cb-574">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="f41cb-574">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="f41cb-575">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="f41cb-575">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="f41cb-576">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-576">Mitigations</span></span>**

<span data-ttu-id="f41cb-577">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="f41cb-577">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="f41cb-578">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="f41cb-578">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="f41cb-579">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="f41cb-579">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="f41cb-580">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f41cb-580">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="f41cb-581">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="f41cb-581">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="f41cb-582">Suivi du problème #10985</span><span class="sxs-lookup"><span data-stu-id="f41cb-582">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="f41cb-583">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-583">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-584">Modification</span><span class="sxs-lookup"><span data-stu-id="f41cb-584">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="f41cb-585">a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="f41cb-585">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="f41cb-586">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-586">Why</span></span>**

<span data-ttu-id="f41cb-587">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="f41cb-587">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="f41cb-588">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-588">Mitigations</span></span>**

<span data-ttu-id="f41cb-589">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="f41cb-589">Use the new name.</span></span> <span data-ttu-id="f41cb-590">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="f41cb-590">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="f41cb-591">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="f41cb-591">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="f41cb-592">Suivi du problème #10730</span><span class="sxs-lookup"><span data-stu-id="f41cb-592">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="f41cb-593">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="f41cb-593">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="f41cb-594">Ancien comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-594">Old behavior</span></span>**

<span data-ttu-id="f41cb-595">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="f41cb-595">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="f41cb-596">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-596">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="f41cb-597">Nouveau comportement</span><span class="sxs-lookup"><span data-stu-id="f41cb-597">New behavior</span></span>**

<span data-ttu-id="f41cb-598">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="f41cb-598">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="f41cb-599">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f41cb-599">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="f41cb-600">Pourquoi</span><span class="sxs-lookup"><span data-stu-id="f41cb-600">Why</span></span>**

<span data-ttu-id="f41cb-601">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="f41cb-601">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="f41cb-602">Atténuations</span><span class="sxs-lookup"><span data-stu-id="f41cb-602">Mitigations</span></span>**

<span data-ttu-id="f41cb-603">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="f41cb-603">Use the new name.</span></span>
