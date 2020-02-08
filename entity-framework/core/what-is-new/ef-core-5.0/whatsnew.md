---
title: Nouveautés de EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052031"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="f172b-102">Nouveautés de EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="f172b-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="f172b-103">EF Core 5,0 est actuellement en cours de développement.</span><span class="sxs-lookup"><span data-stu-id="f172b-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="f172b-104">Cette page contient une vue d’ensemble des changements intéressants introduits dans chaque version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="f172b-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="f172b-105">Le premier aperçu de EF Core 5,0 est provisoirement attendu dans le premier trimestre de 2020.</span><span class="sxs-lookup"><span data-stu-id="f172b-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="f172b-106">Cette page ne duplique pas le [plan pour EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="f172b-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="f172b-107">Le plan décrit les thèmes globaux pour EF Core 5,0, y compris tout ce que nous envisageons d’inclure avant d’expédier la version finale.</span><span class="sxs-lookup"><span data-stu-id="f172b-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="f172b-108">Nous ajouterons des liens à partir d’ici à la documentation officielle au fur et à mesure de sa publication.</span><span class="sxs-lookup"><span data-stu-id="f172b-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="f172b-109">Version préliminaire 1 (pas encore expédiée)</span><span class="sxs-lookup"><span data-stu-id="f172b-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="f172b-110">Journalisation simple</span><span class="sxs-lookup"><span data-stu-id="f172b-110">Simple logging</span></span>

<span data-ttu-id="f172b-111">Cette fonctionnalité ajoute des fonctionnalités similaires à celles de `Database.Log` dans EF6.</span><span class="sxs-lookup"><span data-stu-id="f172b-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="f172b-112">Autrement dit, il offre un moyen simple d’obtenir des journaux à partir de EF Core sans avoir besoin de configurer tout type d’infrastructure de journalisation externe.</span><span class="sxs-lookup"><span data-stu-id="f172b-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="f172b-113">La documentation préliminaire est incluse dans l' [État EF hebdomadaire du 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f172b-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f172b-114">Une documentation supplémentaire est suivie par le [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085)du problème.</span><span class="sxs-lookup"><span data-stu-id="f172b-114">Additional documentation is tracked by issue [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="f172b-115">Moyen simple d’utiliser le SQL généré</span><span class="sxs-lookup"><span data-stu-id="f172b-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="f172b-116">EF Core 5,0 introduit la méthode d’extension `ToQueryString` qui retourne le SQL que EF Core générera lors de l’exécution d’une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="f172b-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="f172b-117">La documentation préliminaire est incluse dans l' [État EF Weekly pour le 9 janvier 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="f172b-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="f172b-118">Une documentation supplémentaire est suivie par le [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331)du problème.</span><span class="sxs-lookup"><span data-stu-id="f172b-118">Additional documentation is tracked by issue [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="f172b-119">Vues de débogage améliorées</span><span class="sxs-lookup"><span data-stu-id="f172b-119">Enhanced debug views</span></span>

<span data-ttu-id="f172b-120">Les vues de débogage sont un moyen simple de consulter les éléments internes d’EF Core lors du débogage des problèmes.</span><span class="sxs-lookup"><span data-stu-id="f172b-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="f172b-121">Une vue de débogage a été implémentée pour le modèle il y a quelque temps.</span><span class="sxs-lookup"><span data-stu-id="f172b-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="f172b-122">Pour EF Core 5,0, nous avons facilité la lecture et l’ajout d’une nouvelle vue de débogage pour les entités suivies dans le gestionnaire d’État.</span><span class="sxs-lookup"><span data-stu-id="f172b-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="f172b-123">La documentation préliminaire est incluse dans l' [État EF Weekly pour le 12 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="f172b-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="f172b-124">Une documentation supplémentaire est suivie par le [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086)du problème.</span><span class="sxs-lookup"><span data-stu-id="f172b-124">Additional documentation is tracked by issue [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="f172b-125">La connexion ou la chaîne de connexion peut être modifiée lors de l’DbContext initialisé</span><span class="sxs-lookup"><span data-stu-id="f172b-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="f172b-126">Il est désormais plus facile de créer une instance DbContext sans connexion ou chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="f172b-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="f172b-127">En outre, la connexion ou la chaîne de connexion peut désormais être mutée sur l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="f172b-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="f172b-128">Cela permet à la même instance de contexte de se connecter de manière dynamique à différentes bases de données.</span><span class="sxs-lookup"><span data-stu-id="f172b-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="f172b-129">La documentation est suivie d’un problème [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="f172b-129">Documentation is tracked by issue [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="f172b-130">Proxys de suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="f172b-130">Change-tracking proxies</span></span>

<span data-ttu-id="f172b-131">EF Core pouvez désormais générer des proxies d’exécution qui implémentent automatiquement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) et [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="f172b-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="f172b-132">Celles-ci rerapportent ensuite les modifications de valeur sur les propriétés d’entité directement à EF Core, évitant ainsi la nécessité d’analyser les modifications.</span><span class="sxs-lookup"><span data-stu-id="f172b-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="f172b-133">Toutefois, les proxies sont fournis avec leur propre ensemble de limitations, donc ils ne le sont pas pour tout le monde.</span><span class="sxs-lookup"><span data-stu-id="f172b-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="f172b-134">La documentation est suivie d’un problème [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="f172b-134">Documentation is tracked by issue [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="f172b-135">Amélioration de la gestion de la sémantique null de la base de données</span><span class="sxs-lookup"><span data-stu-id="f172b-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="f172b-136">En général, les bases de données relationnelles traitent la valeur NULL comme une valeur inconnue et, par conséquent, n’est pas égale à une autre valeur NULL.</span><span class="sxs-lookup"><span data-stu-id="f172b-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="f172b-137">C#, en revanche, traite la valeur NULL comme une valeur définie qui est comparée à toute autre valeur null.</span><span class="sxs-lookup"><span data-stu-id="f172b-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="f172b-138">EF Core par défaut traduit les requêtes afin qu’elles utilisent C# la sémantique null.</span><span class="sxs-lookup"><span data-stu-id="f172b-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="f172b-139">EF Core 5,0 améliore considérablement l’efficacité de ces traductions.</span><span class="sxs-lookup"><span data-stu-id="f172b-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="f172b-140">La documentation est suivie d’un problème [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="f172b-140">Documentation is tracked by issue [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="f172b-141">Propriétés de l’indexeur</span><span class="sxs-lookup"><span data-stu-id="f172b-141">Indexer properties</span></span>

<span data-ttu-id="f172b-142">EF Core 5,0 prend en charge C# le mappage des propriétés de l’indexeur.</span><span class="sxs-lookup"><span data-stu-id="f172b-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="f172b-143">Cela permet aux entités d’agir en tant que conteneurs de propriétés où les colonnes sont mappées à des propriétés nommées dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="f172b-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="f172b-144">La documentation est suivie d’un problème [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="f172b-144">Documentation is tracked by issue [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="f172b-145">Génération de contraintes de validation pour les mappages d’énumération</span><span class="sxs-lookup"><span data-stu-id="f172b-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="f172b-146">EF Core migrations 5,0 peut désormais générer des contraintes de validation pour les mappages de propriété d’énumération.</span><span class="sxs-lookup"><span data-stu-id="f172b-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="f172b-147">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f172b-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="f172b-148">La documentation est suivie d’un problème [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="f172b-148">Documentation is tracked by issue [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="f172b-149">Traductions de requêtes pour d’autres constructions DateTime</span><span class="sxs-lookup"><span data-stu-id="f172b-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="f172b-150">Les requêtes contenant la nouvelle construction DataTime sont désormais traduites.</span><span class="sxs-lookup"><span data-stu-id="f172b-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="f172b-151">En outre, la fonction SQL Server DateDiffWeek est maintenant mappée.</span><span class="sxs-lookup"><span data-stu-id="f172b-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="f172b-152">La documentation est suivie d’un problème [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f172b-152">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="f172b-153">Traductions de requêtes pour d’autres constructions de tableau d’octets</span><span class="sxs-lookup"><span data-stu-id="f172b-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="f172b-154">Les requêtes utilisant les propriétés Contains, Length, SequenceEqual, etc. sur Byte [] sont désormais traduites en SQL.</span><span class="sxs-lookup"><span data-stu-id="f172b-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="f172b-155">La documentation préliminaire est incluse dans l' [État EF hebdomadaire du 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f172b-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f172b-156">Une documentation supplémentaire est suivie par le [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)du problème.</span><span class="sxs-lookup"><span data-stu-id="f172b-156">Additional documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="f172b-157">Traduction de requête pour l’inverse</span><span class="sxs-lookup"><span data-stu-id="f172b-157">Query translation for Reverse</span></span>

<span data-ttu-id="f172b-158">Les requêtes qui utilisent des `Reverse` sont désormais traduites.</span><span class="sxs-lookup"><span data-stu-id="f172b-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="f172b-159">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f172b-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="f172b-160">La documentation est suivie d’un problème [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f172b-160">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="f172b-161">Traduction des requêtes pour les opérateurs au niveau du bit</span><span class="sxs-lookup"><span data-stu-id="f172b-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="f172b-162">Les requêtes utilisant des opérateurs de bits sont maintenant traduites dans d’autres cas, par exemple :</span><span class="sxs-lookup"><span data-stu-id="f172b-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="f172b-163">La documentation est suivie d’un problème [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f172b-163">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="f172b-164">Traduction des requêtes pour les chaînes sur Cosmos</span><span class="sxs-lookup"><span data-stu-id="f172b-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="f172b-165">Les requêtes qui utilisent les méthodes de chaîne Contains, StartsWith et EndsWith sont désormais traduites lors de l’utilisation du fournisseur Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f172b-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="f172b-166">La documentation est suivie d’un problème [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f172b-166">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>
