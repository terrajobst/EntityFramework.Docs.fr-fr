---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b1b5e286e08a8b6b4efe225a176e76023f9fdd20
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405239"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1d649-102">Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="1d649-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d649-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="1d649-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1d649-104">Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1d649-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1d649-105">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1d649-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1d649-106">Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="1d649-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1d649-107">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="1d649-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1d649-108">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1d649-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1d649-109">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-110">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-110">**Old behavior**</span></span>

<span data-ttu-id="1d649-111">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="1d649-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1d649-112">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="1d649-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1d649-113">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-113">**New behavior**</span></span>

<span data-ttu-id="1d649-114">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="1d649-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1d649-115">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="1d649-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1d649-116">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-116">**Why**</span></span>

<span data-ttu-id="1d649-117">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="1d649-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1d649-118">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="1d649-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1d649-119">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="1d649-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1d649-120">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="1d649-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1d649-121">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="1d649-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1d649-122">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="1d649-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1d649-123">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-123">**Mitigations**</span></span>

<span data-ttu-id="1d649-124">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="1d649-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1d649-125">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d649-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1d649-126">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="1d649-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1d649-127">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="1d649-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1d649-128">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-128">**Old behavior**</span></span>

<span data-ttu-id="1d649-129">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1d649-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1d649-130">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-130">**New behavior**</span></span>

<span data-ttu-id="1d649-131">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d649-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1d649-132">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-132">**Why**</span></span>

<span data-ttu-id="1d649-133">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="1d649-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1d649-134">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="1d649-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1d649-135">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="1d649-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1d649-136">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="1d649-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1d649-137">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-137">**Mitigations**</span></span>

<span data-ttu-id="1d649-138">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="1d649-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1d649-139">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="1d649-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1d649-140">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="1d649-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1d649-141">Ce changement a été introduit dans EF Core 3.0-preview 4 et la version correspondante du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d649-141">This change was introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1d649-142">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-142">**Old behavior**</span></span>

<span data-ttu-id="1d649-143">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="1d649-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1d649-144">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-144">**New behavior**</span></span>

<span data-ttu-id="1d649-145">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="1d649-145">Starting in 3.0, the .NET SDK does not incude the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1d649-146">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-146">**Why**</span></span>

<span data-ttu-id="1d649-147">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d649-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1d649-148">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-148">**Mitigations**</span></span>

<span data-ttu-id="1d649-149">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` à l’aide de la commande `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="1d649-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="1d649-150">Par exemple, pour l’installer en tant qu’outil global, vous pouvez taper cette commande :</span><span class="sxs-lookup"><span data-stu-id="1d649-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="1d649-151">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1d649-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1d649-152">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="1d649-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1d649-153">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="1d649-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1d649-154">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-154">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-155">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-155">**Old behavior**</span></span>

<span data-ttu-id="1d649-156">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="1d649-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1d649-157">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-157">**New behavior**</span></span>

<span data-ttu-id="1d649-158">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="1d649-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1d649-159">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1d649-160">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="1d649-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1d649-161">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1d649-162">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="1d649-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1d649-163">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-163">**Why**</span></span>

<span data-ttu-id="1d649-164">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="1d649-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1d649-165">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="1d649-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1d649-166">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-166">**Mitigations**</span></span>

<span data-ttu-id="1d649-167">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="1d649-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="1d649-168">L’exécution de requête est enregistrée dans le journal au niveau du débogage</span><span class="sxs-lookup"><span data-stu-id="1d649-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="1d649-169">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="1d649-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1d649-170">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-170">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-171">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-171">**Old behavior**</span></span>

<span data-ttu-id="1d649-172">Avant EF Core 3.0, l’exécution des requêtes et autres commandes était enregistrée au niveau `Info`.</span><span class="sxs-lookup"><span data-stu-id="1d649-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="1d649-173">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-173">**New behavior**</span></span>

<span data-ttu-id="1d649-174">À compter d’EF Core 3.0, la journalisation de l’exécution des commandes et du SQL s’effectue au niveau `Debug`.</span><span class="sxs-lookup"><span data-stu-id="1d649-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="1d649-175">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-175">**Why**</span></span>

<span data-ttu-id="1d649-176">Ce changement a été apporté afin de réduire le bruit au niveau de journal `Info`.</span><span class="sxs-lookup"><span data-stu-id="1d649-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="1d649-177">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-177">**Mitigations**</span></span>

<span data-ttu-id="1d649-178">Cet événement de journalisation est défini par `RelationalEventId.CommandExecuting` avec l’ID d’événement 20100.</span><span class="sxs-lookup"><span data-stu-id="1d649-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="1d649-179">Pour reconnecter SQL au niveau `Info`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1d649-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="1d649-180">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1d649-181">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="1d649-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1d649-182">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="1d649-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1d649-183">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="1d649-183">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d649-184">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-184">**Old behavior**</span></span>

<span data-ttu-id="1d649-185">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d649-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1d649-186">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="1d649-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1d649-187">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-187">**New behavior**</span></span>

<span data-ttu-id="1d649-188">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="1d649-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1d649-189">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-189">**Why**</span></span>

<span data-ttu-id="1d649-190">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1d649-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1d649-191">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-191">**Mitigations**</span></span>

<span data-ttu-id="1d649-192">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="1d649-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1d649-193">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="1d649-193">This can be avoided by:</span></span>
* <span data-ttu-id="1d649-194">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="1d649-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="1d649-195">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="1d649-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1d649-196">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="1d649-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1d649-197">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="1d649-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1d649-198">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="1d649-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1d649-199">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="1d649-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1d649-200">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-201">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-201">**Old behavior**</span></span>

<span data-ttu-id="1d649-202">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="1d649-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1d649-203">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-203">**New behavior**</span></span>

<span data-ttu-id="1d649-204">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1d649-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1d649-205">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1d649-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1d649-206">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="1d649-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1d649-207">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-207">**Why**</span></span>

<span data-ttu-id="1d649-208">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="1d649-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1d649-209">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-209">**Mitigations**</span></span>

<span data-ttu-id="1d649-210">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="1d649-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1d649-211">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="1d649-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1d649-212">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="1d649-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1d649-213">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="1d649-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1d649-214">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="1d649-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1d649-215">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="1d649-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1d649-216">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-217">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-217">**Old behavior**</span></span>

<span data-ttu-id="1d649-218">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="1d649-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1d649-219">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-219">**New behavior**</span></span>

<span data-ttu-id="1d649-220">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="1d649-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1d649-221">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="1d649-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1d649-222">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-222">**Why**</span></span>

<span data-ttu-id="1d649-223">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1d649-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1d649-224">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-224">**Mitigations**</span></span>

<span data-ttu-id="1d649-225">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1d649-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1d649-226">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1d649-227">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="1d649-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1d649-228">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="1d649-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1d649-229">Ce changement sera introduit dans EF Core 3.0-preview 5.</span><span class="sxs-lookup"><span data-stu-id="1d649-229">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1d649-230">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-230">**Old behavior**</span></span>

<span data-ttu-id="1d649-231">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="1d649-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1d649-232">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-232">**New behavior**</span></span>

<span data-ttu-id="1d649-233">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="1d649-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1d649-234">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-234">**Why**</span></span>

<span data-ttu-id="1d649-235">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="1d649-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1d649-236">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-236">**Mitigations**</span></span>

<span data-ttu-id="1d649-237">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1d649-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1d649-238">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="1d649-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1d649-239">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="1d649-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1d649-240">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-240">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-241">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-241">**Old behavior**</span></span>

<span data-ttu-id="1d649-242">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="1d649-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1d649-243">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="1d649-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1d649-244">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-244">**New behavior**</span></span>

<span data-ttu-id="1d649-245">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="1d649-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1d649-246">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="1d649-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1d649-247">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-247">**Why**</span></span>

<span data-ttu-id="1d649-248">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="1d649-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1d649-249">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="1d649-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1d649-250">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="1d649-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1d649-251">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-251">**Mitigations**</span></span>

<span data-ttu-id="1d649-252">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="1d649-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1d649-253">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="1d649-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1d649-254">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="1d649-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1d649-255">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1d649-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1d649-256">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="1d649-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1d649-257">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="1d649-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1d649-258">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1d649-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1d649-259">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-259">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-260">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-260">**Old behavior**</span></span>

<span data-ttu-id="1d649-261">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="1d649-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1d649-262">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-262">**New behavior**</span></span>

<span data-ttu-id="1d649-263">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1d649-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1d649-264">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1d649-265">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="1d649-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1d649-266">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1d649-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1d649-267">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-267">For example:</span></span>

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

<span data-ttu-id="1d649-268">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="1d649-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1d649-269">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-269">**Why**</span></span>

<span data-ttu-id="1d649-270">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="1d649-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1d649-271">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1d649-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1d649-272">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-272">**Mitigations**</span></span>

<span data-ttu-id="1d649-273">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="1d649-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1d649-274">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="1d649-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1d649-275">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="1d649-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1d649-276">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-276">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-277">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-277">**Old behavior**</span></span>

<span data-ttu-id="1d649-278">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="1d649-278">Consider the following model:</span></span>
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
<span data-ttu-id="1d649-279">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="1d649-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1d649-280">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-280">**New behavior**</span></span>

<span data-ttu-id="1d649-281">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="1d649-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1d649-282">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="1d649-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1d649-283">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-283">**Mitigations**</span></span>

<span data-ttu-id="1d649-284">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="1d649-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1d649-285">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="1d649-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1d649-286">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="1d649-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1d649-287">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="1d649-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1d649-288">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-288">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-289">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-289">**Old behavior**</span></span>

<span data-ttu-id="1d649-290">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="1d649-290">Consider the following model:</span></span>
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
<span data-ttu-id="1d649-291">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="1d649-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1d649-292">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-292">**New behavior**</span></span>

<span data-ttu-id="1d649-293">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1d649-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1d649-294">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="1d649-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1d649-295">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-295">**Why**</span></span>

<span data-ttu-id="1d649-296">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="1d649-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1d649-297">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-297">**Mitigations**</span></span>

<span data-ttu-id="1d649-298">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="1d649-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1d649-299">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="1d649-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1d649-300">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="1d649-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1d649-301">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="1d649-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1d649-302">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-302">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-303">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-303">**Old behavior**</span></span>

<span data-ttu-id="1d649-304">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="1d649-304">Consider the following model:</span></span>
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

<span data-ttu-id="1d649-305">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="1d649-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1d649-306">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-306">**New behavior**</span></span>

<span data-ttu-id="1d649-307">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1d649-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1d649-308">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-308">**Why**</span></span>

<span data-ttu-id="1d649-309">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="1d649-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1d649-310">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-310">**Mitigations**</span></span>

<span data-ttu-id="1d649-311">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="1d649-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1d649-312">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="1d649-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1d649-313">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="1d649-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1d649-314">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-314">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-315">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-315">**Old behavior**</span></span>

<span data-ttu-id="1d649-316">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="1d649-316">Consider the following model:</span></span>
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
<span data-ttu-id="1d649-317">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="1d649-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1d649-318">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="1d649-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1d649-319">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-319">**New behavior**</span></span>

<span data-ttu-id="1d649-320">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="1d649-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1d649-321">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="1d649-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1d649-322">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-322">For example:</span></span>

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

<span data-ttu-id="1d649-323">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-323">**Why**</span></span>

<span data-ttu-id="1d649-324">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="1d649-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1d649-325">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-325">**Mitigations**</span></span>

<span data-ttu-id="1d649-326">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="1d649-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1d649-327">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="1d649-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1d649-328">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="1d649-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1d649-329">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-329">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-330">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-330">**Old behavior**</span></span>

<span data-ttu-id="1d649-331">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="1d649-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="1d649-332">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-332">**New behavior**</span></span>

<span data-ttu-id="1d649-333">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="1d649-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1d649-334">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-334">**Why**</span></span>

<span data-ttu-id="1d649-335">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1d649-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1d649-336">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="1d649-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1d649-337">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-337">**Mitigations**</span></span>

<span data-ttu-id="1d649-338">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="1d649-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1d649-339">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="1d649-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1d649-340">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="1d649-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1d649-341">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-341">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-342">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-342">**Old behavior**</span></span>

<span data-ttu-id="1d649-343">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="1d649-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1d649-344">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-344">**New behavior**</span></span>

<span data-ttu-id="1d649-345">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="1d649-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1d649-346">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="1d649-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1d649-347">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-347">**Why**</span></span>

<span data-ttu-id="1d649-348">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="1d649-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1d649-349">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-349">**Mitigations**</span></span>

<span data-ttu-id="1d649-350">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="1d649-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1d649-351">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="1d649-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1d649-352">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="1d649-352">Backing fields are used by default</span></span>

[<span data-ttu-id="1d649-353">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="1d649-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1d649-354">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="1d649-354">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d649-355">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-355">**Old behavior**</span></span>

<span data-ttu-id="1d649-356">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="1d649-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1d649-357">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="1d649-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1d649-358">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-358">**New behavior**</span></span>

<span data-ttu-id="1d649-359">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="1d649-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1d649-360">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="1d649-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1d649-361">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-361">**Why**</span></span>

<span data-ttu-id="1d649-362">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="1d649-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1d649-363">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-363">**Mitigations**</span></span>

<span data-ttu-id="1d649-364">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété dans l’API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="1d649-364">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="1d649-365">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1d649-366">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="1d649-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1d649-367">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="1d649-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1d649-368">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-368">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-369">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-369">**Old behavior**</span></span>

<span data-ttu-id="1d649-370">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="1d649-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1d649-371">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="1d649-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1d649-372">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-372">**New behavior**</span></span>

<span data-ttu-id="1d649-373">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="1d649-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1d649-374">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-374">**Why**</span></span>

<span data-ttu-id="1d649-375">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="1d649-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1d649-376">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-376">**Mitigations**</span></span>

<span data-ttu-id="1d649-377">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="1d649-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1d649-378">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="1d649-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1d649-379">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="1d649-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="1d649-380">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-380">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-381">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-381">**Old behavior**</span></span>

<span data-ttu-id="1d649-382">Avant EF Core 3.0, une propriété pouvait être spécifiée par une valeur de chaîne, et si aucune propriété portant ce nom n’était trouvée dans le type CLR, EF Core tentait de la comparer à un champ à l’aide de règles de nommage.</span><span class="sxs-lookup"><span data-stu-id="1d649-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
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

<span data-ttu-id="1d649-383">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-383">**New behavior**</span></span>

<span data-ttu-id="1d649-384">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="1d649-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1d649-385">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-385">**Why**</span></span>

<span data-ttu-id="1d649-386">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="1d649-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1d649-387">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-387">**Mitigations**</span></span>

<span data-ttu-id="1d649-388">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="1d649-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1d649-389">Dans une future préversion d’EF Core 3.0, nous prévoyons de rendre de nouveau possible la configuration explicite d’un nom de champ qui est différent du nom de la propriété :</span><span class="sxs-lookup"><span data-stu-id="1d649-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1d649-390">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1d649-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1d649-391">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="1d649-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1d649-392">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-392">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-393">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-393">**Old behavior**</span></span>

<span data-ttu-id="1d649-394">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1d649-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1d649-395">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-395">**New behavior**</span></span>

<span data-ttu-id="1d649-396">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="1d649-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1d649-397">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-397">**Why**</span></span>

<span data-ttu-id="1d649-398">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="1d649-398">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="1d649-399">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="1d649-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1d649-400">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-400">**Mitigations**</span></span>

<span data-ttu-id="1d649-401">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1d649-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1d649-402">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="1d649-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1d649-403">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="1d649-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1d649-404">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-404">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-405">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-405">**Old behavior**</span></span>

<span data-ttu-id="1d649-406">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="1d649-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1d649-407">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="1d649-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1d649-408">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-408">**New behavior**</span></span>

<span data-ttu-id="1d649-409">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="1d649-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1d649-410">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="1d649-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1d649-411">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="1d649-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1d649-412">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="1d649-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1d649-413">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-413">**Why**</span></span>

<span data-ttu-id="1d649-414">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1d649-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1d649-415">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-415">**Mitigations**</span></span>

<span data-ttu-id="1d649-416">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="1d649-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1d649-417">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="1d649-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1d649-418">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="1d649-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1d649-419">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-419">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-420">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-420">**Old behavior**</span></span>

<span data-ttu-id="1d649-421">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="1d649-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1d649-422">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1d649-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1d649-423">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-423">**New behavior**</span></span>

<span data-ttu-id="1d649-424">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="1d649-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1d649-425">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-425">**Why**</span></span>

<span data-ttu-id="1d649-426">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="1d649-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1d649-427">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-427">**Mitigations**</span></span>

<span data-ttu-id="1d649-428">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="1d649-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1d649-429">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="1d649-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1d649-430">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="1d649-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1d649-431">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="1d649-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1d649-432">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="1d649-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1d649-433">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-433">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-434">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-434">**Old behavior**</span></span>

<span data-ttu-id="1d649-435">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="1d649-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1d649-436">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-436">**New behavior**</span></span>

<span data-ttu-id="1d649-437">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="1d649-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1d649-438">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-438">**Why**</span></span>

<span data-ttu-id="1d649-439">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="1d649-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1d649-440">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-440">**Mitigations**</span></span>

<span data-ttu-id="1d649-441">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="1d649-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1d649-442">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="1d649-442">This isn't common.</span></span>
<span data-ttu-id="1d649-443">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="1d649-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1d649-444">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="1d649-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="1d649-445">IDbContextOptionsExtensionWithDebugInfo fusionnée dans IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1d649-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1d649-446">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="1d649-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1d649-447">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-447">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-448">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-448">**Old behavior**</span></span>

<span data-ttu-id="1d649-449">`IDbContextOptionsExtensionWithDebugInfo` était une interface facultative supplémentaire étendue à partir de `IDbContextOptionsExtension` pour éviter de créer un changement cassant dans l’interface durant le cycle de mise en production 2.x.</span><span class="sxs-lookup"><span data-stu-id="1d649-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="1d649-450">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-450">**New behavior**</span></span>

<span data-ttu-id="1d649-451">Les interfaces sont maintenant fusionnées dans `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="1d649-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="1d649-452">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-452">**Why**</span></span>

<span data-ttu-id="1d649-453">Ce changement a été apporté car les interfaces ne font conceptuellement qu’une.</span><span class="sxs-lookup"><span data-stu-id="1d649-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="1d649-454">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-454">**Mitigations**</span></span>

<span data-ttu-id="1d649-455">Toute implémentation de `IDbContextOptionsExtension` devra être mise à jour pour prendre en charge le nouveau membre.</span><span class="sxs-lookup"><span data-stu-id="1d649-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1d649-456">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="1d649-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1d649-457">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="1d649-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1d649-458">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-458">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-459">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-459">**Old behavior**</span></span>

<span data-ttu-id="1d649-460">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="1d649-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1d649-461">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="1d649-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1d649-462">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="1d649-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1d649-463">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-463">**New behavior**</span></span>

<span data-ttu-id="1d649-464">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="1d649-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1d649-465">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="1d649-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1d649-466">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="1d649-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1d649-467">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="1d649-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1d649-468">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-468">**Why**</span></span>

<span data-ttu-id="1d649-469">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="1d649-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1d649-470">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-470">**Mitigations**</span></span>

<span data-ttu-id="1d649-471">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="1d649-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1d649-472">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="1d649-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1d649-473">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="1d649-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1d649-474">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-474">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-475">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-475">**Old behavior**</span></span>

<span data-ttu-id="1d649-476">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="1d649-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1d649-477">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-477">**New behavior**</span></span>

<span data-ttu-id="1d649-478">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="1d649-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1d649-479">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-479">**Why**</span></span>

<span data-ttu-id="1d649-480">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="1d649-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1d649-481">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-481">**Mitigations**</span></span>

<span data-ttu-id="1d649-482">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="1d649-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1d649-483">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d649-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1d649-484">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1d649-485">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="1d649-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1d649-486">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="1d649-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1d649-487">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-487">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-488">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-488">**Old behavior**</span></span>

<span data-ttu-id="1d649-489">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="1d649-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="1d649-490">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1d649-491">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="1d649-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1d649-492">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="1d649-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1d649-493">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-493">**New behavior**</span></span>

<span data-ttu-id="1d649-494">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="1d649-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1d649-495">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-495">**Why**</span></span>

<span data-ttu-id="1d649-496">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="1d649-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1d649-497">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-497">**Mitigations**</span></span>

<span data-ttu-id="1d649-498">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="1d649-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1d649-499">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="1d649-499">This is not common.</span></span>
<span data-ttu-id="1d649-500">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="1d649-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1d649-501">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1d649-502">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="1d649-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1d649-503">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="1d649-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1d649-504">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-504">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-505">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-505">**Old behavior**</span></span>

<span data-ttu-id="1d649-506">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="1d649-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1d649-507">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="1d649-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1d649-508">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-508">**New behavior**</span></span>

<span data-ttu-id="1d649-509">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="1d649-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1d649-510">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-510">**Why**</span></span>

<span data-ttu-id="1d649-511">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="1d649-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1d649-512">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-512">**Mitigations**</span></span>

<span data-ttu-id="1d649-513">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="1d649-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1d649-514">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="1d649-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1d649-515">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="1d649-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1d649-516">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1d649-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1d649-517">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="1d649-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1d649-518">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="1d649-518">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d649-519">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-519">**Old behavior**</span></span>

<span data-ttu-id="1d649-520">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="1d649-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1d649-521">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-521">**New behavior**</span></span>

<span data-ttu-id="1d649-522">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="1d649-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1d649-523">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-523">**Why**</span></span>

<span data-ttu-id="1d649-524">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="1d649-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1d649-525">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-525">**Mitigations**</span></span>

<span data-ttu-id="1d649-526">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="1d649-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1d649-527">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="1d649-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1d649-528">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="1d649-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1d649-529">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="1d649-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1d649-530">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-530">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-531">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-531">**Old behavior**</span></span>

<span data-ttu-id="1d649-532">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="1d649-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1d649-533">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-533">**New behavior**</span></span>

<span data-ttu-id="1d649-534">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="1d649-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1d649-535">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-535">**Why**</span></span>

<span data-ttu-id="1d649-536">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="1d649-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1d649-537">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="1d649-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1d649-538">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-538">**Mitigations**</span></span>

<span data-ttu-id="1d649-539">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="1d649-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1d649-540">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="1d649-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1d649-541">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="1d649-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1d649-542">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-542">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-543">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-543">**Old behavior**</span></span>

<span data-ttu-id="1d649-544">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1d649-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1d649-545">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-545">**New behavior**</span></span>

<span data-ttu-id="1d649-546">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="1d649-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1d649-547">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1d649-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1d649-548">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-548">**Why**</span></span>

<span data-ttu-id="1d649-549">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="1d649-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1d649-550">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-550">**Mitigations**</span></span>

<span data-ttu-id="1d649-551">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="1d649-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1d649-552">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="1d649-552">Metadata API changes</span></span>

[<span data-ttu-id="1d649-553">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="1d649-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1d649-554">Ce changement sera introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-554">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-555">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-555">**New behavior**</span></span>

<span data-ttu-id="1d649-556">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="1d649-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1d649-557">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-557">**Why**</span></span>

<span data-ttu-id="1d649-558">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="1d649-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1d649-559">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-559">**Mitigations**</span></span>

<span data-ttu-id="1d649-560">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="1d649-560">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1d649-561">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="1d649-561">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1d649-562">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="1d649-562">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1d649-563">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="1d649-563">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d649-564">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-564">**Old behavior**</span></span>

<span data-ttu-id="1d649-565">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="1d649-565">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1d649-566">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-566">**New behavior**</span></span>

<span data-ttu-id="1d649-567">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="1d649-567">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1d649-568">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-568">**Why**</span></span>

<span data-ttu-id="1d649-569">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="1d649-569">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1d649-570">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-570">**Mitigations**</span></span>

<span data-ttu-id="1d649-571">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d649-571">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1d649-572">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="1d649-572">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1d649-573">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1d649-573">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1d649-574">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-574">**Old behavior**</span></span>

<span data-ttu-id="1d649-575">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1d649-575">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1d649-576">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-576">**New behavior**</span></span>

<span data-ttu-id="1d649-577">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1d649-577">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1d649-578">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-578">**Why**</span></span>

<span data-ttu-id="1d649-579">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="1d649-579">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1d649-580">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-580">**Mitigations**</span></span>

<span data-ttu-id="1d649-581">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="1d649-581">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1d649-582">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="1d649-582">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1d649-583">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="1d649-583">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1d649-584">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-584">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-585">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-585">**Old behavior**</span></span>

<span data-ttu-id="1d649-586">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d649-586">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1d649-587">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-587">**New behavior**</span></span>

<span data-ttu-id="1d649-588">Maintenant, les valeurs GUID sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="1d649-588">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="1d649-589">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-589">**Why**</span></span>

<span data-ttu-id="1d649-590">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="1d649-590">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1d649-591">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="1d649-591">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1d649-592">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-592">**Mitigations**</span></span>

<span data-ttu-id="1d649-593">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="1d649-593">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1d649-594">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="1d649-594">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1d649-595">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="1d649-595">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1d649-596">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="1d649-596">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1d649-597">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="1d649-597">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1d649-598">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-598">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-599">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-599">**Old behavior**</span></span>

<span data-ttu-id="1d649-600">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d649-600">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1d649-601">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="1d649-601">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1d649-602">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-602">**New behavior**</span></span>

<span data-ttu-id="1d649-603">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="1d649-603">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="1d649-604">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-604">**Why**</span></span>

<span data-ttu-id="1d649-605">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="1d649-605">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1d649-606">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-606">**Mitigations**</span></span>

<span data-ttu-id="1d649-607">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="1d649-607">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1d649-608">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="1d649-608">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1d649-609">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="1d649-609">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1d649-610">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="1d649-610">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1d649-611">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="1d649-611">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1d649-612">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-612">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-613">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-613">**Old behavior**</span></span>

<span data-ttu-id="1d649-614">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="1d649-614">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="1d649-615">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-615">**New behavior**</span></span>

<span data-ttu-id="1d649-616">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="1d649-616">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1d649-617">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-617">**Why**</span></span>

<span data-ttu-id="1d649-618">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="1d649-618">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1d649-619">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="1d649-619">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1d649-620">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-620">**Mitigations**</span></span>

<span data-ttu-id="1d649-621">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="1d649-621">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1d649-622">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="1d649-622">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1d649-623">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="1d649-623">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1d649-624">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="1d649-624">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1d649-625">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="1d649-625">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1d649-626">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="1d649-626">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1d649-627">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-627">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-628">**Changement**</span><span class="sxs-lookup"><span data-stu-id="1d649-628">**Change**</span></span>

<span data-ttu-id="1d649-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1d649-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1d649-630">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-630">**Why**</span></span>

<span data-ttu-id="1d649-631">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="1d649-631">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1d649-632">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-632">**Mitigations**</span></span>

<span data-ttu-id="1d649-633">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="1d649-633">Use the new name.</span></span> <span data-ttu-id="1d649-634">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="1d649-634">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1d649-635">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="1d649-635">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1d649-636">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="1d649-636">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1d649-637">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="1d649-637">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d649-638">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-638">**Old behavior**</span></span>

<span data-ttu-id="1d649-639">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="1d649-639">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1d649-640">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-640">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1d649-641">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="1d649-641">**New behavior**</span></span>

<span data-ttu-id="1d649-642">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="1d649-642">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="1d649-643">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d649-643">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1d649-644">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="1d649-644">**Why**</span></span>

<span data-ttu-id="1d649-645">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="1d649-645">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1d649-646">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="1d649-646">**Mitigations**</span></span>

<span data-ttu-id="1d649-647">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="1d649-647">Use the new name.</span></span>
