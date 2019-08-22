---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565311"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="33ce8-102">Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)</span><span class="sxs-lookup"><span data-stu-id="33ce8-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33ce8-103">Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.</span><span class="sxs-lookup"><span data-stu-id="33ce8-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="33ce8-104">Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="33ce8-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="33ce8-105">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="33ce8-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="33ce8-106">Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="33ce8-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="33ce8-107">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="33ce8-107">Summary</span></span>

| <span data-ttu-id="33ce8-108">**Modification critique**</span><span class="sxs-lookup"><span data-stu-id="33ce8-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="33ce8-109">**Impact**</span><span class="sxs-lookup"><span data-stu-id="33ce8-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="33ce8-110">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="33ce8-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="33ce8-111">Haute</span><span class="sxs-lookup"><span data-stu-id="33ce8-111">High</span></span>       |
| [<span data-ttu-id="33ce8-112">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="33ce8-113">Haute</span><span class="sxs-lookup"><span data-stu-id="33ce8-113">High</span></span>      |
| [<span data-ttu-id="33ce8-114">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="33ce8-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="33ce8-115">Haute</span><span class="sxs-lookup"><span data-stu-id="33ce8-115">High</span></span>      |
| [<span data-ttu-id="33ce8-116">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="33ce8-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="33ce8-117">Haute</span><span class="sxs-lookup"><span data-stu-id="33ce8-117">High</span></span>      |
| [<span data-ttu-id="33ce8-118">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="33ce8-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="33ce8-119">Haute</span><span class="sxs-lookup"><span data-stu-id="33ce8-119">High</span></span>      |
| [<span data-ttu-id="33ce8-120">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33ce8-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="33ce8-121">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-121">Medium</span></span>      |
| [<span data-ttu-id="33ce8-122">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="33ce8-123">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-123">Medium</span></span>      |
| [<span data-ttu-id="33ce8-124">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="33ce8-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="33ce8-125">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-125">Medium</span></span>      |
| [<span data-ttu-id="33ce8-126">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="33ce8-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="33ce8-127">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-127">Medium</span></span>      |
| [<span data-ttu-id="33ce8-128">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="33ce8-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="33ce8-129">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-129">Medium</span></span>      |
| [<span data-ttu-id="33ce8-130">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="33ce8-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="33ce8-131">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-131">Medium</span></span>      |
| [<span data-ttu-id="33ce8-132">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="33ce8-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="33ce8-133">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-133">Medium</span></span>      |
| [<span data-ttu-id="33ce8-134">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="33ce8-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="33ce8-135">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-135">Medium</span></span>      |
| [<span data-ttu-id="33ce8-136">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="33ce8-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="33ce8-137">Moyenne</span><span class="sxs-lookup"><span data-stu-id="33ce8-137">Medium</span></span>      |
| [<span data-ttu-id="33ce8-138">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="33ce8-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="33ce8-139">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-139">Low</span></span>      |
| [<span data-ttu-id="33ce8-140">~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli</span><span class="sxs-lookup"><span data-stu-id="33ce8-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="33ce8-141">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-141">Low</span></span>      |
| [<span data-ttu-id="33ce8-142">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="33ce8-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="33ce8-143">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-143">Low</span></span>      |
| [<span data-ttu-id="33ce8-144">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="33ce8-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="33ce8-145">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-145">Low</span></span>      |
| [<span data-ttu-id="33ce8-146">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="33ce8-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="33ce8-147">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-147">Low</span></span>      |
| [<span data-ttu-id="33ce8-148">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="33ce8-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="33ce8-149">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-149">Low</span></span>      |
| [<span data-ttu-id="33ce8-150">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="33ce8-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="33ce8-151">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-151">Low</span></span>      |
| [<span data-ttu-id="33ce8-152">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="33ce8-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="33ce8-153">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-153">Low</span></span>      |
| [<span data-ttu-id="33ce8-154">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="33ce8-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="33ce8-155">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-155">Low</span></span>      |
| [<span data-ttu-id="33ce8-156">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="33ce8-157">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-157">Low</span></span>      |
| [<span data-ttu-id="33ce8-158">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="33ce8-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="33ce8-159">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-159">Low</span></span>      |
| [<span data-ttu-id="33ce8-160">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="33ce8-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="33ce8-161">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-161">Low</span></span>      |
| [<span data-ttu-id="33ce8-162">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="33ce8-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="33ce8-163">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-163">Low</span></span>      |
| [<span data-ttu-id="33ce8-164">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="33ce8-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="33ce8-165">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-165">Low</span></span>      |
| [<span data-ttu-id="33ce8-166">Les clés de tableaux d’octets et de chaînes ne sont pas générées par client par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="33ce8-167">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-167">Low</span></span>      |
| [<span data-ttu-id="33ce8-168">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="33ce8-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="33ce8-169">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-169">Low</span></span>      |
| [<span data-ttu-id="33ce8-170">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="33ce8-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="33ce8-171">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-171">Low</span></span>      |
| [<span data-ttu-id="33ce8-172">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="33ce8-173">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-173">Low</span></span>      |
| [<span data-ttu-id="33ce8-174">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="33ce8-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="33ce8-175">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-175">Low</span></span>      |
| [<span data-ttu-id="33ce8-176">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="33ce8-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="33ce8-177">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-177">Low</span></span>      |
| [<span data-ttu-id="33ce8-178">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="33ce8-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="33ce8-179">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-179">Low</span></span>      |
| [<span data-ttu-id="33ce8-180">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="33ce8-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="33ce8-181">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-181">Low</span></span>      |
| [<span data-ttu-id="33ce8-182">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="33ce8-183">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-183">Low</span></span>      |
| [<span data-ttu-id="33ce8-184">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="33ce8-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="33ce8-185">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-185">Low</span></span>      |
| [<span data-ttu-id="33ce8-186">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="33ce8-187">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-187">Low</span></span>      |
| [<span data-ttu-id="33ce8-188">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="33ce8-189">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-189">Low</span></span>      |
| [<span data-ttu-id="33ce8-190">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="33ce8-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="33ce8-191">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-191">Low</span></span>      |
| [<span data-ttu-id="33ce8-192">Les infos/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="33ce8-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="33ce8-193">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-193">Low</span></span>      |
| [<span data-ttu-id="33ce8-194">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="33ce8-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="33ce8-195">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-195">Low</span></span>      |
| [<span data-ttu-id="33ce8-196">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="33ce8-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="33ce8-197">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-197">Low</span></span>      |
| [<span data-ttu-id="33ce8-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="33ce8-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="33ce8-199">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-199">Low</span></span>      |
| [<span data-ttu-id="33ce8-200">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="33ce8-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="33ce8-201">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-201">Low</span></span>      |
| [<span data-ttu-id="33ce8-202">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="33ce8-203">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-203">Low</span></span>      |
| [<span data-ttu-id="33ce8-204">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="33ce8-205">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-205">Low</span></span>      |
| [<span data-ttu-id="33ce8-206">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="33ce8-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="33ce8-207">Faible</span><span class="sxs-lookup"><span data-stu-id="33ce8-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="33ce8-208">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="33ce8-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="33ce8-209">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="33ce8-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="33ce8-210">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-211">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-211">**Old behavior**</span></span>

<span data-ttu-id="33ce8-212">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="33ce8-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="33ce8-213">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="33ce8-214">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-214">**New behavior**</span></span>

<span data-ttu-id="33ce8-215">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="33ce8-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="33ce8-216">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="33ce8-217">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-217">**Why**</span></span>

<span data-ttu-id="33ce8-218">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="33ce8-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="33ce8-219">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="33ce8-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="33ce8-220">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="33ce8-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="33ce8-221">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="33ce8-222">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="33ce8-223">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="33ce8-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="33ce8-224">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-224">**Mitigations**</span></span>

<span data-ttu-id="33ce8-225">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="33ce8-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="33ce8-226">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="33ce8-227">Suivi de problème no 15498</span><span class="sxs-lookup"><span data-stu-id="33ce8-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="33ce8-228">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-229">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-229">**Old behavior**</span></span>

<span data-ttu-id="33ce8-230">Avant la version 3.0, EF Core ciblait .NET Standard 2.0 et s’exécutait sur toutes les plateformes qui prennent en charge cette norme, y compris .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="33ce8-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="33ce8-231">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-231">**New behavior**</span></span>

<span data-ttu-id="33ce8-232">À partir de la version 3.0, EF Core cible .NET Standard 2.1 et s’exécute sur toutes les plateformes qui prennent en charge cette norme.</span><span class="sxs-lookup"><span data-stu-id="33ce8-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="33ce8-233">Cela n'inclut pas .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="33ce8-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="33ce8-234">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-234">**Why**</span></span>

<span data-ttu-id="33ce8-235">Cela fait partie d’une décision stratégique dans les technologies .NET de concentrer l’énergie sur .NET Core et d’autres plateformes .NET modernes, telles que Xamarin.</span><span class="sxs-lookup"><span data-stu-id="33ce8-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="33ce8-236">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-236">**Mitigations**</span></span>

<span data-ttu-id="33ce8-237">Envisagez de passer à une plateforme .NET moderne.</span><span class="sxs-lookup"><span data-stu-id="33ce8-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="33ce8-238">Si ce n’est pas possible, continuez à utiliser EF Core 2.1 ou EF Core 2.2, qui prennent tous deux en charge .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="33ce8-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="33ce8-239">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33ce8-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="33ce8-240">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="33ce8-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="33ce8-241">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="33ce8-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="33ce8-242">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-242">**Old behavior**</span></span>

<span data-ttu-id="33ce8-243">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="33ce8-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="33ce8-244">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-244">**New behavior**</span></span>

<span data-ttu-id="33ce8-245">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="33ce8-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="33ce8-246">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-246">**Why**</span></span>

<span data-ttu-id="33ce8-247">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="33ce8-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="33ce8-248">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="33ce8-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="33ce8-249">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="33ce8-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="33ce8-250">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="33ce8-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="33ce8-251">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-251">**Mitigations**</span></span>

<span data-ttu-id="33ce8-252">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="33ce8-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="33ce8-253">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="33ce8-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="33ce8-254">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="33ce8-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="33ce8-255">Ce changement a été introduit dans EF Core 3.0-preview 4 et la version correspondante du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="33ce8-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="33ce8-256">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-256">**Old behavior**</span></span>

<span data-ttu-id="33ce8-257">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="33ce8-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="33ce8-258">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-258">**New behavior**</span></span>

<span data-ttu-id="33ce8-259">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="33ce8-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="33ce8-260">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-260">**Why**</span></span>

<span data-ttu-id="33ce8-261">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="33ce8-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="33ce8-262">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-262">**Mitigations**</span></span>

<span data-ttu-id="33ce8-263">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` comme outil général :</span><span class="sxs-lookup"><span data-stu-id="33ce8-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="33ce8-264">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="33ce8-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="33ce8-265">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="33ce8-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="33ce8-266">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="33ce8-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="33ce8-267">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-268">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-268">**Old behavior**</span></span>

<span data-ttu-id="33ce8-269">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="33ce8-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="33ce8-270">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-270">**New behavior**</span></span>

<span data-ttu-id="33ce8-271">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="33ce8-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="33ce8-272">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="33ce8-273">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="33ce8-274">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="33ce8-275">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="33ce8-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="33ce8-276">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-276">**Why**</span></span>

<span data-ttu-id="33ce8-277">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="33ce8-278">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="33ce8-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="33ce8-279">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-279">**Mitigations**</span></span>

<span data-ttu-id="33ce8-280">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="33ce8-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="33ce8-281">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="33ce8-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="33ce8-282">Suivi de problème no 15704</span><span class="sxs-lookup"><span data-stu-id="33ce8-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="33ce8-283">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33ce8-284">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-284">**Old behavior**</span></span>

<span data-ttu-id="33ce8-285">Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.</span><span class="sxs-lookup"><span data-stu-id="33ce8-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="33ce8-286">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-286">**New behavior**</span></span>

<span data-ttu-id="33ce8-287">À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="33ce8-288">Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="33ce8-289">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-289">**Why**</span></span>

<span data-ttu-id="33ce8-290">La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="33ce8-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="33ce8-291">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-291">**Mitigations**</span></span>

<span data-ttu-id="33ce8-292">Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="33ce8-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="33ce8-293">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="33ce8-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="33ce8-294">Suivi de problème no 13518</span><span class="sxs-lookup"><span data-stu-id="33ce8-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="33ce8-295">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33ce8-296">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-296">**Old behavior**</span></span>

<span data-ttu-id="33ce8-297">Avant EF Core 3.0, la même instance d’entité était utilisée pour chaque occurrence d’une entité avec un type et un ID donnés.</span><span class="sxs-lookup"><span data-stu-id="33ce8-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="33ce8-298">Cela correspond au comportement des requêtes de suivi.</span><span class="sxs-lookup"><span data-stu-id="33ce8-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="33ce8-299">Par exemple, cette requête :</span><span class="sxs-lookup"><span data-stu-id="33ce8-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="33ce8-300">retournait la même instance `Category` pour chaque `Product` associé à la catégorie donnée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="33ce8-301">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-301">**New behavior**</span></span>

<span data-ttu-id="33ce8-302">À compter de EF Core 3.0, différentes instances d’entité sont créées lorsqu’une entité avec un type et un ID donnés est rencontrée à différents emplacements dans le graphe retourné.</span><span class="sxs-lookup"><span data-stu-id="33ce8-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="33ce8-303">Par exemple, la requête ci-dessus retourne à présent une nouvelle instance `Category` pour chaque `Product` même si deux produits sont associés à la même catégorie.</span><span class="sxs-lookup"><span data-stu-id="33ce8-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="33ce8-304">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-304">**Why**</span></span>

<span data-ttu-id="33ce8-305">La résolution de l’identité (autrement dit, le fait de déterminer qu’une entité a les mêmes type et ID qu’une entité précédemment rencontrée) améliore les performances et la surcharge de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="33ce8-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="33ce8-306">Cela agit généralement à l’opposé de la raison pour laquelle aucune requête de suivi n’est utilisée en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="33ce8-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="33ce8-307">En outre, même si la résolution de l’identité peut parfois être utile, elle n’est pas nécessaire si les entités doivent être sérialisées et envoyées à un client, ce qui est courant pour les requêtes sans suivi.</span><span class="sxs-lookup"><span data-stu-id="33ce8-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="33ce8-308">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-308">**Mitigations**</span></span>

<span data-ttu-id="33ce8-309">Utilisez une requête de suivi si la résolution de l’identité est requise.</span><span class="sxs-lookup"><span data-stu-id="33ce8-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="33ce8-310">~~L’exécution de requêtes est enregistrée au niveau du débogage~~ rétabli</span><span class="sxs-lookup"><span data-stu-id="33ce8-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="33ce8-311">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="33ce8-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="33ce8-312">Ce changement a été rétabli dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-313">Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application.</span><span class="sxs-lookup"><span data-stu-id="33ce8-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="33ce8-314">Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :</span><span class="sxs-lookup"><span data-stu-id="33ce8-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="33ce8-315">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="33ce8-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="33ce8-316">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="33ce8-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="33ce8-317">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33ce8-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33ce8-318">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-318">**Old behavior**</span></span>

<span data-ttu-id="33ce8-319">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="33ce8-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="33ce8-320">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="33ce8-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="33ce8-321">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-321">**New behavior**</span></span>

<span data-ttu-id="33ce8-322">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="33ce8-323">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-323">**Why**</span></span>

<span data-ttu-id="33ce8-324">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="33ce8-325">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-325">**Mitigations**</span></span>

<span data-ttu-id="33ce8-326">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="33ce8-327">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="33ce8-327">This can be avoided by:</span></span>
* <span data-ttu-id="33ce8-328">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="33ce8-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="33ce8-329">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="33ce8-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="33ce8-330">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="33ce8-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="33ce8-331">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="33ce8-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="33ce8-332">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="33ce8-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="33ce8-333">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="33ce8-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="33ce8-334">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-335">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-335">**Old behavior**</span></span>

<span data-ttu-id="33ce8-336">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="33ce8-337">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-337">**New behavior**</span></span>

<span data-ttu-id="33ce8-338">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="33ce8-339">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="33ce8-340">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="33ce8-341">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-341">**Why**</span></span>

<span data-ttu-id="33ce8-342">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="33ce8-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="33ce8-343">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-343">**Mitigations**</span></span>

<span data-ttu-id="33ce8-344">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="33ce8-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="33ce8-345">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="33ce8-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="33ce8-346">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="33ce8-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="33ce8-347">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="33ce8-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="33ce8-348">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="33ce8-349">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="33ce8-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="33ce8-350">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-351">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-351">**Old behavior**</span></span>

<span data-ttu-id="33ce8-352">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="33ce8-353">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-353">**New behavior**</span></span>

<span data-ttu-id="33ce8-354">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="33ce8-355">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="33ce8-356">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-356">**Why**</span></span>

<span data-ttu-id="33ce8-357">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="33ce8-358">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-358">**Mitigations**</span></span>

<span data-ttu-id="33ce8-359">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="33ce8-360">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="33ce8-361">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="33ce8-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="33ce8-362">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="33ce8-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="33ce8-363">Ce changement a été introduit dans EF Core 3.0-preview 5.</span><span class="sxs-lookup"><span data-stu-id="33ce8-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="33ce8-364">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-364">**Old behavior**</span></span>

<span data-ttu-id="33ce8-365">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="33ce8-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="33ce8-366">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-366">**New behavior**</span></span>

<span data-ttu-id="33ce8-367">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="33ce8-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="33ce8-368">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-368">**Why**</span></span>

<span data-ttu-id="33ce8-369">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="33ce8-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="33ce8-370">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-370">**Mitigations**</span></span>

<span data-ttu-id="33ce8-371">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="33ce8-372">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="33ce8-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="33ce8-373">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="33ce8-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="33ce8-374">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-375">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-375">**Old behavior**</span></span>

<span data-ttu-id="33ce8-376">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="33ce8-377">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="33ce8-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="33ce8-378">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-378">**New behavior**</span></span>

<span data-ttu-id="33ce8-379">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33ce8-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="33ce8-380">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="33ce8-381">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-381">**Why**</span></span>

<span data-ttu-id="33ce8-382">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="33ce8-383">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="33ce8-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="33ce8-384">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="33ce8-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="33ce8-385">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-385">**Mitigations**</span></span>

<span data-ttu-id="33ce8-386">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="33ce8-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="33ce8-387">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="33ce8-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="33ce8-388">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="33ce8-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="33ce8-389">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="33ce8-390">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="33ce8-391">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="33ce8-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="33ce8-392">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="33ce8-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="33ce8-393">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-394">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-394">**Old behavior**</span></span>

<span data-ttu-id="33ce8-395">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="33ce8-396">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-396">**New behavior**</span></span>

<span data-ttu-id="33ce8-397">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="33ce8-398">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="33ce8-399">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="33ce8-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="33ce8-400">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="33ce8-401">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-401">For example:</span></span>

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

<span data-ttu-id="33ce8-402">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="33ce8-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="33ce8-403">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-403">**Why**</span></span>

<span data-ttu-id="33ce8-404">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="33ce8-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="33ce8-405">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="33ce8-406">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-406">**Mitigations**</span></span>

<span data-ttu-id="33ce8-407">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="33ce8-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="33ce8-408">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="33ce8-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="33ce8-409">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="33ce8-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="33ce8-410">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-411">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-411">**Old behavior**</span></span>

<span data-ttu-id="33ce8-412">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="33ce8-412">Consider the following model:</span></span>
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
<span data-ttu-id="33ce8-413">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="33ce8-414">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-414">**New behavior**</span></span>

<span data-ttu-id="33ce8-415">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="33ce8-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="33ce8-416">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="33ce8-417">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-417">**Mitigations**</span></span>

<span data-ttu-id="33ce8-418">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="33ce8-419">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="33ce8-420">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="33ce8-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="33ce8-421">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="33ce8-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="33ce8-422">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-423">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-423">**Old behavior**</span></span>

<span data-ttu-id="33ce8-424">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="33ce8-424">Consider the following model:</span></span>
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
<span data-ttu-id="33ce8-425">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="33ce8-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="33ce8-426">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-426">**New behavior**</span></span>

<span data-ttu-id="33ce8-427">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="33ce8-428">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="33ce8-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="33ce8-429">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-429">**Why**</span></span>

<span data-ttu-id="33ce8-430">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="33ce8-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="33ce8-431">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-431">**Mitigations**</span></span>

<span data-ttu-id="33ce8-432">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="33ce8-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="33ce8-433">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="33ce8-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="33ce8-434">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="33ce8-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="33ce8-435">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="33ce8-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="33ce8-436">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-437">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-437">**Old behavior**</span></span>

<span data-ttu-id="33ce8-438">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="33ce8-438">Consider the following model:</span></span>
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

<span data-ttu-id="33ce8-439">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="33ce8-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="33ce8-440">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-440">**New behavior**</span></span>

<span data-ttu-id="33ce8-441">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="33ce8-442">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-442">**Why**</span></span>

<span data-ttu-id="33ce8-443">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="33ce8-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="33ce8-444">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-444">**Mitigations**</span></span>

<span data-ttu-id="33ce8-445">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="33ce8-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="33ce8-446">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="33ce8-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="33ce8-447">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="33ce8-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="33ce8-448">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-449">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-449">**Old behavior**</span></span>

<span data-ttu-id="33ce8-450">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="33ce8-450">Consider the following model:</span></span>
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
<span data-ttu-id="33ce8-451">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="33ce8-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="33ce8-452">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="33ce8-453">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-453">**New behavior**</span></span>

<span data-ttu-id="33ce8-454">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="33ce8-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="33ce8-455">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="33ce8-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="33ce8-456">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-456">For example:</span></span>

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

<span data-ttu-id="33ce8-457">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-457">**Why**</span></span>

<span data-ttu-id="33ce8-458">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="33ce8-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="33ce8-459">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-459">**Mitigations**</span></span>

<span data-ttu-id="33ce8-460">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="33ce8-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="33ce8-461">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="33ce8-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="33ce8-462">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="33ce8-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="33ce8-463">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-464">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-464">**Old behavior**</span></span>

<span data-ttu-id="33ce8-465">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="33ce8-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="33ce8-466">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-466">**New behavior**</span></span>

<span data-ttu-id="33ce8-467">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="33ce8-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="33ce8-468">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-468">**Why**</span></span>

<span data-ttu-id="33ce8-469">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="33ce8-470">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="33ce8-471">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-471">**Mitigations**</span></span>

<span data-ttu-id="33ce8-472">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="33ce8-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="33ce8-473">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="33ce8-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="33ce8-474">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="33ce8-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="33ce8-475">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-476">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-476">**Old behavior**</span></span>

<span data-ttu-id="33ce8-477">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="33ce8-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="33ce8-478">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-478">**New behavior**</span></span>

<span data-ttu-id="33ce8-479">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="33ce8-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="33ce8-480">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="33ce8-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="33ce8-481">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-481">**Why**</span></span>

<span data-ttu-id="33ce8-482">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="33ce8-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="33ce8-483">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-483">**Mitigations**</span></span>

<span data-ttu-id="33ce8-484">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="33ce8-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="33ce8-485">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="33ce8-486">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-486">Backing fields are used by default</span></span>

[<span data-ttu-id="33ce8-487">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="33ce8-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="33ce8-488">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33ce8-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33ce8-489">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-489">**Old behavior**</span></span>

<span data-ttu-id="33ce8-490">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="33ce8-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="33ce8-491">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="33ce8-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="33ce8-492">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-492">**New behavior**</span></span>

<span data-ttu-id="33ce8-493">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="33ce8-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="33ce8-494">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="33ce8-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="33ce8-495">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-495">**Why**</span></span>

<span data-ttu-id="33ce8-496">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="33ce8-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="33ce8-497">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-497">**Mitigations**</span></span>

<span data-ttu-id="33ce8-498">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="33ce8-499">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="33ce8-500">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="33ce8-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="33ce8-501">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="33ce8-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="33ce8-502">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-503">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-503">**Old behavior**</span></span>

<span data-ttu-id="33ce8-504">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="33ce8-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="33ce8-505">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="33ce8-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="33ce8-506">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-506">**New behavior**</span></span>

<span data-ttu-id="33ce8-507">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="33ce8-508">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-508">**Why**</span></span>

<span data-ttu-id="33ce8-509">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="33ce8-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="33ce8-510">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-510">**Mitigations**</span></span>

<span data-ttu-id="33ce8-511">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="33ce8-512">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="33ce8-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="33ce8-513">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="33ce8-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="33ce8-514">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-515">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-515">**Old behavior**</span></span>

<span data-ttu-id="33ce8-516">Avant EF Core 3.0, une propriété pouvait être spécifiée par une valeur de chaîne, et si aucune propriété portant ce nom n’était trouvée dans le type CLR, EF Core tentait de la comparer à un champ à l’aide de règles de nommage.</span><span class="sxs-lookup"><span data-stu-id="33ce8-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="33ce8-517">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-517">**New behavior**</span></span>

<span data-ttu-id="33ce8-518">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="33ce8-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="33ce8-519">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-519">**Why**</span></span>

<span data-ttu-id="33ce8-520">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="33ce8-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="33ce8-521">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-521">**Mitigations**</span></span>

<span data-ttu-id="33ce8-522">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="33ce8-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="33ce8-523">Dans une future préversion d’EF Core 3.0, nous prévoyons de rendre de nouveau possible la configuration explicite d’un nom de champ qui est différent du nom de la propriété :</span><span class="sxs-lookup"><span data-stu-id="33ce8-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="33ce8-524">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="33ce8-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="33ce8-525">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="33ce8-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="33ce8-526">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-527">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-527">**Old behavior**</span></span>

<span data-ttu-id="33ce8-528">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="33ce8-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="33ce8-529">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-529">**New behavior**</span></span>

<span data-ttu-id="33ce8-530">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="33ce8-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="33ce8-531">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-531">**Why**</span></span>

<span data-ttu-id="33ce8-532">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="33ce8-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="33ce8-533">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="33ce8-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="33ce8-534">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-534">**Mitigations**</span></span>

<span data-ttu-id="33ce8-535">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="33ce8-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="33ce8-536">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="33ce8-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="33ce8-537">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="33ce8-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="33ce8-538">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-539">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-539">**Old behavior**</span></span>

<span data-ttu-id="33ce8-540">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="33ce8-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="33ce8-541">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="33ce8-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="33ce8-542">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-542">**New behavior**</span></span>

<span data-ttu-id="33ce8-543">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="33ce8-544">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="33ce8-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="33ce8-545">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="33ce8-546">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="33ce8-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="33ce8-547">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-547">**Why**</span></span>

<span data-ttu-id="33ce8-548">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="33ce8-549">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-549">**Mitigations**</span></span>

<span data-ttu-id="33ce8-550">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="33ce8-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="33ce8-551">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="33ce8-552">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="33ce8-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="33ce8-553">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-554">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-554">**Old behavior**</span></span>

<span data-ttu-id="33ce8-555">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="33ce8-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="33ce8-556">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="33ce8-557">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-557">**New behavior**</span></span>

<span data-ttu-id="33ce8-558">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="33ce8-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="33ce8-559">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-559">**Why**</span></span>

<span data-ttu-id="33ce8-560">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="33ce8-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="33ce8-561">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-561">**Mitigations**</span></span>

<span data-ttu-id="33ce8-562">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="33ce8-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="33ce8-563">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="33ce8-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="33ce8-564">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="33ce8-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="33ce8-565">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="33ce8-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="33ce8-566">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="33ce8-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="33ce8-567">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-568">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-568">**Old behavior**</span></span>

<span data-ttu-id="33ce8-569">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="33ce8-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="33ce8-570">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-570">**New behavior**</span></span>

<span data-ttu-id="33ce8-571">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="33ce8-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="33ce8-572">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-572">**Why**</span></span>

<span data-ttu-id="33ce8-573">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="33ce8-574">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-574">**Mitigations**</span></span>

<span data-ttu-id="33ce8-575">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="33ce8-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="33ce8-576">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="33ce8-576">This isn't common.</span></span>
<span data-ttu-id="33ce8-577">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="33ce8-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="33ce8-578">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="33ce8-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="33ce8-579">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="33ce8-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="33ce8-580">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="33ce8-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="33ce8-581">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-582">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-582">**Old behavior**</span></span>

<span data-ttu-id="33ce8-583">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="33ce8-584">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="33ce8-585">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="33ce8-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="33ce8-586">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-586">**New behavior**</span></span>

<span data-ttu-id="33ce8-587">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="33ce8-588">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="33ce8-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="33ce8-589">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="33ce8-590">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="33ce8-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="33ce8-591">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-591">**Why**</span></span>

<span data-ttu-id="33ce8-592">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="33ce8-593">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-593">**Mitigations**</span></span>

<span data-ttu-id="33ce8-594">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="33ce8-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="33ce8-595">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="33ce8-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="33ce8-596">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="33ce8-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="33ce8-597">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-598">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-598">**Old behavior**</span></span>

<span data-ttu-id="33ce8-599">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="33ce8-600">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-600">**New behavior**</span></span>

<span data-ttu-id="33ce8-601">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="33ce8-602">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-602">**Why**</span></span>

<span data-ttu-id="33ce8-603">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="33ce8-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="33ce8-604">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-604">**Mitigations**</span></span>

<span data-ttu-id="33ce8-605">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="33ce8-606">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="33ce8-607">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="33ce8-608">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="33ce8-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="33ce8-609">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="33ce8-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="33ce8-610">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-611">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-611">**Old behavior**</span></span>

<span data-ttu-id="33ce8-612">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="33ce8-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="33ce8-613">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="33ce8-614">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="33ce8-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="33ce8-615">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="33ce8-616">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-616">**New behavior**</span></span>

<span data-ttu-id="33ce8-617">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="33ce8-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="33ce8-618">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-618">**Why**</span></span>

<span data-ttu-id="33ce8-619">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="33ce8-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="33ce8-620">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-620">**Mitigations**</span></span>

<span data-ttu-id="33ce8-621">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="33ce8-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="33ce8-622">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="33ce8-622">This is not common.</span></span>
<span data-ttu-id="33ce8-623">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="33ce8-624">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="33ce8-625">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="33ce8-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="33ce8-626">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="33ce8-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="33ce8-627">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-628">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-628">**Old behavior**</span></span>

<span data-ttu-id="33ce8-629">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="33ce8-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="33ce8-630">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="33ce8-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="33ce8-631">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-631">**New behavior**</span></span>

<span data-ttu-id="33ce8-632">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="33ce8-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="33ce8-633">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-633">**Why**</span></span>

<span data-ttu-id="33ce8-634">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="33ce8-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="33ce8-635">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-635">**Mitigations**</span></span>

<span data-ttu-id="33ce8-636">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="33ce8-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="33ce8-637">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="33ce8-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="33ce8-638">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="33ce8-639">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="33ce8-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="33ce8-640">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="33ce8-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="33ce8-641">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="33ce8-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="33ce8-642">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-642">**Old behavior**</span></span>

<span data-ttu-id="33ce8-643">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="33ce8-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="33ce8-644">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-644">**New behavior**</span></span>

<span data-ttu-id="33ce8-645">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="33ce8-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="33ce8-646">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-646">**Why**</span></span>

<span data-ttu-id="33ce8-647">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="33ce8-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="33ce8-648">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-648">**Mitigations**</span></span>

<span data-ttu-id="33ce8-649">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="33ce8-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="33ce8-650">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="33ce8-651">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="33ce8-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="33ce8-652">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="33ce8-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="33ce8-653">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-654">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-654">**Old behavior**</span></span>

<span data-ttu-id="33ce8-655">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="33ce8-656">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-656">**New behavior**</span></span>

<span data-ttu-id="33ce8-657">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="33ce8-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="33ce8-658">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-658">**Why**</span></span>

<span data-ttu-id="33ce8-659">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="33ce8-660">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="33ce8-661">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-661">**Mitigations**</span></span>

<span data-ttu-id="33ce8-662">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="33ce8-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="33ce8-663">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="33ce8-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="33ce8-664">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="33ce8-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="33ce8-665">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-666">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-666">**Old behavior**</span></span>

<span data-ttu-id="33ce8-667">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="33ce8-668">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-668">**New behavior**</span></span>

<span data-ttu-id="33ce8-669">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="33ce8-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="33ce8-670">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="33ce8-671">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-671">**Why**</span></span>

<span data-ttu-id="33ce8-672">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="33ce8-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="33ce8-673">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-673">**Mitigations**</span></span>

<span data-ttu-id="33ce8-674">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="33ce8-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="33ce8-675">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="33ce8-675">Metadata API changes</span></span>

[<span data-ttu-id="33ce8-676">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="33ce8-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="33ce8-677">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-678">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-678">**New behavior**</span></span>

<span data-ttu-id="33ce8-679">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="33ce8-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="33ce8-680">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-680">**Why**</span></span>

<span data-ttu-id="33ce8-681">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="33ce8-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="33ce8-682">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-682">**Mitigations**</span></span>

<span data-ttu-id="33ce8-683">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="33ce8-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="33ce8-684">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="33ce8-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="33ce8-685">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="33ce8-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="33ce8-686">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33ce8-687">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-687">**New behavior**</span></span>

<span data-ttu-id="33ce8-688">Les méthodes d’extension spécifiques au fournisseur seront aplaties :</span><span class="sxs-lookup"><span data-stu-id="33ce8-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="33ce8-689">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-689">**Why**</span></span>

<span data-ttu-id="33ce8-690">Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="33ce8-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="33ce8-691">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-691">**Mitigations**</span></span>

<span data-ttu-id="33ce8-692">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="33ce8-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="33ce8-693">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="33ce8-694">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="33ce8-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="33ce8-695">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="33ce8-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="33ce8-696">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-696">**Old behavior**</span></span>

<span data-ttu-id="33ce8-697">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="33ce8-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="33ce8-698">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-698">**New behavior**</span></span>

<span data-ttu-id="33ce8-699">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="33ce8-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="33ce8-700">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-700">**Why**</span></span>

<span data-ttu-id="33ce8-701">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="33ce8-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="33ce8-702">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-702">**Mitigations**</span></span>

<span data-ttu-id="33ce8-703">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="33ce8-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="33ce8-704">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="33ce8-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="33ce8-705">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="33ce8-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="33ce8-706">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-706">**Old behavior**</span></span>

<span data-ttu-id="33ce8-707">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="33ce8-708">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-708">**New behavior**</span></span>

<span data-ttu-id="33ce8-709">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="33ce8-710">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-710">**Why**</span></span>

<span data-ttu-id="33ce8-711">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="33ce8-712">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-712">**Mitigations**</span></span>

<span data-ttu-id="33ce8-713">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="33ce8-714">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="33ce8-715">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="33ce8-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="33ce8-716">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-717">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-717">**Old behavior**</span></span>

<span data-ttu-id="33ce8-718">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="33ce8-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="33ce8-719">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-719">**New behavior**</span></span>

<span data-ttu-id="33ce8-720">Maintenant, les valeurs Guid sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="33ce8-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="33ce8-721">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-721">**Why**</span></span>

<span data-ttu-id="33ce8-722">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="33ce8-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="33ce8-723">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="33ce8-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="33ce8-724">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-724">**Mitigations**</span></span>

<span data-ttu-id="33ce8-725">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="33ce8-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="33ce8-726">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="33ce8-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="33ce8-727">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="33ce8-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="33ce8-728">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="33ce8-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="33ce8-729">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="33ce8-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="33ce8-730">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-731">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-731">**Old behavior**</span></span>

<span data-ttu-id="33ce8-732">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="33ce8-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="33ce8-733">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="33ce8-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="33ce8-734">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-734">**New behavior**</span></span>

<span data-ttu-id="33ce8-735">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="33ce8-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="33ce8-736">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-736">**Why**</span></span>

<span data-ttu-id="33ce8-737">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="33ce8-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="33ce8-738">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-738">**Mitigations**</span></span>

<span data-ttu-id="33ce8-739">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="33ce8-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="33ce8-740">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="33ce8-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="33ce8-741">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="33ce8-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="33ce8-742">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="33ce8-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="33ce8-743">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="33ce8-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="33ce8-744">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-745">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-745">**Old behavior**</span></span>

<span data-ttu-id="33ce8-746">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="33ce8-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="33ce8-747">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-747">**New behavior**</span></span>

<span data-ttu-id="33ce8-748">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="33ce8-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="33ce8-749">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-749">**Why**</span></span>

<span data-ttu-id="33ce8-750">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="33ce8-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="33ce8-751">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="33ce8-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="33ce8-752">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-752">**Mitigations**</span></span>

<span data-ttu-id="33ce8-753">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="33ce8-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="33ce8-754">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="33ce8-755">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="33ce8-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="33ce8-756">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="33ce8-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="33ce8-757">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="33ce8-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="33ce8-758">Suivi de problème no 16400</span><span class="sxs-lookup"><span data-stu-id="33ce8-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="33ce8-759">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33ce8-760">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-760">**Old behavior**</span></span>

<span data-ttu-id="33ce8-761">Avant EF Core 3.0, `UseRowNumberForPaging` pouvait être utilisé pour générer SQL pour la pagination qui est compatible avec SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="33ce8-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="33ce8-762">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-762">**New behavior**</span></span>

<span data-ttu-id="33ce8-763">À compter de EF Core 3.0, EF génère uniquement SQL pour la pagination qui est uniquement compatible avec les versions de SQL Server ultérieures.</span><span class="sxs-lookup"><span data-stu-id="33ce8-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="33ce8-764">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-764">**Why**</span></span>

<span data-ttu-id="33ce8-765">Nous effectuons cette modification, car [SQL Server 2008 n’est plus un produit pris en charge](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) et la mise à jour de cette fonctionnalité pour fonctionner avec les modifications de requête effectuées dans EF Core 3.0 est un travail significatif.</span><span class="sxs-lookup"><span data-stu-id="33ce8-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="33ce8-766">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-766">**Mitigations**</span></span>

<span data-ttu-id="33ce8-767">Nous vous recommandons d’effectuer la mise à jour vers une version plus récente de SQL Server ou d’utiliser un niveau de compatibilité plus élevé, afin que le SQL généré soit pris en charge.</span><span class="sxs-lookup"><span data-stu-id="33ce8-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="33ce8-768">Cela dit, si vous ne pouvez pas faire cela, veuillez [commenter le problème de suivi](https://github.com/aspnet/EntityFrameworkCore/issues/16400) en incluant des détails.</span><span class="sxs-lookup"><span data-stu-id="33ce8-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="33ce8-769">Nous pourrions revisiter cette décision en fonction des commentaires.</span><span class="sxs-lookup"><span data-stu-id="33ce8-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="33ce8-770">Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="33ce8-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="33ce8-771">Suivi du problème no 16119</span><span class="sxs-lookup"><span data-stu-id="33ce8-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="33ce8-772">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-773">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-773">**Old behavior**</span></span>

<span data-ttu-id="33ce8-774">`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.</span><span class="sxs-lookup"><span data-stu-id="33ce8-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="33ce8-775">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-775">**New behavior**</span></span>

<span data-ttu-id="33ce8-776">Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="33ce8-777">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-777">**Why**</span></span>

<span data-ttu-id="33ce8-778">Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="33ce8-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="33ce8-779">Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.</span><span class="sxs-lookup"><span data-stu-id="33ce8-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="33ce8-780">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-780">**Mitigations**</span></span>

<span data-ttu-id="33ce8-781">Mettre à jour des extensions pour suivre le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="33ce8-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="33ce8-782">Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.</span><span class="sxs-lookup"><span data-stu-id="33ce8-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="33ce8-783">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="33ce8-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="33ce8-784">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="33ce8-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="33ce8-785">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-786">**Changement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-786">**Change**</span></span>

<span data-ttu-id="33ce8-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="33ce8-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="33ce8-788">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-788">**Why**</span></span>

<span data-ttu-id="33ce8-789">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="33ce8-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="33ce8-790">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-790">**Mitigations**</span></span>

<span data-ttu-id="33ce8-791">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="33ce8-791">Use the new name.</span></span> <span data-ttu-id="33ce8-792">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="33ce8-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="33ce8-793">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="33ce8-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="33ce8-794">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="33ce8-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="33ce8-795">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-796">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-796">**Old behavior**</span></span>

<span data-ttu-id="33ce8-797">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="33ce8-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="33ce8-798">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="33ce8-799">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-799">**New behavior**</span></span>

<span data-ttu-id="33ce8-800">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="33ce8-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="33ce8-801">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="33ce8-802">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-802">**Why**</span></span>

<span data-ttu-id="33ce8-803">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="33ce8-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="33ce8-804">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-804">**Mitigations**</span></span>

<span data-ttu-id="33ce8-805">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="33ce8-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="33ce8-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="33ce8-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="33ce8-807">Suivi du problème no 15997</span><span class="sxs-lookup"><span data-stu-id="33ce8-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="33ce8-808">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-809">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-809">**Old behavior**</span></span>

<span data-ttu-id="33ce8-810">Avant EF Core 3.0, ces méthodes étaient protégées.</span><span class="sxs-lookup"><span data-stu-id="33ce8-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="33ce8-811">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-811">**New behavior**</span></span>

<span data-ttu-id="33ce8-812">À compter d’EF Core 3.0, ces méthodes sont publiques.</span><span class="sxs-lookup"><span data-stu-id="33ce8-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="33ce8-813">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-813">**Why**</span></span>

<span data-ttu-id="33ce8-814">Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide.</span><span class="sxs-lookup"><span data-stu-id="33ce8-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="33ce8-815">Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.</span><span class="sxs-lookup"><span data-stu-id="33ce8-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="33ce8-816">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-816">**Mitigations**</span></span>

<span data-ttu-id="33ce8-817">Modifiez l’accessibilité de n’importe quel remplacements.</span><span class="sxs-lookup"><span data-stu-id="33ce8-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="33ce8-818">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="33ce8-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="33ce8-819">Suivi du problème no 11506</span><span class="sxs-lookup"><span data-stu-id="33ce8-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="33ce8-820">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="33ce8-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="33ce8-821">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-821">**Old behavior**</span></span>

<span data-ttu-id="33ce8-822">Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.</span><span class="sxs-lookup"><span data-stu-id="33ce8-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="33ce8-823">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-823">**New behavior**</span></span>

<span data-ttu-id="33ce8-824">À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="33ce8-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="33ce8-825">Ce qui signifie que la dépendance ne circule pas de manière transitive dans d’autres projets et que vous ne pouvez plus, par défaut, faire référence à son assembly.</span><span class="sxs-lookup"><span data-stu-id="33ce8-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="33ce8-826">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-826">**Why**</span></span>

<span data-ttu-id="33ce8-827">Ce package est uniquement destiné à être utilisé au moment du design.</span><span class="sxs-lookup"><span data-stu-id="33ce8-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="33ce8-828">Les applications déployées ne doivent pas y faire référence.</span><span class="sxs-lookup"><span data-stu-id="33ce8-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="33ce8-829">Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="33ce8-830">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-830">**Mitigations**</span></span>

<span data-ttu-id="33ce8-831">Si vous devez référencer ce package pour écraser le comportement au moment du design d’EF Core, vous pouvez mettre à jour des métadonnées d’élément PackageReference dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="33ce8-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="33ce8-832">Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées.</span><span class="sxs-lookup"><span data-stu-id="33ce8-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="33ce8-833">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="33ce8-834">Suivi du problème no 14824</span><span class="sxs-lookup"><span data-stu-id="33ce8-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="33ce8-835">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-836">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-836">**Old behavior**</span></span>

<span data-ttu-id="33ce8-837">Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="33ce8-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="33ce8-838">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-838">**New behavior**</span></span>

<span data-ttu-id="33ce8-839">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="33ce8-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="33ce8-840">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-840">**Why**</span></span>

<span data-ttu-id="33ce8-841">La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="33ce8-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="33ce8-842">Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="33ce8-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="33ce8-843">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-843">**Mitigations**</span></span>

<span data-ttu-id="33ce8-844">SQLitePCL.raw version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="33ce8-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="33ce8-845">Pour plus d’informations, consultez les [notes de publication](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="33ce8-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="33ce8-846">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="33ce8-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="33ce8-847">Suivi de problème no 14825</span><span class="sxs-lookup"><span data-stu-id="33ce8-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="33ce8-848">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="33ce8-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="33ce8-849">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-849">**Old behavior**</span></span>

<span data-ttu-id="33ce8-850">Les packages spatiaux dépendaient précédemment de la version 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="33ce8-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="33ce8-851">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-851">**New behavior**</span></span>

<span data-ttu-id="33ce8-852">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="33ce8-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="33ce8-853">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-853">**Why**</span></span>

<span data-ttu-id="33ce8-854">La version 2.0.0 de NetTopologySuite vise à résoudre plusieurs problèmes de facilité d’utilisation rencontrés par les utilisateurs EF Core.</span><span class="sxs-lookup"><span data-stu-id="33ce8-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="33ce8-855">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-855">**Mitigations**</span></span>

<span data-ttu-id="33ce8-856">NetTopologySuite version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="33ce8-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="33ce8-857">Pour plus d’informations, consultez les [notes de publication](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="33ce8-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="33ce8-858">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="33ce8-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="33ce8-859">Suivi de problème no 13573</span><span class="sxs-lookup"><span data-stu-id="33ce8-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="33ce8-860">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="33ce8-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="33ce8-861">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-861">**Old behavior**</span></span>

<span data-ttu-id="33ce8-862">Un type d’entité avec plusieurs propriétés de navigation unidirectionnelle autoréférencées et les clés étrangères correspondantes a été incorrectement configuré en tant que relation unique.</span><span class="sxs-lookup"><span data-stu-id="33ce8-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="33ce8-863">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-863">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="33ce8-864">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="33ce8-864">**New behavior**</span></span>

<span data-ttu-id="33ce8-865">Ce scénario est maintenant détecté dans la génération de modèle et une exception est levée, indiquant que le modèle est ambigu.</span><span class="sxs-lookup"><span data-stu-id="33ce8-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="33ce8-866">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="33ce8-866">**Why**</span></span>

<span data-ttu-id="33ce8-867">Le modèle résultant est ambigu et probablement erroné dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="33ce8-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="33ce8-868">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="33ce8-868">**Mitigations**</span></span>

<span data-ttu-id="33ce8-869">Utilisez la configuration complète de la relation.</span><span class="sxs-lookup"><span data-stu-id="33ce8-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="33ce8-870">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="33ce8-870">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
