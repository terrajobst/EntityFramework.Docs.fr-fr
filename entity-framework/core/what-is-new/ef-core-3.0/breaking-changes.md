---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 534ac95cccc03e9797ba766e601e2fe86eaf8061
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319216"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="ebcc0-102">Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="ebcc0-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ebcc0-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ebcc0-104">Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="ebcc0-105">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="ebcc0-106">Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="ebcc0-107">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="ebcc0-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="ebcc0-108">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="ebcc0-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="ebcc0-109">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-110">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-110">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-111">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="ebcc0-112">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="ebcc0-113">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-113">**New behavior**</span></span>

<span data-ttu-id="ebcc0-114">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="ebcc0-115">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="ebcc0-116">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-116">**Why**</span></span>

<span data-ttu-id="ebcc0-117">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="ebcc0-118">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="ebcc0-119">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="ebcc0-120">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="ebcc0-121">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="ebcc0-122">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="ebcc0-123">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-123">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-124">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="ebcc0-125">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebcc0-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="ebcc0-126">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="ebcc0-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="ebcc0-127">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="ebcc0-128">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-128">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-129">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="ebcc0-130">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-130">**New behavior**</span></span>

<span data-ttu-id="ebcc0-131">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="ebcc0-132">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-132">**Why**</span></span>

<span data-ttu-id="ebcc0-133">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="ebcc0-134">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="ebcc0-135">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="ebcc0-136">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="ebcc0-137">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-137">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-138">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="ebcc0-139">L’exécution de requête est enregistrée dans le journal au niveau du débogage</span><span class="sxs-lookup"><span data-stu-id="ebcc0-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="ebcc0-140">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="ebcc0-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="ebcc0-141">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-142">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-142">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-143">Avant EF Core 3.0, l’exécution des requêtes et autres commandes était enregistrée au niveau `Info`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="ebcc0-144">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-144">**New behavior**</span></span>

<span data-ttu-id="ebcc0-145">À compter d’EF Core 3.0, la journalisation de l’exécution des commandes et du SQL s’effectue au niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="ebcc0-146">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-146">**Why**</span></span>

<span data-ttu-id="ebcc0-147">Ce changement a été apporté afin de réduire le bruit au niveau de journal `Info`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="ebcc0-148">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-148">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-149">Cet événement de journalisation est défini par `RelationalEventId.CommandExecuting` avec l’ID d’événement 20100.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="ebcc0-150">Pour reconnecter SQL au niveau `Info`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="ebcc0-151">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="ebcc0-152">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="ebcc0-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="ebcc0-153">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="ebcc0-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="ebcc0-154">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ebcc0-155">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-155">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-156">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="ebcc0-157">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="ebcc0-158">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-158">**New behavior**</span></span>

<span data-ttu-id="ebcc0-159">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="ebcc0-160">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-160">**Why**</span></span>

<span data-ttu-id="ebcc0-161">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="ebcc0-162">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-162">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-163">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="ebcc0-164">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-164">This can be avoided by:</span></span>
* <span data-ttu-id="ebcc0-165">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="ebcc0-166">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="ebcc0-167">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="ebcc0-168">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="ebcc0-169">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="ebcc0-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="ebcc0-170">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="ebcc0-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="ebcc0-171">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-172">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-172">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-173">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="ebcc0-174">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-174">**New behavior**</span></span>

<span data-ttu-id="ebcc0-175">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="ebcc0-176">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="ebcc0-177">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="ebcc0-178">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-178">**Why**</span></span>

<span data-ttu-id="ebcc0-179">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="ebcc0-180">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-180">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-181">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="ebcc0-182">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="ebcc0-183">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="ebcc0-184">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="ebcc0-185">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="ebcc0-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="ebcc0-186">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="ebcc0-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="ebcc0-187">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-188">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-188">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-189">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="ebcc0-190">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-190">**New behavior**</span></span>

<span data-ttu-id="ebcc0-191">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="ebcc0-192">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="ebcc0-193">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-193">**Why**</span></span>

<span data-ttu-id="ebcc0-194">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="ebcc0-195">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-195">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-196">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="ebcc0-197">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="ebcc0-198">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="ebcc0-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="ebcc0-199">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="ebcc0-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="ebcc0-200">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-201">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-201">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-202">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="ebcc0-203">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="ebcc0-204">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-204">**New behavior**</span></span>

<span data-ttu-id="ebcc0-205">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="ebcc0-206">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="ebcc0-207">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-207">**Why**</span></span>

<span data-ttu-id="ebcc0-208">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="ebcc0-209">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="ebcc0-210">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="ebcc0-211">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-211">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-212">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="ebcc0-213">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="ebcc0-214">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="ebcc0-215">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="ebcc0-216">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="ebcc0-217">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="ebcc0-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="ebcc0-218">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="ebcc0-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="ebcc0-219">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-220">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-220">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-221">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="ebcc0-222">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-222">**New behavior**</span></span>

<span data-ttu-id="ebcc0-223">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="ebcc0-224">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="ebcc0-225">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="ebcc0-226">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="ebcc0-227">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-227">For example:</span></span>

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

<span data-ttu-id="ebcc0-228">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="ebcc0-229">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-229">**Why**</span></span>

<span data-ttu-id="ebcc0-230">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="ebcc0-231">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="ebcc0-232">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-232">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-233">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="ebcc0-234">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="ebcc0-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="ebcc0-235">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="ebcc0-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="ebcc0-236">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-237">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-237">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-238">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-238">Consider the following model:</span></span>
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
<span data-ttu-id="ebcc0-239">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="ebcc0-240">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="ebcc0-241">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-241">**New behavior**</span></span>

<span data-ttu-id="ebcc0-242">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="ebcc0-243">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="ebcc0-244">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-244">For example:</span></span>

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

<span data-ttu-id="ebcc0-245">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-245">**Why**</span></span>

<span data-ttu-id="ebcc0-246">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="ebcc0-247">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-247">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-248">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="ebcc0-249">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="ebcc0-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="ebcc0-250">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="ebcc0-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="ebcc0-251">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-252">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-252">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-253">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="ebcc0-254">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-254">**New behavior**</span></span>

<span data-ttu-id="ebcc0-255">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="ebcc0-256">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="ebcc0-257">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-257">**Why**</span></span>

<span data-ttu-id="ebcc0-258">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="ebcc0-259">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-259">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-260">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="ebcc0-261">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="ebcc0-262">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="ebcc0-262">Backing fields are used by default</span></span>

[<span data-ttu-id="ebcc0-263">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="ebcc0-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="ebcc0-264">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ebcc0-265">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-265">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-266">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="ebcc0-267">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="ebcc0-268">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-268">**New behavior**</span></span>

<span data-ttu-id="ebcc0-269">À compter d’EF Core 3.0, si le champ de stockage d’une propriété est connu, cette propriété sera toujours lu et écrite à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="ebcc0-270">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="ebcc0-271">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-271">**Why**</span></span>

<span data-ttu-id="ebcc0-272">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="ebcc0-273">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-273">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-274">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété dans l’API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="ebcc0-275">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="ebcc0-276">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="ebcc0-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="ebcc0-277">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="ebcc0-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="ebcc0-278">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-279">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-279">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-280">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="ebcc0-281">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="ebcc0-282">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-282">**New behavior**</span></span>

<span data-ttu-id="ebcc0-283">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="ebcc0-284">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-284">**Why**</span></span>

<span data-ttu-id="ebcc0-285">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="ebcc0-286">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-286">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-287">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="ebcc0-288">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="ebcc0-289">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ebcc0-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="ebcc0-290">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="ebcc0-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="ebcc0-291">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-292">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-292">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-293">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="ebcc0-294">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-294">**New behavior**</span></span>

<span data-ttu-id="ebcc0-295">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="ebcc0-296">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-296">**Why**</span></span>

<span data-ttu-id="ebcc0-297">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="ebcc0-298">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="ebcc0-299">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-299">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-300">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="ebcc0-301">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="ebcc0-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="ebcc0-302">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="ebcc0-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ebcc0-303">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-304">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-304">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-305">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="ebcc0-306">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="ebcc0-307">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-307">**New behavior**</span></span>

<span data-ttu-id="ebcc0-308">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="ebcc0-309">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="ebcc0-310">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="ebcc0-311">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="ebcc0-312">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-312">**Why**</span></span>

<span data-ttu-id="ebcc0-313">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="ebcc0-314">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-314">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-315">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="ebcc0-316">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="ebcc0-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="ebcc0-317">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="ebcc0-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="ebcc0-318">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-319">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-319">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-320">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="ebcc0-321">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="ebcc0-322">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-322">**New behavior**</span></span>

<span data-ttu-id="ebcc0-323">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="ebcc0-324">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-324">**Why**</span></span>

<span data-ttu-id="ebcc0-325">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="ebcc0-326">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-326">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-327">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="ebcc0-328">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="ebcc0-329">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="ebcc0-330">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="ebcc0-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="ebcc0-331">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="ebcc0-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="ebcc0-332">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-333">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-333">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-334">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="ebcc0-335">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-335">**New behavior**</span></span>

<span data-ttu-id="ebcc0-336">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="ebcc0-337">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-337">**Why**</span></span>

<span data-ttu-id="ebcc0-338">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="ebcc0-339">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-339">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-340">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="ebcc0-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="ebcc0-341">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-341">This isn't common.</span></span>
<span data-ttu-id="ebcc0-342">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="ebcc0-343">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="ebcc0-344">IDbContextOptionsExtensionWithDebugInfo fusionnée dans IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="ebcc0-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="ebcc0-345">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="ebcc0-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="ebcc0-346">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-347">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-347">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-348">`IDbContextOptionsExtensionWithDebugInfo` était une interface facultative supplémentaire étendue à partir de `IDbContextOptionsExtension` pour éviter de créer un changement cassant dans l’interface durant le cycle de mise en production 2.x.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="ebcc0-349">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-349">**New behavior**</span></span>

<span data-ttu-id="ebcc0-350">Les interfaces sont maintenant fusionnées dans `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="ebcc0-351">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-351">**Why**</span></span>

<span data-ttu-id="ebcc0-352">Ce changement a été apporté car les interfaces ne font conceptuellement qu’une.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="ebcc0-353">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-353">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-354">Toute implémentation de `IDbContextOptionsExtension` devra être mise à jour pour prendre en charge le nouveau membre.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="ebcc0-355">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="ebcc0-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="ebcc0-356">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="ebcc0-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="ebcc0-357">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-358">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-358">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-359">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="ebcc0-360">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="ebcc0-361">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="ebcc0-362">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-362">**New behavior**</span></span>

<span data-ttu-id="ebcc0-363">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="ebcc0-364">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="ebcc0-365">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="ebcc0-366">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="ebcc0-367">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-367">**Why**</span></span>

<span data-ttu-id="ebcc0-368">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="ebcc0-369">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-369">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-370">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="ebcc0-371">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="ebcc0-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="ebcc0-372">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="ebcc0-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="ebcc0-373">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-374">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-374">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-375">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="ebcc0-376">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-376">**New behavior**</span></span>

<span data-ttu-id="ebcc0-377">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="ebcc0-378">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-378">**Why**</span></span>

<span data-ttu-id="ebcc0-379">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="ebcc0-380">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-380">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-381">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="ebcc0-382">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="ebcc0-383">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="ebcc0-384">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="ebcc0-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="ebcc0-385">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="ebcc0-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="ebcc0-386">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-387">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-387">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-388">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="ebcc0-389">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="ebcc0-390">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="ebcc0-391">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="ebcc0-392">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-392">**New behavior**</span></span>

<span data-ttu-id="ebcc0-393">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="ebcc0-394">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-394">**Why**</span></span>

<span data-ttu-id="ebcc0-395">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="ebcc0-396">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-396">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-397">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="ebcc0-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="ebcc0-398">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-398">This is not common.</span></span>
<span data-ttu-id="ebcc0-399">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="ebcc0-400">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ebcc0-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="ebcc0-401">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="ebcc0-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="ebcc0-402">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="ebcc0-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="ebcc0-403">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="ebcc0-404">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-404">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-405">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="ebcc0-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="ebcc0-406">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-406">**New behavior**</span></span>

<span data-ttu-id="ebcc0-407">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="ebcc0-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="ebcc0-408">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-408">**Why**</span></span>

<span data-ttu-id="ebcc0-409">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="ebcc0-410">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-410">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-411">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="ebcc0-412">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="ebcc0-413">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="ebcc0-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="ebcc0-414">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="ebcc0-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="ebcc0-415">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-416">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-416">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-417">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="ebcc0-418">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-418">**New behavior**</span></span>

<span data-ttu-id="ebcc0-419">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="ebcc0-420">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-420">**Why**</span></span>

<span data-ttu-id="ebcc0-421">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="ebcc0-422">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="ebcc0-423">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-423">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-424">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="ebcc0-425">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="ebcc0-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="ebcc0-426">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="ebcc0-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="ebcc0-427">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-428">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-428">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-429">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="ebcc0-430">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-430">**New behavior**</span></span>

<span data-ttu-id="ebcc0-431">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="ebcc0-432">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="ebcc0-433">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-433">**Why**</span></span>

<span data-ttu-id="ebcc0-434">Ce changement a été apporté afin de consolider l’API pour les index avec `Includes` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="ebcc0-435">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-435">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-436">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="ebcc0-437">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="ebcc0-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="ebcc0-438">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="ebcc0-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="ebcc0-439">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="ebcc0-440">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-440">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-441">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ebcc0-442">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-442">**New behavior**</span></span>

<span data-ttu-id="ebcc0-443">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="ebcc0-444">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-444">**Why**</span></span>

<span data-ttu-id="ebcc0-445">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="ebcc0-446">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-446">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-447">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="ebcc0-448">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="ebcc0-449">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="ebcc0-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="ebcc0-450">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-450">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-451">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="ebcc0-452">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-452">**New behavior**</span></span>

<span data-ttu-id="ebcc0-453">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="ebcc0-454">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-454">**Why**</span></span>

<span data-ttu-id="ebcc0-455">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="ebcc0-456">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-456">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-457">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="ebcc0-458">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="ebcc0-458">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="ebcc0-459">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="ebcc0-459">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="ebcc0-460">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-461">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-461">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-462">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-462">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="ebcc0-463">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-463">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="ebcc0-464">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-464">**New behavior**</span></span>

<span data-ttu-id="ebcc0-465">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-465">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="ebcc0-466">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-466">**Why**</span></span>

<span data-ttu-id="ebcc0-467">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-467">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="ebcc0-468">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-468">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-469">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="ebcc0-470">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="ebcc0-471">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-471">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="ebcc0-472">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="ebcc0-472">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="ebcc0-473">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="ebcc0-473">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="ebcc0-474">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="ebcc0-475">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-475">**Old behavior**</span></span>

<span data-ttu-id="ebcc0-476">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-476">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="ebcc0-477">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-477">**New behavior**</span></span>

<span data-ttu-id="ebcc0-478">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-478">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="ebcc0-479">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-479">**Why**</span></span>

<span data-ttu-id="ebcc0-480">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-480">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="ebcc0-481">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-481">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="ebcc0-482">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="ebcc0-482">**Mitigations**</span></span>

<span data-ttu-id="ebcc0-483">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="ebcc0-483">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="ebcc0-484">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-484">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="ebcc0-485">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-485">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="ebcc0-486">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="ebcc0-486">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```
