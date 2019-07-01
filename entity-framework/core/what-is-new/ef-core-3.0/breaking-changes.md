---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394827"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="cd92d-102">Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="cd92d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd92d-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="cd92d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="cd92d-104">Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="cd92d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="cd92d-105">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="cd92d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="cd92d-106">Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="cd92d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="cd92d-107">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="cd92d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="cd92d-108">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="cd92d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="cd92d-109">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-110">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-110">**Old behavior**</span></span>

<span data-ttu-id="cd92d-111">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="cd92d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="cd92d-112">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="cd92d-113">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-113">**New behavior**</span></span>

<span data-ttu-id="cd92d-114">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="cd92d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="cd92d-115">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="cd92d-116">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-116">**Why**</span></span>

<span data-ttu-id="cd92d-117">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="cd92d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="cd92d-118">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="cd92d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="cd92d-119">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="cd92d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="cd92d-120">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="cd92d-121">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="cd92d-122">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="cd92d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="cd92d-123">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-123">**Mitigations**</span></span>

<span data-ttu-id="cd92d-124">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="cd92d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="cd92d-125">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd92d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="cd92d-126">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="cd92d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="cd92d-127">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="cd92d-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="cd92d-128">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-128">**Old behavior**</span></span>

<span data-ttu-id="cd92d-129">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cd92d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="cd92d-130">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-130">**New behavior**</span></span>

<span data-ttu-id="cd92d-131">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd92d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="cd92d-132">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-132">**Why**</span></span>

<span data-ttu-id="cd92d-133">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="cd92d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="cd92d-134">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="cd92d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="cd92d-135">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="cd92d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="cd92d-136">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="cd92d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="cd92d-137">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-137">**Mitigations**</span></span>

<span data-ttu-id="cd92d-138">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="cd92d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="cd92d-139">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="cd92d-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="cd92d-140">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="cd92d-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="cd92d-141">Ce changement a été introduit dans EF Core 3.0-preview 4 et la version correspondante du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd92d-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="cd92d-142">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-142">**Old behavior**</span></span>

<span data-ttu-id="cd92d-143">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="cd92d-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="cd92d-144">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-144">**New behavior**</span></span>

<span data-ttu-id="cd92d-145">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="cd92d-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="cd92d-146">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-146">**Why**</span></span>

<span data-ttu-id="cd92d-147">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd92d-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="cd92d-148">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-148">**Mitigations**</span></span>

<span data-ttu-id="cd92d-149">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` à l’aide de la commande `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="cd92d-150">Par exemple, pour l’installer en tant qu’outil global, vous pouvez taper cette commande :</span><span class="sxs-lookup"><span data-stu-id="cd92d-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="cd92d-151">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="cd92d-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="cd92d-152">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="cd92d-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="cd92d-153">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="cd92d-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="cd92d-154">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-155">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-155">**Old behavior**</span></span>

<span data-ttu-id="cd92d-156">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="cd92d-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="cd92d-157">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-157">**New behavior**</span></span>

<span data-ttu-id="cd92d-158">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="cd92d-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="cd92d-159">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="cd92d-160">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="cd92d-161">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="cd92d-162">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="cd92d-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="cd92d-163">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-163">**Why**</span></span>

<span data-ttu-id="cd92d-164">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="cd92d-165">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="cd92d-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="cd92d-166">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-166">**Mitigations**</span></span>

<span data-ttu-id="cd92d-167">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="cd92d-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="cd92d-168">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="cd92d-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="cd92d-169">Suivi de problème no 15704</span><span class="sxs-lookup"><span data-stu-id="cd92d-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="cd92d-170">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="cd92d-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="cd92d-171">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-171">**Old behavior**</span></span>

<span data-ttu-id="cd92d-172">Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.</span><span class="sxs-lookup"><span data-stu-id="cd92d-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="cd92d-173">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-173">**New behavior**</span></span>

<span data-ttu-id="cd92d-174">À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="cd92d-175">Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="cd92d-176">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-176">**Why**</span></span>

<span data-ttu-id="cd92d-177">La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="cd92d-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="cd92d-178">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-178">**Mitigations**</span></span>

<span data-ttu-id="cd92d-179">Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="cd92d-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="cd92d-180">~~L’exécution de requêtes est enregistrée au niveau du débogage~~ rétabli</span><span class="sxs-lookup"><span data-stu-id="cd92d-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="cd92d-181">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="cd92d-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="cd92d-182">Ce changement a été rétabli dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="cd92d-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="cd92d-183">Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application.</span><span class="sxs-lookup"><span data-stu-id="cd92d-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="cd92d-184">Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :</span><span class="sxs-lookup"><span data-stu-id="cd92d-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="cd92d-185">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="cd92d-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="cd92d-186">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="cd92d-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="cd92d-187">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="cd92d-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd92d-188">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-188">**Old behavior**</span></span>

<span data-ttu-id="cd92d-189">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd92d-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="cd92d-190">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="cd92d-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="cd92d-191">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-191">**New behavior**</span></span>

<span data-ttu-id="cd92d-192">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="cd92d-193">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-193">**Why**</span></span>

<span data-ttu-id="cd92d-194">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="cd92d-195">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-195">**Mitigations**</span></span>

<span data-ttu-id="cd92d-196">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="cd92d-197">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="cd92d-197">This can be avoided by:</span></span>
* <span data-ttu-id="cd92d-198">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="cd92d-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="cd92d-199">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="cd92d-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="cd92d-200">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="cd92d-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="cd92d-201">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="cd92d-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="cd92d-202">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="cd92d-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="cd92d-203">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="cd92d-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="cd92d-204">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-205">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-205">**Old behavior**</span></span>

<span data-ttu-id="cd92d-206">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="cd92d-207">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-207">**New behavior**</span></span>

<span data-ttu-id="cd92d-208">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="cd92d-209">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="cd92d-210">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="cd92d-211">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-211">**Why**</span></span>

<span data-ttu-id="cd92d-212">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="cd92d-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="cd92d-213">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-213">**Mitigations**</span></span>

<span data-ttu-id="cd92d-214">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="cd92d-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="cd92d-215">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="cd92d-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="cd92d-216">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="cd92d-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="cd92d-217">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="cd92d-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="cd92d-218">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="cd92d-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="cd92d-219">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="cd92d-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="cd92d-220">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-221">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-221">**Old behavior**</span></span>

<span data-ttu-id="cd92d-222">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="cd92d-223">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-223">**New behavior**</span></span>

<span data-ttu-id="cd92d-224">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="cd92d-225">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="cd92d-226">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-226">**Why**</span></span>

<span data-ttu-id="cd92d-227">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="cd92d-228">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-228">**Mitigations**</span></span>

<span data-ttu-id="cd92d-229">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="cd92d-230">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="cd92d-231">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="cd92d-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="cd92d-232">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="cd92d-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="cd92d-233">Ce changement a été introduit dans EF Core 3.0-preview 5.</span><span class="sxs-lookup"><span data-stu-id="cd92d-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="cd92d-234">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-234">**Old behavior**</span></span>

<span data-ttu-id="cd92d-235">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="cd92d-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="cd92d-236">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-236">**New behavior**</span></span>

<span data-ttu-id="cd92d-237">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="cd92d-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="cd92d-238">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-238">**Why**</span></span>

<span data-ttu-id="cd92d-239">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="cd92d-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="cd92d-240">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-240">**Mitigations**</span></span>

<span data-ttu-id="cd92d-241">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="cd92d-242">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="cd92d-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="cd92d-243">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="cd92d-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="cd92d-244">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-245">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-245">**Old behavior**</span></span>

<span data-ttu-id="cd92d-246">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="cd92d-247">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="cd92d-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="cd92d-248">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-248">**New behavior**</span></span>

<span data-ttu-id="cd92d-249">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="cd92d-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="cd92d-250">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="cd92d-251">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-251">**Why**</span></span>

<span data-ttu-id="cd92d-252">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="cd92d-253">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="cd92d-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="cd92d-254">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="cd92d-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="cd92d-255">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-255">**Mitigations**</span></span>

<span data-ttu-id="cd92d-256">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="cd92d-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="cd92d-257">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="cd92d-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="cd92d-258">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="cd92d-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="cd92d-259">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="cd92d-260">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="cd92d-261">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="cd92d-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="cd92d-262">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="cd92d-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="cd92d-263">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-264">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-264">**Old behavior**</span></span>

<span data-ttu-id="cd92d-265">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="cd92d-266">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-266">**New behavior**</span></span>

<span data-ttu-id="cd92d-267">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="cd92d-268">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="cd92d-269">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="cd92d-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="cd92d-270">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="cd92d-271">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-271">For example:</span></span>

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

<span data-ttu-id="cd92d-272">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="cd92d-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="cd92d-273">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-273">**Why**</span></span>

<span data-ttu-id="cd92d-274">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="cd92d-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="cd92d-275">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="cd92d-276">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-276">**Mitigations**</span></span>

<span data-ttu-id="cd92d-277">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="cd92d-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="cd92d-278">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="cd92d-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="cd92d-279">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="cd92d-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="cd92d-280">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-281">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-281">**Old behavior**</span></span>

<span data-ttu-id="cd92d-282">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="cd92d-282">Consider the following model:</span></span>
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
<span data-ttu-id="cd92d-283">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="cd92d-284">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-284">**New behavior**</span></span>

<span data-ttu-id="cd92d-285">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="cd92d-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="cd92d-286">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="cd92d-287">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-287">**Mitigations**</span></span>

<span data-ttu-id="cd92d-288">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="cd92d-289">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="cd92d-290">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="cd92d-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="cd92d-291">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="cd92d-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="cd92d-292">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-293">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-293">**Old behavior**</span></span>

<span data-ttu-id="cd92d-294">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="cd92d-294">Consider the following model:</span></span>
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
<span data-ttu-id="cd92d-295">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="cd92d-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="cd92d-296">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-296">**New behavior**</span></span>

<span data-ttu-id="cd92d-297">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="cd92d-298">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="cd92d-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="cd92d-299">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-299">**Why**</span></span>

<span data-ttu-id="cd92d-300">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="cd92d-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="cd92d-301">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-301">**Mitigations**</span></span>

<span data-ttu-id="cd92d-302">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="cd92d-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="cd92d-303">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="cd92d-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="cd92d-304">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="cd92d-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="cd92d-305">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="cd92d-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="cd92d-306">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-307">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-307">**Old behavior**</span></span>

<span data-ttu-id="cd92d-308">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="cd92d-308">Consider the following model:</span></span>
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

<span data-ttu-id="cd92d-309">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="cd92d-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="cd92d-310">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-310">**New behavior**</span></span>

<span data-ttu-id="cd92d-311">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="cd92d-312">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-312">**Why**</span></span>

<span data-ttu-id="cd92d-313">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="cd92d-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="cd92d-314">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-314">**Mitigations**</span></span>

<span data-ttu-id="cd92d-315">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="cd92d-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="cd92d-316">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="cd92d-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="cd92d-317">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="cd92d-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="cd92d-318">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-319">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-319">**Old behavior**</span></span>

<span data-ttu-id="cd92d-320">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="cd92d-320">Consider the following model:</span></span>
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
<span data-ttu-id="cd92d-321">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="cd92d-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="cd92d-322">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="cd92d-323">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-323">**New behavior**</span></span>

<span data-ttu-id="cd92d-324">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="cd92d-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="cd92d-325">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="cd92d-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="cd92d-326">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-326">For example:</span></span>

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

<span data-ttu-id="cd92d-327">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-327">**Why**</span></span>

<span data-ttu-id="cd92d-328">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="cd92d-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="cd92d-329">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-329">**Mitigations**</span></span>

<span data-ttu-id="cd92d-330">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="cd92d-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="cd92d-331">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="cd92d-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="cd92d-332">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="cd92d-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="cd92d-333">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-334">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-334">**Old behavior**</span></span>

<span data-ttu-id="cd92d-335">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="cd92d-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="cd92d-336">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-336">**New behavior**</span></span>

<span data-ttu-id="cd92d-337">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="cd92d-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="cd92d-338">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-338">**Why**</span></span>

<span data-ttu-id="cd92d-339">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="cd92d-340">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="cd92d-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="cd92d-341">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-341">**Mitigations**</span></span>

<span data-ttu-id="cd92d-342">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="cd92d-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="cd92d-343">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="cd92d-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="cd92d-344">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="cd92d-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="cd92d-345">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-346">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-346">**Old behavior**</span></span>

<span data-ttu-id="cd92d-347">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="cd92d-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="cd92d-348">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-348">**New behavior**</span></span>

<span data-ttu-id="cd92d-349">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="cd92d-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="cd92d-350">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="cd92d-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="cd92d-351">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-351">**Why**</span></span>

<span data-ttu-id="cd92d-352">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="cd92d-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="cd92d-353">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-353">**Mitigations**</span></span>

<span data-ttu-id="cd92d-354">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="cd92d-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="cd92d-355">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="cd92d-356">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="cd92d-356">Backing fields are used by default</span></span>

[<span data-ttu-id="cd92d-357">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="cd92d-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="cd92d-358">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="cd92d-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd92d-359">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-359">**Old behavior**</span></span>

<span data-ttu-id="cd92d-360">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="cd92d-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="cd92d-361">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="cd92d-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="cd92d-362">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-362">**New behavior**</span></span>

<span data-ttu-id="cd92d-363">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd92d-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="cd92d-364">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="cd92d-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="cd92d-365">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-365">**Why**</span></span>

<span data-ttu-id="cd92d-366">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="cd92d-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="cd92d-367">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-367">**Mitigations**</span></span>

<span data-ttu-id="cd92d-368">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="cd92d-369">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="cd92d-370">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="cd92d-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="cd92d-371">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="cd92d-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="cd92d-372">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-373">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-373">**Old behavior**</span></span>

<span data-ttu-id="cd92d-374">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="cd92d-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="cd92d-375">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="cd92d-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="cd92d-376">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-376">**New behavior**</span></span>

<span data-ttu-id="cd92d-377">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="cd92d-378">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-378">**Why**</span></span>

<span data-ttu-id="cd92d-379">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="cd92d-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="cd92d-380">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-380">**Mitigations**</span></span>

<span data-ttu-id="cd92d-381">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="cd92d-382">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="cd92d-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="cd92d-383">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="cd92d-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="cd92d-384">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-385">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-385">**Old behavior**</span></span>

<span data-ttu-id="cd92d-386">Avant EF Core 3.0, une propriété pouvait être spécifiée par une valeur de chaîne, et si aucune propriété portant ce nom n’était trouvée dans le type CLR, EF Core tentait de la comparer à un champ à l’aide de règles de nommage.</span><span class="sxs-lookup"><span data-stu-id="cd92d-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="cd92d-387">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-387">**New behavior**</span></span>

<span data-ttu-id="cd92d-388">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="cd92d-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="cd92d-389">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-389">**Why**</span></span>

<span data-ttu-id="cd92d-390">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="cd92d-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="cd92d-391">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-391">**Mitigations**</span></span>

<span data-ttu-id="cd92d-392">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="cd92d-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="cd92d-393">Dans une future préversion d’EF Core 3.0, nous prévoyons de rendre de nouveau possible la configuration explicite d’un nom de champ qui est différent du nom de la propriété :</span><span class="sxs-lookup"><span data-stu-id="cd92d-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="cd92d-394">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="cd92d-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="cd92d-395">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="cd92d-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="cd92d-396">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-397">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-397">**Old behavior**</span></span>

<span data-ttu-id="cd92d-398">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="cd92d-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="cd92d-399">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-399">**New behavior**</span></span>

<span data-ttu-id="cd92d-400">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="cd92d-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="cd92d-401">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-401">**Why**</span></span>

<span data-ttu-id="cd92d-402">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="cd92d-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="cd92d-403">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="cd92d-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="cd92d-404">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-404">**Mitigations**</span></span>

<span data-ttu-id="cd92d-405">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="cd92d-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="cd92d-406">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="cd92d-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="cd92d-407">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="cd92d-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="cd92d-408">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-409">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-409">**Old behavior**</span></span>

<span data-ttu-id="cd92d-410">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="cd92d-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="cd92d-411">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="cd92d-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="cd92d-412">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-412">**New behavior**</span></span>

<span data-ttu-id="cd92d-413">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="cd92d-414">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="cd92d-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="cd92d-415">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="cd92d-416">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="cd92d-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="cd92d-417">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-417">**Why**</span></span>

<span data-ttu-id="cd92d-418">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="cd92d-419">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-419">**Mitigations**</span></span>

<span data-ttu-id="cd92d-420">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="cd92d-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="cd92d-421">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="cd92d-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="cd92d-422">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="cd92d-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="cd92d-423">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-424">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-424">**Old behavior**</span></span>

<span data-ttu-id="cd92d-425">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="cd92d-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="cd92d-426">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="cd92d-427">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-427">**New behavior**</span></span>

<span data-ttu-id="cd92d-428">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="cd92d-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="cd92d-429">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-429">**Why**</span></span>

<span data-ttu-id="cd92d-430">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="cd92d-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="cd92d-431">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-431">**Mitigations**</span></span>

<span data-ttu-id="cd92d-432">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="cd92d-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="cd92d-433">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="cd92d-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="cd92d-434">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="cd92d-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="cd92d-435">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="cd92d-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="cd92d-436">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="cd92d-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="cd92d-437">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-438">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-438">**Old behavior**</span></span>

<span data-ttu-id="cd92d-439">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="cd92d-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="cd92d-440">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-440">**New behavior**</span></span>

<span data-ttu-id="cd92d-441">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="cd92d-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="cd92d-442">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-442">**Why**</span></span>

<span data-ttu-id="cd92d-443">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="cd92d-444">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-444">**Mitigations**</span></span>

<span data-ttu-id="cd92d-445">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="cd92d-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="cd92d-446">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="cd92d-446">This isn't common.</span></span>
<span data-ttu-id="cd92d-447">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="cd92d-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="cd92d-448">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="cd92d-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="cd92d-449">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="cd92d-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="cd92d-450">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="cd92d-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="cd92d-451">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-452">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-452">**Old behavior**</span></span>

<span data-ttu-id="cd92d-453">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="cd92d-454">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="cd92d-455">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="cd92d-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="cd92d-456">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-456">**New behavior**</span></span>

<span data-ttu-id="cd92d-457">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="cd92d-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="cd92d-458">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="cd92d-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="cd92d-459">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="cd92d-460">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="cd92d-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="cd92d-461">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-461">**Why**</span></span>

<span data-ttu-id="cd92d-462">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="cd92d-463">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-463">**Mitigations**</span></span>

<span data-ttu-id="cd92d-464">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="cd92d-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="cd92d-465">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="cd92d-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="cd92d-466">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="cd92d-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="cd92d-467">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-468">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-468">**Old behavior**</span></span>

<span data-ttu-id="cd92d-469">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="cd92d-470">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-470">**New behavior**</span></span>

<span data-ttu-id="cd92d-471">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="cd92d-472">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-472">**Why**</span></span>

<span data-ttu-id="cd92d-473">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="cd92d-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="cd92d-474">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-474">**Mitigations**</span></span>

<span data-ttu-id="cd92d-475">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="cd92d-476">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="cd92d-477">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="cd92d-478">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="cd92d-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="cd92d-479">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="cd92d-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="cd92d-480">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-481">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-481">**Old behavior**</span></span>

<span data-ttu-id="cd92d-482">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="cd92d-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="cd92d-483">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="cd92d-484">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="cd92d-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="cd92d-485">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="cd92d-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="cd92d-486">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-486">**New behavior**</span></span>

<span data-ttu-id="cd92d-487">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="cd92d-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="cd92d-488">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-488">**Why**</span></span>

<span data-ttu-id="cd92d-489">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="cd92d-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="cd92d-490">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-490">**Mitigations**</span></span>

<span data-ttu-id="cd92d-491">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="cd92d-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="cd92d-492">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="cd92d-492">This is not common.</span></span>
<span data-ttu-id="cd92d-493">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="cd92d-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="cd92d-494">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="cd92d-495">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="cd92d-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="cd92d-496">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="cd92d-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="cd92d-497">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-498">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-498">**Old behavior**</span></span>

<span data-ttu-id="cd92d-499">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="cd92d-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="cd92d-500">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="cd92d-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="cd92d-501">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-501">**New behavior**</span></span>

<span data-ttu-id="cd92d-502">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="cd92d-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="cd92d-503">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-503">**Why**</span></span>

<span data-ttu-id="cd92d-504">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="cd92d-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="cd92d-505">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-505">**Mitigations**</span></span>

<span data-ttu-id="cd92d-506">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="cd92d-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="cd92d-507">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="cd92d-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="cd92d-508">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="cd92d-509">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd92d-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="cd92d-510">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="cd92d-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="cd92d-511">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="cd92d-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd92d-512">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-512">**Old behavior**</span></span>

<span data-ttu-id="cd92d-513">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="cd92d-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="cd92d-514">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-514">**New behavior**</span></span>

<span data-ttu-id="cd92d-515">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="cd92d-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="cd92d-516">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-516">**Why**</span></span>

<span data-ttu-id="cd92d-517">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="cd92d-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="cd92d-518">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-518">**Mitigations**</span></span>

<span data-ttu-id="cd92d-519">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="cd92d-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="cd92d-520">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="cd92d-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="cd92d-521">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="cd92d-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="cd92d-522">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="cd92d-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="cd92d-523">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-524">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-524">**Old behavior**</span></span>

<span data-ttu-id="cd92d-525">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="cd92d-526">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-526">**New behavior**</span></span>

<span data-ttu-id="cd92d-527">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="cd92d-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="cd92d-528">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-528">**Why**</span></span>

<span data-ttu-id="cd92d-529">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="cd92d-530">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="cd92d-531">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-531">**Mitigations**</span></span>

<span data-ttu-id="cd92d-532">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="cd92d-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="cd92d-533">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="cd92d-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="cd92d-534">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="cd92d-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="cd92d-535">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-536">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-536">**Old behavior**</span></span>

<span data-ttu-id="cd92d-537">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="cd92d-538">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-538">**New behavior**</span></span>

<span data-ttu-id="cd92d-539">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="cd92d-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="cd92d-540">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="cd92d-541">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-541">**Why**</span></span>

<span data-ttu-id="cd92d-542">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="cd92d-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="cd92d-543">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-543">**Mitigations**</span></span>

<span data-ttu-id="cd92d-544">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="cd92d-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="cd92d-545">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="cd92d-545">Metadata API changes</span></span>

[<span data-ttu-id="cd92d-546">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="cd92d-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="cd92d-547">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-548">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-548">**New behavior**</span></span>

<span data-ttu-id="cd92d-549">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="cd92d-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="cd92d-550">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-550">**Why**</span></span>

<span data-ttu-id="cd92d-551">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="cd92d-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="cd92d-552">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-552">**Mitigations**</span></span>

<span data-ttu-id="cd92d-553">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="cd92d-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="cd92d-554">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="cd92d-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="cd92d-555">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="cd92d-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="cd92d-556">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="cd92d-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="cd92d-557">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-557">**New behavior**</span></span>

<span data-ttu-id="cd92d-558">Les méthodes d’extension spécifiques au fournisseur seront aplaties :</span><span class="sxs-lookup"><span data-stu-id="cd92d-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="cd92d-559">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-559">**Why**</span></span>

<span data-ttu-id="cd92d-560">Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="cd92d-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="cd92d-561">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-561">**Mitigations**</span></span>

<span data-ttu-id="cd92d-562">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="cd92d-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="cd92d-563">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="cd92d-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="cd92d-564">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="cd92d-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="cd92d-565">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="cd92d-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd92d-566">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-566">**Old behavior**</span></span>

<span data-ttu-id="cd92d-567">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="cd92d-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="cd92d-568">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-568">**New behavior**</span></span>

<span data-ttu-id="cd92d-569">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="cd92d-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="cd92d-570">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-570">**Why**</span></span>

<span data-ttu-id="cd92d-571">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="cd92d-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="cd92d-572">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-572">**Mitigations**</span></span>

<span data-ttu-id="cd92d-573">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd92d-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="cd92d-574">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="cd92d-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="cd92d-575">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="cd92d-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="cd92d-576">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-576">**Old behavior**</span></span>

<span data-ttu-id="cd92d-577">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="cd92d-578">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-578">**New behavior**</span></span>

<span data-ttu-id="cd92d-579">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="cd92d-580">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-580">**Why**</span></span>

<span data-ttu-id="cd92d-581">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="cd92d-582">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-582">**Mitigations**</span></span>

<span data-ttu-id="cd92d-583">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cd92d-584">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="cd92d-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cd92d-585">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="cd92d-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="cd92d-586">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-587">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-587">**Old behavior**</span></span>

<span data-ttu-id="cd92d-588">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="cd92d-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="cd92d-589">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-589">**New behavior**</span></span>

<span data-ttu-id="cd92d-590">Maintenant, les valeurs Guid sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="cd92d-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="cd92d-591">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-591">**Why**</span></span>

<span data-ttu-id="cd92d-592">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="cd92d-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="cd92d-593">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="cd92d-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="cd92d-594">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-594">**Mitigations**</span></span>

<span data-ttu-id="cd92d-595">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="cd92d-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="cd92d-596">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="cd92d-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="cd92d-597">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="cd92d-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cd92d-598">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="cd92d-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cd92d-599">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="cd92d-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="cd92d-600">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-601">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-601">**Old behavior**</span></span>

<span data-ttu-id="cd92d-602">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="cd92d-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="cd92d-603">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="cd92d-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="cd92d-604">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-604">**New behavior**</span></span>

<span data-ttu-id="cd92d-605">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="cd92d-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="cd92d-606">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-606">**Why**</span></span>

<span data-ttu-id="cd92d-607">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="cd92d-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="cd92d-608">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-608">**Mitigations**</span></span>

<span data-ttu-id="cd92d-609">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="cd92d-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="cd92d-610">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="cd92d-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="cd92d-611">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="cd92d-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="cd92d-612">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="cd92d-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="cd92d-613">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="cd92d-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="cd92d-614">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-615">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-615">**Old behavior**</span></span>

<span data-ttu-id="cd92d-616">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="cd92d-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="cd92d-617">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-617">**New behavior**</span></span>

<span data-ttu-id="cd92d-618">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="cd92d-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="cd92d-619">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-619">**Why**</span></span>

<span data-ttu-id="cd92d-620">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="cd92d-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="cd92d-621">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="cd92d-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="cd92d-622">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-622">**Mitigations**</span></span>

<span data-ttu-id="cd92d-623">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="cd92d-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="cd92d-624">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="cd92d-625">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="cd92d-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="cd92d-626">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="cd92d-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="cd92d-627">Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="cd92d-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="cd92d-628">Suivi du problème no 16119</span><span class="sxs-lookup"><span data-stu-id="cd92d-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="cd92d-629">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="cd92d-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="cd92d-630">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-630">**Old behavior**</span></span>

<span data-ttu-id="cd92d-631">`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.</span><span class="sxs-lookup"><span data-stu-id="cd92d-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="cd92d-632">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-632">**New behavior**</span></span>

<span data-ttu-id="cd92d-633">Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="cd92d-634">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-634">**Why**</span></span>

<span data-ttu-id="cd92d-635">Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="cd92d-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="cd92d-636">Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.</span><span class="sxs-lookup"><span data-stu-id="cd92d-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="cd92d-637">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-637">**Mitigations**</span></span>

<span data-ttu-id="cd92d-638">Mettre à jour des extensions pour suivre le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="cd92d-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="cd92d-639">Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd92d-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="cd92d-640">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="cd92d-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="cd92d-641">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="cd92d-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="cd92d-642">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-643">**Changement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-643">**Change**</span></span>

<span data-ttu-id="cd92d-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="cd92d-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="cd92d-645">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-645">**Why**</span></span>

<span data-ttu-id="cd92d-646">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="cd92d-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="cd92d-647">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-647">**Mitigations**</span></span>

<span data-ttu-id="cd92d-648">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="cd92d-648">Use the new name.</span></span> <span data-ttu-id="cd92d-649">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="cd92d-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="cd92d-650">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="cd92d-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="cd92d-651">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="cd92d-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="cd92d-652">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-653">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-653">**Old behavior**</span></span>

<span data-ttu-id="cd92d-654">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="cd92d-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="cd92d-655">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="cd92d-656">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-656">**New behavior**</span></span>

<span data-ttu-id="cd92d-657">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="cd92d-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="cd92d-658">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="cd92d-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="cd92d-659">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-659">**Why**</span></span>

<span data-ttu-id="cd92d-660">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="cd92d-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="cd92d-661">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-661">**Mitigations**</span></span>

<span data-ttu-id="cd92d-662">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="cd92d-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="cd92d-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="cd92d-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="cd92d-664">Suivi du problème no 15997</span><span class="sxs-lookup"><span data-stu-id="cd92d-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="cd92d-665">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="cd92d-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="cd92d-666">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-666">**Old behavior**</span></span>

<span data-ttu-id="cd92d-667">Avant EF Core 3.0, ces méthodes étaient protégées.</span><span class="sxs-lookup"><span data-stu-id="cd92d-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="cd92d-668">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-668">**New behavior**</span></span>

<span data-ttu-id="cd92d-669">À compter d’EF Core 3.0, ces méthodes sont publiques.</span><span class="sxs-lookup"><span data-stu-id="cd92d-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="cd92d-670">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-670">**Why**</span></span>

<span data-ttu-id="cd92d-671">Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide.</span><span class="sxs-lookup"><span data-stu-id="cd92d-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="cd92d-672">Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.</span><span class="sxs-lookup"><span data-stu-id="cd92d-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="cd92d-673">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-673">**Mitigations**</span></span>

<span data-ttu-id="cd92d-674">Modifiez l’accessibilité de n’importe quel remplacements.</span><span class="sxs-lookup"><span data-stu-id="cd92d-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="cd92d-675">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="cd92d-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="cd92d-676">Suivi du problème no 11506</span><span class="sxs-lookup"><span data-stu-id="cd92d-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="cd92d-677">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="cd92d-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd92d-678">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-678">**Old behavior**</span></span>

<span data-ttu-id="cd92d-679">Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.</span><span class="sxs-lookup"><span data-stu-id="cd92d-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="cd92d-680">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-680">**New behavior**</span></span>

<span data-ttu-id="cd92d-681">À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="cd92d-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="cd92d-682">Ce qui signifie que la dépendance ne circule pas de manière transitive dans d’autres projets et que vous ne pouvez plus, par défaut, faire référence à son assembly.</span><span class="sxs-lookup"><span data-stu-id="cd92d-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="cd92d-683">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-683">**Why**</span></span>

<span data-ttu-id="cd92d-684">Ce package est uniquement destiné à être utilisé au moment du design.</span><span class="sxs-lookup"><span data-stu-id="cd92d-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="cd92d-685">Les applications déployées ne doivent pas y faire référence.</span><span class="sxs-lookup"><span data-stu-id="cd92d-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="cd92d-686">Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="cd92d-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="cd92d-687">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-687">**Mitigations**</span></span>

<span data-ttu-id="cd92d-688">Si vous devez référencer ce package pour écraser le comportement au moment du design d’EF Core, vous pouvez mettre à jour des métadonnées d’élément PackageReference dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="cd92d-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="cd92d-689">Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées.</span><span class="sxs-lookup"><span data-stu-id="cd92d-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="cd92d-690">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="cd92d-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="cd92d-691">Suivi du problème no 14824</span><span class="sxs-lookup"><span data-stu-id="cd92d-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="cd92d-692">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="cd92d-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="cd92d-693">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-693">**Old behavior**</span></span>

<span data-ttu-id="cd92d-694">Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="cd92d-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="cd92d-695">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="cd92d-695">**New behavior**</span></span>

<span data-ttu-id="cd92d-696">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="cd92d-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="cd92d-697">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="cd92d-697">**Why**</span></span>

<span data-ttu-id="cd92d-698">La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="cd92d-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="cd92d-699">Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="cd92d-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="cd92d-700">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="cd92d-700">**Mitigations**</span></span>

<span data-ttu-id="cd92d-701">SQLitePCL.raw version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="cd92d-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="cd92d-702">Pour plus d’informations, consultez les [notes de publication](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="cd92d-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
