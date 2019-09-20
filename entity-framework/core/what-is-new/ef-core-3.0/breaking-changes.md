---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 1f63593631017a61c39ccab9216adbc4663700e7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148901"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="9d656-102">Dernières modifications incluses dans EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="9d656-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="9d656-103">Les modifications d’API et de comportement suivantes peuvent bloquer les applications existantes lors de leur mise à niveau vers 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="9d656-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="9d656-104">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="9d656-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="9d656-105">Les interruptions d’une version préliminaire 3,0 à une autre version d’évaluation de 3,0 ne sont pas documentées ici.</span><span class="sxs-lookup"><span data-stu-id="9d656-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="9d656-106">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="9d656-106">Summary</span></span>

| <span data-ttu-id="9d656-107">**Modification avec rupture**</span><span class="sxs-lookup"><span data-stu-id="9d656-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="9d656-108">**Impact**</span><span class="sxs-lookup"><span data-stu-id="9d656-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="9d656-109">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="9d656-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="9d656-110">Haute</span><span class="sxs-lookup"><span data-stu-id="9d656-110">High</span></span>       |
| [<span data-ttu-id="9d656-111">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9d656-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="9d656-112">Haute</span><span class="sxs-lookup"><span data-stu-id="9d656-112">High</span></span>      |
| [<span data-ttu-id="9d656-113">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="9d656-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="9d656-114">Haute</span><span class="sxs-lookup"><span data-stu-id="9d656-114">High</span></span>      |
| [<span data-ttu-id="9d656-115">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="9d656-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="9d656-116">Haute</span><span class="sxs-lookup"><span data-stu-id="9d656-116">High</span></span>      |
| [<span data-ttu-id="9d656-117">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="9d656-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="9d656-118">Haute</span><span class="sxs-lookup"><span data-stu-id="9d656-118">High</span></span>      |
| [<span data-ttu-id="9d656-119">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d656-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="9d656-120">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-120">Medium</span></span>      |
| [<span data-ttu-id="9d656-121">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="9d656-122">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-122">Medium</span></span>      |
| [<span data-ttu-id="9d656-123">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="9d656-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="9d656-124">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-124">Medium</span></span>      |
| [<span data-ttu-id="9d656-125">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="9d656-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="9d656-126">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-126">Medium</span></span>      |
| [<span data-ttu-id="9d656-127">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="9d656-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="9d656-128">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-128">Medium</span></span>      |
| [<span data-ttu-id="9d656-129">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="9d656-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="9d656-130">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-130">Medium</span></span>      |
| [<span data-ttu-id="9d656-131">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9d656-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="9d656-132">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-132">Medium</span></span>      |
| [<span data-ttu-id="9d656-133">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="9d656-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="9d656-134">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-134">Medium</span></span>      |
| [<span data-ttu-id="9d656-135">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="9d656-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="9d656-136">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9d656-136">Medium</span></span>      |
| [<span data-ttu-id="9d656-137">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="9d656-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="9d656-138">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-138">Low</span></span>      |
| [<span data-ttu-id="9d656-139">~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli</span><span class="sxs-lookup"><span data-stu-id="9d656-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="9d656-140">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-140">Low</span></span>      |
| [<span data-ttu-id="9d656-141">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="9d656-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="9d656-142">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-142">Low</span></span>      |
| [<span data-ttu-id="9d656-143">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="9d656-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="9d656-144">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-144">Low</span></span>      |
| [<span data-ttu-id="9d656-145">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="9d656-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="9d656-146">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-146">Low</span></span>      |
| [<span data-ttu-id="9d656-147">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="9d656-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="9d656-148">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-148">Low</span></span>      |
| [<span data-ttu-id="9d656-149">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="9d656-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="9d656-150">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-150">Low</span></span>      |
| [<span data-ttu-id="9d656-151">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="9d656-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="9d656-152">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-152">Low</span></span>      |
| [<span data-ttu-id="9d656-153">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9d656-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="9d656-154">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-154">Low</span></span>      |
| [<span data-ttu-id="9d656-155">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="9d656-156">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-156">Low</span></span>      |
| [<span data-ttu-id="9d656-157">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="9d656-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="9d656-158">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-158">Low</span></span>      |
| [<span data-ttu-id="9d656-159">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="9d656-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="9d656-160">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-160">Low</span></span>      |
| [<span data-ttu-id="9d656-161">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9d656-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="9d656-162">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-162">Low</span></span>      |
| [<span data-ttu-id="9d656-163">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="9d656-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="9d656-164">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-164">Low</span></span>      |
| [<span data-ttu-id="9d656-165">Les clés de tableaux d’octets et de chaînes ne sont pas générées par client par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="9d656-166">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-166">Low</span></span>      |
| [<span data-ttu-id="9d656-167">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="9d656-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="9d656-168">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-168">Low</span></span>      |
| [<span data-ttu-id="9d656-169">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="9d656-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="9d656-170">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-170">Low</span></span>      |
| [<span data-ttu-id="9d656-171">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="9d656-172">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-172">Low</span></span>      |
| [<span data-ttu-id="9d656-173">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="9d656-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="9d656-174">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-174">Low</span></span>      |
| [<span data-ttu-id="9d656-175">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="9d656-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="9d656-176">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-176">Low</span></span>      |
| [<span data-ttu-id="9d656-177">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9d656-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="9d656-178">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-178">Low</span></span>      |
| [<span data-ttu-id="9d656-179">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="9d656-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="9d656-180">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-180">Low</span></span>      |
| [<span data-ttu-id="9d656-181">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="9d656-182">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-182">Low</span></span>      |
| [<span data-ttu-id="9d656-183">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9d656-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="9d656-184">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-184">Low</span></span>      |
| [<span data-ttu-id="9d656-185">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="9d656-186">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-186">Low</span></span>      |
| [<span data-ttu-id="9d656-187">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="9d656-188">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-188">Low</span></span>      |
| [<span data-ttu-id="9d656-189">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="9d656-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="9d656-190">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-190">Low</span></span>      |
| [<span data-ttu-id="9d656-191">Les infos/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9d656-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="9d656-192">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-192">Low</span></span>      |
| [<span data-ttu-id="9d656-193">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="9d656-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="9d656-194">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-194">Low</span></span>      |
| [<span data-ttu-id="9d656-195">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="9d656-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="9d656-196">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-196">Low</span></span>      |
| [<span data-ttu-id="9d656-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="9d656-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="9d656-198">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-198">Low</span></span>      |
| [<span data-ttu-id="9d656-199">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9d656-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="9d656-200">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-200">Low</span></span>      |
| [<span data-ttu-id="9d656-201">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9d656-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="9d656-202">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-202">Low</span></span>      |
| [<span data-ttu-id="9d656-203">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9d656-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="9d656-204">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-204">Low</span></span>      |
| [<span data-ttu-id="9d656-205">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="9d656-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="9d656-206">Faible</span><span class="sxs-lookup"><span data-stu-id="9d656-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="9d656-207">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="9d656-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="9d656-208">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="9d656-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="9d656-209">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-210">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-210">**Old behavior**</span></span>

<span data-ttu-id="9d656-211">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="9d656-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="9d656-212">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="9d656-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="9d656-213">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-213">**New behavior**</span></span>

<span data-ttu-id="9d656-214">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="9d656-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="9d656-215">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9d656-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="9d656-216">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-216">**Why**</span></span>

<span data-ttu-id="9d656-217">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="9d656-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="9d656-218">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="9d656-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="9d656-219">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="9d656-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="9d656-220">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="9d656-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="9d656-221">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="9d656-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="9d656-222">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="9d656-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="9d656-223">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-223">**Mitigations**</span></span>

<span data-ttu-id="9d656-224">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="9d656-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="9d656-225">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9d656-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="9d656-226">Suivi de problème no 15498</span><span class="sxs-lookup"><span data-stu-id="9d656-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="9d656-227">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-228">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-228">**Old behavior**</span></span>

<span data-ttu-id="9d656-229">Avant la version 3.0, EF Core ciblait .NET Standard 2.0 et s’exécutait sur toutes les plateformes qui prennent en charge cette norme, y compris .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9d656-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="9d656-230">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-230">**New behavior**</span></span>

<span data-ttu-id="9d656-231">À partir de la version 3.0, EF Core cible .NET Standard 2.1 et s’exécute sur toutes les plateformes qui prennent en charge cette norme.</span><span class="sxs-lookup"><span data-stu-id="9d656-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="9d656-232">Cela n'inclut pas .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9d656-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="9d656-233">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-233">**Why**</span></span>

<span data-ttu-id="9d656-234">Cela fait partie d’une décision stratégique dans les technologies .NET de concentrer l’énergie sur .NET Core et d’autres plateformes .NET modernes, telles que Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9d656-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="9d656-235">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-235">**Mitigations**</span></span>

<span data-ttu-id="9d656-236">Envisagez de passer à une plateforme .NET moderne.</span><span class="sxs-lookup"><span data-stu-id="9d656-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="9d656-237">Si ce n’est pas possible, continuez à utiliser EF Core 2.1 ou EF Core 2.2, qui prennent tous deux en charge .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9d656-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="9d656-238">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d656-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="9d656-239">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="9d656-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="9d656-240">Ce changement a été introduit dans ASP.NET Core 3.0-preview 1.</span><span class="sxs-lookup"><span data-stu-id="9d656-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="9d656-241">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-241">**Old behavior**</span></span>

<span data-ttu-id="9d656-242">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9d656-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="9d656-243">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-243">**New behavior**</span></span>

<span data-ttu-id="9d656-244">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d656-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="9d656-245">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-245">**Why**</span></span>

<span data-ttu-id="9d656-246">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="9d656-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="9d656-247">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="9d656-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="9d656-248">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="9d656-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="9d656-249">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="9d656-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="9d656-250">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-250">**Mitigations**</span></span>

<span data-ttu-id="9d656-251">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="9d656-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="9d656-252">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="9d656-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="9d656-253">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="9d656-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="9d656-254">Ce changement a été introduit dans EF Core 3.0-preview 4 et la version correspondante du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d656-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="9d656-255">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-255">**Old behavior**</span></span>

<span data-ttu-id="9d656-256">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="9d656-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="9d656-257">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-257">**New behavior**</span></span>

<span data-ttu-id="9d656-258">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="9d656-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="9d656-259">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-259">**Why**</span></span>

<span data-ttu-id="9d656-260">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9d656-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="9d656-261">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-261">**Mitigations**</span></span>

<span data-ttu-id="9d656-262">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` comme outil général :</span><span class="sxs-lookup"><span data-stu-id="9d656-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="9d656-263">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="9d656-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="9d656-264">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="9d656-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="9d656-265">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="9d656-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="9d656-266">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-267">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-267">**Old behavior**</span></span>

<span data-ttu-id="9d656-268">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="9d656-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="9d656-269">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-269">**New behavior**</span></span>

<span data-ttu-id="9d656-270">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="9d656-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="9d656-271">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="9d656-272">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="9d656-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="9d656-273">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="9d656-274">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="9d656-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="9d656-275">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-275">**Why**</span></span>

<span data-ttu-id="9d656-276">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="9d656-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="9d656-277">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="9d656-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="9d656-278">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-278">**Mitigations**</span></span>

<span data-ttu-id="9d656-279">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="9d656-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="9d656-280">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="9d656-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="9d656-281">Suivi de problème no 15704</span><span class="sxs-lookup"><span data-stu-id="9d656-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="9d656-282">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="9d656-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="9d656-283">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-283">**Old behavior**</span></span>

<span data-ttu-id="9d656-284">Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.</span><span class="sxs-lookup"><span data-stu-id="9d656-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="9d656-285">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-285">**New behavior**</span></span>

<span data-ttu-id="9d656-286">À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9d656-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="9d656-287">Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="9d656-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="9d656-288">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-288">**Why**</span></span>

<span data-ttu-id="9d656-289">La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="9d656-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="9d656-290">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-290">**Mitigations**</span></span>

<span data-ttu-id="9d656-291">Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="9d656-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="9d656-292">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="9d656-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="9d656-293">Suivi de problème no 13518</span><span class="sxs-lookup"><span data-stu-id="9d656-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="9d656-294">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="9d656-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="9d656-295">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-295">**Old behavior**</span></span>

<span data-ttu-id="9d656-296">Avant EF Core 3.0, la même instance d’entité était utilisée pour chaque occurrence d’une entité avec un type et un ID donnés.</span><span class="sxs-lookup"><span data-stu-id="9d656-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="9d656-297">Cela correspond au comportement des requêtes de suivi.</span><span class="sxs-lookup"><span data-stu-id="9d656-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="9d656-298">Par exemple, la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="9d656-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="9d656-299">retournait la même instance `Category` pour chaque `Product` associé à la catégorie donnée.</span><span class="sxs-lookup"><span data-stu-id="9d656-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="9d656-300">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-300">**New behavior**</span></span>

<span data-ttu-id="9d656-301">À compter de EF Core 3.0, différentes instances d’entité sont créées lorsqu’une entité avec un type et un ID donnés est rencontrée à différents emplacements dans le graphe retourné.</span><span class="sxs-lookup"><span data-stu-id="9d656-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="9d656-302">Par exemple, la requête ci-dessus retourne à présent une nouvelle instance `Category` pour chaque `Product` même si deux produits sont associés à la même catégorie.</span><span class="sxs-lookup"><span data-stu-id="9d656-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="9d656-303">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-303">**Why**</span></span>

<span data-ttu-id="9d656-304">La résolution de l’identité (autrement dit, le fait de déterminer qu’une entité a les mêmes type et ID qu’une entité précédemment rencontrée) améliore les performances et la surcharge de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="9d656-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="9d656-305">Cela agit généralement à l’opposé de la raison pour laquelle aucune requête de suivi n’est utilisée en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="9d656-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="9d656-306">En outre, même si la résolution de l’identité peut parfois être utile, elle n’est pas nécessaire si les entités doivent être sérialisées et envoyées à un client, ce qui est courant pour les requêtes sans suivi.</span><span class="sxs-lookup"><span data-stu-id="9d656-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="9d656-307">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-307">**Mitigations**</span></span>

<span data-ttu-id="9d656-308">Utilisez une requête de suivi si la résolution de l’identité est requise.</span><span class="sxs-lookup"><span data-stu-id="9d656-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="9d656-309">~~L’exécution de requêtes est enregistrée au niveau du débogage~~ rétabli</span><span class="sxs-lookup"><span data-stu-id="9d656-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="9d656-310">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="9d656-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="9d656-311">Ce changement a été rétabli dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-312">Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application.</span><span class="sxs-lookup"><span data-stu-id="9d656-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="9d656-313">Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :</span><span class="sxs-lookup"><span data-stu-id="9d656-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="9d656-314">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="9d656-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="9d656-315">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="9d656-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="9d656-316">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="9d656-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9d656-317">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-317">**Old behavior**</span></span>

<span data-ttu-id="9d656-318">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="9d656-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="9d656-319">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="9d656-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="9d656-320">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-320">**New behavior**</span></span>

<span data-ttu-id="9d656-321">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="9d656-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="9d656-322">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-322">**Why**</span></span>

<span data-ttu-id="9d656-323">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9d656-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="9d656-324">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-324">**Mitigations**</span></span>

<span data-ttu-id="9d656-325">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="9d656-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="9d656-326">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="9d656-326">This can be avoided by:</span></span>
* <span data-ttu-id="9d656-327">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="9d656-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="9d656-328">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="9d656-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="9d656-329">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="9d656-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="9d656-330">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="9d656-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="9d656-331">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="9d656-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="9d656-332">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="9d656-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="9d656-333">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-334">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-334">**Old behavior**</span></span>

<span data-ttu-id="9d656-335">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="9d656-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="9d656-336">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-336">**New behavior**</span></span>

<span data-ttu-id="9d656-337">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="9d656-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="9d656-338">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9d656-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="9d656-339">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="9d656-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="9d656-340">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-340">**Why**</span></span>

<span data-ttu-id="9d656-341">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="9d656-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="9d656-342">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-342">**Mitigations**</span></span>

<span data-ttu-id="9d656-343">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="9d656-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="9d656-344">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="9d656-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="9d656-345">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9d656-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="9d656-346">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="9d656-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="9d656-347">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="9d656-348">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="9d656-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="9d656-349">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-350">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-350">**Old behavior**</span></span>

<span data-ttu-id="9d656-351">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="9d656-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="9d656-352">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-352">**New behavior**</span></span>

<span data-ttu-id="9d656-353">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="9d656-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="9d656-354">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="9d656-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="9d656-355">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-355">**Why**</span></span>

<span data-ttu-id="9d656-356">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9d656-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="9d656-357">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-357">**Mitigations**</span></span>

<span data-ttu-id="9d656-358">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="9d656-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="9d656-359">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="9d656-360">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="9d656-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="9d656-361">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="9d656-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="9d656-362">Ce changement a été introduit dans EF Core 3.0-preview 5.</span><span class="sxs-lookup"><span data-stu-id="9d656-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="9d656-363">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-363">**Old behavior**</span></span>

<span data-ttu-id="9d656-364">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="9d656-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="9d656-365">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-365">**New behavior**</span></span>

<span data-ttu-id="9d656-366">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="9d656-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="9d656-367">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-367">**Why**</span></span>

<span data-ttu-id="9d656-368">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="9d656-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="9d656-369">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-369">**Mitigations**</span></span>

<span data-ttu-id="9d656-370">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="9d656-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="9d656-371">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="9d656-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="9d656-372">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="9d656-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="9d656-373">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-374">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-374">**Old behavior**</span></span>

<span data-ttu-id="9d656-375">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/keyless-entity-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="9d656-375">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="9d656-376">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="9d656-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="9d656-377">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-377">**New behavior**</span></span>

<span data-ttu-id="9d656-378">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="9d656-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="9d656-379">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="9d656-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="9d656-380">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-380">**Why**</span></span>

<span data-ttu-id="9d656-381">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="9d656-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="9d656-382">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="9d656-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="9d656-383">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="9d656-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="9d656-384">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-384">**Mitigations**</span></span>

<span data-ttu-id="9d656-385">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="9d656-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="9d656-386">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="9d656-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="9d656-387">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="9d656-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="9d656-388">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9d656-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="9d656-389">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="9d656-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="9d656-390">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="9d656-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="9d656-391">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="9d656-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="9d656-392">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-393">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-393">**Old behavior**</span></span>

<span data-ttu-id="9d656-394">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="9d656-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="9d656-395">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-395">**New behavior**</span></span>

<span data-ttu-id="9d656-396">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="9d656-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="9d656-397">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="9d656-398">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="9d656-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="9d656-399">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="9d656-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="9d656-400">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-400">For example:</span></span>

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

<span data-ttu-id="9d656-401">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="9d656-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="9d656-402">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-402">**Why**</span></span>

<span data-ttu-id="9d656-403">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="9d656-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="9d656-404">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="9d656-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="9d656-405">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-405">**Mitigations**</span></span>

<span data-ttu-id="9d656-406">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9d656-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="9d656-407">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="9d656-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="9d656-408">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="9d656-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="9d656-409">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-410">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-410">**Old behavior**</span></span>

<span data-ttu-id="9d656-411">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9d656-411">Consider the following model:</span></span>
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
<span data-ttu-id="9d656-412">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="9d656-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="9d656-413">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-413">**New behavior**</span></span>

<span data-ttu-id="9d656-414">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="9d656-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="9d656-415">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="9d656-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="9d656-416">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-416">**Mitigations**</span></span>

<span data-ttu-id="9d656-417">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="9d656-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="9d656-418">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="9d656-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="9d656-419">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="9d656-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="9d656-420">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="9d656-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="9d656-421">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-422">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-422">**Old behavior**</span></span>

<span data-ttu-id="9d656-423">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9d656-423">Consider the following model:</span></span>
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
<span data-ttu-id="9d656-424">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="9d656-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="9d656-425">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-425">**New behavior**</span></span>

<span data-ttu-id="9d656-426">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="9d656-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="9d656-427">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="9d656-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="9d656-428">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-428">**Why**</span></span>

<span data-ttu-id="9d656-429">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="9d656-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="9d656-430">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-430">**Mitigations**</span></span>

<span data-ttu-id="9d656-431">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="9d656-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="9d656-432">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="9d656-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="9d656-433">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="9d656-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="9d656-434">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="9d656-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="9d656-435">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-436">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-436">**Old behavior**</span></span>

<span data-ttu-id="9d656-437">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9d656-437">Consider the following model:</span></span>
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

<span data-ttu-id="9d656-438">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d656-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="9d656-439">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-439">**New behavior**</span></span>

<span data-ttu-id="9d656-440">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="9d656-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="9d656-441">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-441">**Why**</span></span>

<span data-ttu-id="9d656-442">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="9d656-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="9d656-443">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-443">**Mitigations**</span></span>

<span data-ttu-id="9d656-444">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="9d656-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="9d656-445">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="9d656-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="9d656-446">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="9d656-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="9d656-447">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-448">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-448">**Old behavior**</span></span>

<span data-ttu-id="9d656-449">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9d656-449">Consider the following model:</span></span>
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
<span data-ttu-id="9d656-450">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="9d656-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="9d656-451">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="9d656-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="9d656-452">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-452">**New behavior**</span></span>

<span data-ttu-id="9d656-453">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="9d656-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="9d656-454">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="9d656-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="9d656-455">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-455">For example:</span></span>

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

<span data-ttu-id="9d656-456">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-456">**Why**</span></span>

<span data-ttu-id="9d656-457">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="9d656-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="9d656-458">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-458">**Mitigations**</span></span>

<span data-ttu-id="9d656-459">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="9d656-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="9d656-460">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9d656-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="9d656-461">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="9d656-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="9d656-462">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-463">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-463">**Old behavior**</span></span>

<span data-ttu-id="9d656-464">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="9d656-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="9d656-465">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-465">**New behavior**</span></span>

<span data-ttu-id="9d656-466">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="9d656-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="9d656-467">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-467">**Why**</span></span>

<span data-ttu-id="9d656-468">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="9d656-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="9d656-469">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="9d656-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="9d656-470">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-470">**Mitigations**</span></span>

<span data-ttu-id="9d656-471">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="9d656-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="9d656-472">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="9d656-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="9d656-473">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="9d656-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="9d656-474">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-475">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-475">**Old behavior**</span></span>

<span data-ttu-id="9d656-476">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9d656-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="9d656-477">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-477">**New behavior**</span></span>

<span data-ttu-id="9d656-478">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9d656-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="9d656-479">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="9d656-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="9d656-480">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-480">**Why**</span></span>

<span data-ttu-id="9d656-481">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9d656-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="9d656-482">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-482">**Mitigations**</span></span>

<span data-ttu-id="9d656-483">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="9d656-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="9d656-484">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="9d656-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="9d656-485">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-485">Backing fields are used by default</span></span>

[<span data-ttu-id="9d656-486">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="9d656-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="9d656-487">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="9d656-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9d656-488">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-488">**Old behavior**</span></span>

<span data-ttu-id="9d656-489">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="9d656-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="9d656-490">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="9d656-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="9d656-491">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-491">**New behavior**</span></span>

<span data-ttu-id="9d656-492">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="9d656-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="9d656-493">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="9d656-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="9d656-494">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-494">**Why**</span></span>

<span data-ttu-id="9d656-495">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="9d656-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="9d656-496">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-496">**Mitigations**</span></span>

<span data-ttu-id="9d656-497">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9d656-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="9d656-498">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="9d656-499">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="9d656-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="9d656-500">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="9d656-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="9d656-501">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-502">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-502">**Old behavior**</span></span>

<span data-ttu-id="9d656-503">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="9d656-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="9d656-504">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="9d656-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="9d656-505">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-505">**New behavior**</span></span>

<span data-ttu-id="9d656-506">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9d656-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="9d656-507">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-507">**Why**</span></span>

<span data-ttu-id="9d656-508">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="9d656-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="9d656-509">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-509">**Mitigations**</span></span>

<span data-ttu-id="9d656-510">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="9d656-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="9d656-511">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9d656-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="9d656-512">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="9d656-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="9d656-513">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-514">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-514">**Old behavior**</span></span>

<span data-ttu-id="9d656-515">Avant le EF Core 3,0, une propriété pourrait être spécifiée par une valeur de chaîne et si aucune propriété portant ce nom n’a été trouvée sur le type .NET, EF Core essaiera de la faire correspondre à un champ à l’aide de règles de Convention.</span><span class="sxs-lookup"><span data-stu-id="9d656-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="9d656-516">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-516">**New behavior**</span></span>

<span data-ttu-id="9d656-517">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="9d656-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="9d656-518">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-518">**Why**</span></span>

<span data-ttu-id="9d656-519">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="9d656-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="9d656-520">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-520">**Mitigations**</span></span>

<span data-ttu-id="9d656-521">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="9d656-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="9d656-522">Dans une version ultérieure de EF Core après 3,0, nous prévoyons de réactiver explicitement la configuration d’un nom de champ différent du nom de la propriété (voir le problème [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)) :</span><span class="sxs-lookup"><span data-stu-id="9d656-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="9d656-523">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9d656-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="9d656-524">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="9d656-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="9d656-525">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-526">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-526">**Old behavior**</span></span>

<span data-ttu-id="9d656-527">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9d656-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="9d656-528">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-528">**New behavior**</span></span>

<span data-ttu-id="9d656-529">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9d656-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="9d656-530">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-530">**Why**</span></span>

<span data-ttu-id="9d656-531">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="9d656-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="9d656-532">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="9d656-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="9d656-533">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-533">**Mitigations**</span></span>

<span data-ttu-id="9d656-534">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9d656-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="9d656-535">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="9d656-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="9d656-536">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="9d656-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="9d656-537">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-538">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-538">**Old behavior**</span></span>

<span data-ttu-id="9d656-539">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="9d656-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="9d656-540">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="9d656-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="9d656-541">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-541">**New behavior**</span></span>

<span data-ttu-id="9d656-542">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="9d656-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="9d656-543">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="9d656-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="9d656-544">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="9d656-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="9d656-545">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="9d656-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="9d656-546">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-546">**Why**</span></span>

<span data-ttu-id="9d656-547">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="9d656-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="9d656-548">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-548">**Mitigations**</span></span>

<span data-ttu-id="9d656-549">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="9d656-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="9d656-550">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="9d656-551">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="9d656-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="9d656-552">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-553">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-553">**Old behavior**</span></span>

<span data-ttu-id="9d656-554">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="9d656-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="9d656-555">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="9d656-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="9d656-556">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-556">**New behavior**</span></span>

<span data-ttu-id="9d656-557">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="9d656-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="9d656-558">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-558">**Why**</span></span>

<span data-ttu-id="9d656-559">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="9d656-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="9d656-560">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-560">**Mitigations**</span></span>

<span data-ttu-id="9d656-561">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="9d656-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="9d656-562">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9d656-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="9d656-563">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="9d656-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="9d656-564">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="9d656-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="9d656-565">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="9d656-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="9d656-566">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-567">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-567">**Old behavior**</span></span>

<span data-ttu-id="9d656-568">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="9d656-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="9d656-569">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-569">**New behavior**</span></span>

<span data-ttu-id="9d656-570">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="9d656-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="9d656-571">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-571">**Why**</span></span>

<span data-ttu-id="9d656-572">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9d656-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="9d656-573">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-573">**Mitigations**</span></span>

<span data-ttu-id="9d656-574">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="9d656-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="9d656-575">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9d656-575">This isn't common.</span></span>
<span data-ttu-id="9d656-576">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="9d656-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="9d656-577">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="9d656-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="9d656-578">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="9d656-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="9d656-579">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="9d656-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="9d656-580">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-581">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-581">**Old behavior**</span></span>

<span data-ttu-id="9d656-582">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="9d656-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="9d656-583">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="9d656-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="9d656-584">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="9d656-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="9d656-585">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-585">**New behavior**</span></span>

<span data-ttu-id="9d656-586">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9d656-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="9d656-587">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="9d656-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="9d656-588">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="9d656-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="9d656-589">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="9d656-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="9d656-590">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-590">**Why**</span></span>

<span data-ttu-id="9d656-591">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="9d656-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="9d656-592">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-592">**Mitigations**</span></span>

<span data-ttu-id="9d656-593">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="9d656-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="9d656-594">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="9d656-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="9d656-595">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="9d656-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="9d656-596">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-597">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-597">**Old behavior**</span></span>

<span data-ttu-id="9d656-598">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9d656-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="9d656-599">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-599">**New behavior**</span></span>

<span data-ttu-id="9d656-600">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9d656-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="9d656-601">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-601">**Why**</span></span>

<span data-ttu-id="9d656-602">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="9d656-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="9d656-603">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-603">**Mitigations**</span></span>

<span data-ttu-id="9d656-604">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9d656-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="9d656-605">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9d656-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="9d656-606">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="9d656-607">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="9d656-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="9d656-608">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="9d656-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="9d656-609">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-610">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-610">**Old behavior**</span></span>

<span data-ttu-id="9d656-611">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="9d656-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="9d656-612">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="9d656-613">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="9d656-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="9d656-614">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9d656-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="9d656-615">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-615">**New behavior**</span></span>

<span data-ttu-id="9d656-616">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="9d656-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="9d656-617">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-617">**Why**</span></span>

<span data-ttu-id="9d656-618">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="9d656-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="9d656-619">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-619">**Mitigations**</span></span>

<span data-ttu-id="9d656-620">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="9d656-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="9d656-621">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9d656-621">This is not common.</span></span>
<span data-ttu-id="9d656-622">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9d656-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="9d656-623">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="9d656-624">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="9d656-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="9d656-625">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="9d656-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="9d656-626">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-627">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-627">**Old behavior**</span></span>

<span data-ttu-id="9d656-628">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="9d656-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="9d656-629">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="9d656-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="9d656-630">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-630">**New behavior**</span></span>

<span data-ttu-id="9d656-631">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="9d656-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="9d656-632">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-632">**Why**</span></span>

<span data-ttu-id="9d656-633">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="9d656-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="9d656-634">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-634">**Mitigations**</span></span>

<span data-ttu-id="9d656-635">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="9d656-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="9d656-636">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="9d656-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="9d656-637">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="9d656-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="9d656-638">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9d656-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="9d656-639">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="9d656-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="9d656-640">Ce changement a été introduit dans EF Core 3.0-preview 2.</span><span class="sxs-lookup"><span data-stu-id="9d656-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9d656-641">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-641">**Old behavior**</span></span>

<span data-ttu-id="9d656-642">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="9d656-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="9d656-643">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-643">**New behavior**</span></span>

<span data-ttu-id="9d656-644">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="9d656-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="9d656-645">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-645">**Why**</span></span>

<span data-ttu-id="9d656-646">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="9d656-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="9d656-647">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-647">**Mitigations**</span></span>

<span data-ttu-id="9d656-648">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9d656-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="9d656-649">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="9d656-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="9d656-650">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="9d656-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="9d656-651">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="9d656-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="9d656-652">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-653">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-653">**Old behavior**</span></span>

<span data-ttu-id="9d656-654">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9d656-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="9d656-655">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-655">**New behavior**</span></span>

<span data-ttu-id="9d656-656">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="9d656-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="9d656-657">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-657">**Why**</span></span>

<span data-ttu-id="9d656-658">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="9d656-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="9d656-659">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="9d656-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="9d656-660">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-660">**Mitigations**</span></span>

<span data-ttu-id="9d656-661">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="9d656-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="9d656-662">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="9d656-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="9d656-663">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="9d656-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="9d656-664">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-665">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-665">**Old behavior**</span></span>

<span data-ttu-id="9d656-666">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="9d656-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="9d656-667">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-667">**New behavior**</span></span>

<span data-ttu-id="9d656-668">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="9d656-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="9d656-669">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="9d656-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="9d656-670">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-670">**Why**</span></span>

<span data-ttu-id="9d656-671">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="9d656-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="9d656-672">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-672">**Mitigations**</span></span>

<span data-ttu-id="9d656-673">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9d656-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="9d656-674">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9d656-674">Metadata API changes</span></span>

[<span data-ttu-id="9d656-675">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="9d656-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9d656-676">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-677">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-677">**New behavior**</span></span>

<span data-ttu-id="9d656-678">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="9d656-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="9d656-679">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-679">**Why**</span></span>

<span data-ttu-id="9d656-680">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="9d656-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="9d656-681">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-681">**Mitigations**</span></span>

<span data-ttu-id="9d656-682">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="9d656-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="9d656-683">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="9d656-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="9d656-684">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="9d656-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9d656-685">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="9d656-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="9d656-686">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-686">**New behavior**</span></span>

<span data-ttu-id="9d656-687">Les méthodes d’extension spécifiques au fournisseur seront aplaties :</span><span class="sxs-lookup"><span data-stu-id="9d656-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="9d656-688">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-688">**Why**</span></span>

<span data-ttu-id="9d656-689">Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="9d656-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="9d656-690">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-690">**Mitigations**</span></span>

<span data-ttu-id="9d656-691">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="9d656-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="9d656-692">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="9d656-693">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="9d656-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="9d656-694">Ce changement a été introduit dans EF Core 3.0-preview 3.</span><span class="sxs-lookup"><span data-stu-id="9d656-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9d656-695">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-695">**Old behavior**</span></span>

<span data-ttu-id="9d656-696">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="9d656-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9d656-697">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-697">**New behavior**</span></span>

<span data-ttu-id="9d656-698">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="9d656-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9d656-699">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-699">**Why**</span></span>

<span data-ttu-id="9d656-700">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="9d656-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="9d656-701">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-701">**Mitigations**</span></span>

<span data-ttu-id="9d656-702">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d656-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="9d656-703">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="9d656-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="9d656-704">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9d656-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="9d656-705">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-705">**Old behavior**</span></span>

<span data-ttu-id="9d656-706">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="9d656-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="9d656-707">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-707">**New behavior**</span></span>

<span data-ttu-id="9d656-708">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="9d656-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="9d656-709">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-709">**Why**</span></span>

<span data-ttu-id="9d656-710">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="9d656-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="9d656-711">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-711">**Mitigations**</span></span>

<span data-ttu-id="9d656-712">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="9d656-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9d656-713">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9d656-714">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="9d656-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="9d656-715">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-716">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-716">**Old behavior**</span></span>

<span data-ttu-id="9d656-717">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="9d656-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="9d656-718">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-718">**New behavior**</span></span>

<span data-ttu-id="9d656-719">Maintenant, les valeurs Guid sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="9d656-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="9d656-720">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-720">**Why**</span></span>

<span data-ttu-id="9d656-721">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="9d656-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="9d656-722">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="9d656-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9d656-723">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-723">**Mitigations**</span></span>

<span data-ttu-id="9d656-724">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="9d656-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="9d656-725">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="9d656-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="9d656-726">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="9d656-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9d656-727">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9d656-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9d656-728">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="9d656-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="9d656-729">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-730">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-730">**Old behavior**</span></span>

<span data-ttu-id="9d656-731">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="9d656-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="9d656-732">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="9d656-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="9d656-733">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-733">**New behavior**</span></span>

<span data-ttu-id="9d656-734">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="9d656-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="9d656-735">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-735">**Why**</span></span>

<span data-ttu-id="9d656-736">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="9d656-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9d656-737">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-737">**Mitigations**</span></span>

<span data-ttu-id="9d656-738">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="9d656-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="9d656-739">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="9d656-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="9d656-740">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="9d656-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="9d656-741">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="9d656-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="9d656-742">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="9d656-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="9d656-743">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-744">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-744">**Old behavior**</span></span>

<span data-ttu-id="9d656-745">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="9d656-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="9d656-746">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-746">**New behavior**</span></span>

<span data-ttu-id="9d656-747">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="9d656-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="9d656-748">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-748">**Why**</span></span>

<span data-ttu-id="9d656-749">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="9d656-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="9d656-750">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="9d656-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="9d656-751">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-751">**Mitigations**</span></span>

<span data-ttu-id="9d656-752">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="9d656-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="9d656-753">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="9d656-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="9d656-754">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="9d656-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="9d656-755">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="9d656-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="9d656-756">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="9d656-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="9d656-757">Suivi de problème no 16400</span><span class="sxs-lookup"><span data-stu-id="9d656-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="9d656-758">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="9d656-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="9d656-759">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-759">**Old behavior**</span></span>

<span data-ttu-id="9d656-760">Avant EF Core 3.0, `UseRowNumberForPaging` pouvait être utilisé pour générer SQL pour la pagination qui est compatible avec SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="9d656-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="9d656-761">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-761">**New behavior**</span></span>

<span data-ttu-id="9d656-762">À compter de EF Core 3.0, EF génère uniquement SQL pour la pagination qui est uniquement compatible avec les versions de SQL Server ultérieures.</span><span class="sxs-lookup"><span data-stu-id="9d656-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="9d656-763">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-763">**Why**</span></span>

<span data-ttu-id="9d656-764">Nous effectuons cette modification, car [SQL Server 2008 n’est plus un produit pris en charge](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) et la mise à jour de cette fonctionnalité pour fonctionner avec les modifications de requête effectuées dans EF Core 3.0 est un travail significatif.</span><span class="sxs-lookup"><span data-stu-id="9d656-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="9d656-765">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-765">**Mitigations**</span></span>

<span data-ttu-id="9d656-766">Nous vous recommandons d’effectuer la mise à jour vers une version plus récente de SQL Server ou d’utiliser un niveau de compatibilité plus élevé, afin que le SQL généré soit pris en charge.</span><span class="sxs-lookup"><span data-stu-id="9d656-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="9d656-767">Cela dit, si vous ne pouvez pas faire cela, veuillez [commenter le problème de suivi](https://github.com/aspnet/EntityFrameworkCore/issues/16400) en incluant des détails.</span><span class="sxs-lookup"><span data-stu-id="9d656-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="9d656-768">Nous pourrions revisiter cette décision en fonction des commentaires.</span><span class="sxs-lookup"><span data-stu-id="9d656-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="9d656-769">Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9d656-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="9d656-770">Suivi du problème no 16119</span><span class="sxs-lookup"><span data-stu-id="9d656-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="9d656-771">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-772">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-772">**Old behavior**</span></span>

<span data-ttu-id="9d656-773">`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.</span><span class="sxs-lookup"><span data-stu-id="9d656-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="9d656-774">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-774">**New behavior**</span></span>

<span data-ttu-id="9d656-775">Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="9d656-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="9d656-776">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-776">**Why**</span></span>

<span data-ttu-id="9d656-777">Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="9d656-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="9d656-778">Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.</span><span class="sxs-lookup"><span data-stu-id="9d656-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="9d656-779">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-779">**Mitigations**</span></span>

<span data-ttu-id="9d656-780">Mettre à jour des extensions pour suivre le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="9d656-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="9d656-781">Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d656-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="9d656-782">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="9d656-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="9d656-783">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="9d656-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="9d656-784">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-785">**Changement**</span><span class="sxs-lookup"><span data-stu-id="9d656-785">**Change**</span></span>

<span data-ttu-id="9d656-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="9d656-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="9d656-787">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-787">**Why**</span></span>

<span data-ttu-id="9d656-788">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="9d656-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="9d656-789">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-789">**Mitigations**</span></span>

<span data-ttu-id="9d656-790">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="9d656-790">Use the new name.</span></span> <span data-ttu-id="9d656-791">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="9d656-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="9d656-792">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="9d656-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="9d656-793">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="9d656-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="9d656-794">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-795">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-795">**Old behavior**</span></span>

<span data-ttu-id="9d656-796">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="9d656-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="9d656-797">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="9d656-798">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-798">**New behavior**</span></span>

<span data-ttu-id="9d656-799">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="9d656-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="9d656-800">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="9d656-801">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-801">**Why**</span></span>

<span data-ttu-id="9d656-802">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="9d656-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="9d656-803">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-803">**Mitigations**</span></span>

<span data-ttu-id="9d656-804">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="9d656-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="9d656-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="9d656-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="9d656-806">Suivi du problème no 15997</span><span class="sxs-lookup"><span data-stu-id="9d656-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="9d656-807">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-808">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-808">**Old behavior**</span></span>

<span data-ttu-id="9d656-809">Avant EF Core 3.0, ces méthodes étaient protégées.</span><span class="sxs-lookup"><span data-stu-id="9d656-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="9d656-810">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-810">**New behavior**</span></span>

<span data-ttu-id="9d656-811">À compter d’EF Core 3.0, ces méthodes sont publiques.</span><span class="sxs-lookup"><span data-stu-id="9d656-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="9d656-812">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-812">**Why**</span></span>

<span data-ttu-id="9d656-813">Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide.</span><span class="sxs-lookup"><span data-stu-id="9d656-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="9d656-814">Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.</span><span class="sxs-lookup"><span data-stu-id="9d656-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="9d656-815">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-815">**Mitigations**</span></span>

<span data-ttu-id="9d656-816">Modifiez l’accessibilité de n’importe quel remplacements.</span><span class="sxs-lookup"><span data-stu-id="9d656-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="9d656-817">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9d656-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="9d656-818">Suivi du problème no 11506</span><span class="sxs-lookup"><span data-stu-id="9d656-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="9d656-819">Ce changement a été introduit dans EF Core 3.0-preview 4.</span><span class="sxs-lookup"><span data-stu-id="9d656-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9d656-820">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-820">**Old behavior**</span></span>

<span data-ttu-id="9d656-821">Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.</span><span class="sxs-lookup"><span data-stu-id="9d656-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="9d656-822">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-822">**New behavior**</span></span>

<span data-ttu-id="9d656-823">À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="9d656-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="9d656-824">Ce qui signifie que la dépendance ne circule pas de manière transitive dans d’autres projets et que vous ne pouvez plus, par défaut, faire référence à son assembly.</span><span class="sxs-lookup"><span data-stu-id="9d656-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="9d656-825">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-825">**Why**</span></span>

<span data-ttu-id="9d656-826">Ce package est uniquement destiné à être utilisé au moment du design.</span><span class="sxs-lookup"><span data-stu-id="9d656-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="9d656-827">Les applications déployées ne doivent pas y faire référence.</span><span class="sxs-lookup"><span data-stu-id="9d656-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="9d656-828">Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="9d656-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="9d656-829">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-829">**Mitigations**</span></span>

<span data-ttu-id="9d656-830">Si vous devez référencer ce package pour écraser le comportement au moment du design d’EF Core, vous pouvez mettre à jour des métadonnées d’élément PackageReference dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="9d656-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="9d656-831">Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées.</span><span class="sxs-lookup"><span data-stu-id="9d656-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="9d656-832">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9d656-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="9d656-833">Suivi du problème no 14824</span><span class="sxs-lookup"><span data-stu-id="9d656-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="9d656-834">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-835">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-835">**Old behavior**</span></span>

<span data-ttu-id="9d656-836">Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="9d656-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="9d656-837">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-837">**New behavior**</span></span>

<span data-ttu-id="9d656-838">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9d656-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9d656-839">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-839">**Why**</span></span>

<span data-ttu-id="9d656-840">La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="9d656-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="9d656-841">Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="9d656-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="9d656-842">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-842">**Mitigations**</span></span>

<span data-ttu-id="9d656-843">SQLitePCL.raw version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="9d656-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9d656-844">Pour plus d’informations, consultez les [notes de publication](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="9d656-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="9d656-845">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9d656-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="9d656-846">Suivi de problème no 14825</span><span class="sxs-lookup"><span data-stu-id="9d656-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="9d656-847">Ce changement a été introduit dans EF Core 3.0-préversion 7.</span><span class="sxs-lookup"><span data-stu-id="9d656-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="9d656-848">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-848">**Old behavior**</span></span>

<span data-ttu-id="9d656-849">Les packages spatiaux dépendaient précédemment de la version 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="9d656-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="9d656-850">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-850">**New behavior**</span></span>

<span data-ttu-id="9d656-851">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9d656-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9d656-852">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-852">**Why**</span></span>

<span data-ttu-id="9d656-853">La version 2.0.0 de NetTopologySuite vise à résoudre plusieurs problèmes de facilité d’utilisation rencontrés par les utilisateurs EF Core.</span><span class="sxs-lookup"><span data-stu-id="9d656-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="9d656-854">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-854">**Mitigations**</span></span>

<span data-ttu-id="9d656-855">NetTopologySuite version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="9d656-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9d656-856">Pour plus d’informations, consultez les [notes de publication](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="9d656-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="9d656-857">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="9d656-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="9d656-858">Suivi de problème no 13573</span><span class="sxs-lookup"><span data-stu-id="9d656-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="9d656-859">Ce changement a été introduit dans EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="9d656-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="9d656-860">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-860">**Old behavior**</span></span>

<span data-ttu-id="9d656-861">Un type d’entité avec plusieurs propriétés de navigation unidirectionnelle autoréférencées et les clés étrangères correspondantes a été incorrectement configuré en tant que relation unique.</span><span class="sxs-lookup"><span data-stu-id="9d656-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="9d656-862">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-862">For example:</span></span>

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

<span data-ttu-id="9d656-863">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9d656-863">**New behavior**</span></span>

<span data-ttu-id="9d656-864">Ce scénario est maintenant détecté dans la génération de modèle et une exception est levée, indiquant que le modèle est ambigu.</span><span class="sxs-lookup"><span data-stu-id="9d656-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="9d656-865">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9d656-865">**Why**</span></span>

<span data-ttu-id="9d656-866">Le modèle résultant est ambigu et probablement erroné dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="9d656-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="9d656-867">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9d656-867">**Mitigations**</span></span>

<span data-ttu-id="9d656-868">Utilisez la configuration complète de la relation.</span><span class="sxs-lookup"><span data-stu-id="9d656-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="9d656-869">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9d656-869">For example:</span></span>

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
