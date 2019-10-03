---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813622"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="9f619-102">Dernières modifications incluses dans EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="9f619-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="9f619-103">Les modifications d’API et de comportement suivantes peuvent bloquer les applications existantes lors de leur mise à niveau vers 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="9f619-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="9f619-104">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="9f619-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="9f619-105">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="9f619-105">Summary</span></span>

| <span data-ttu-id="9f619-106">**Modification avec rupture**</span><span class="sxs-lookup"><span data-stu-id="9f619-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="9f619-107">**Impact**</span><span class="sxs-lookup"><span data-stu-id="9f619-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="9f619-108">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="9f619-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="9f619-109">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-109">High</span></span>       |
| [<span data-ttu-id="9f619-110">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9f619-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="9f619-111">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-111">High</span></span>      |
| [<span data-ttu-id="9f619-112">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="9f619-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="9f619-113">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-113">High</span></span>      |
| [<span data-ttu-id="9f619-114">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="9f619-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="9f619-115">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-115">High</span></span>      |
| [<span data-ttu-id="9f619-116">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="9f619-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="9f619-117">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-117">High</span></span>      |
| [<span data-ttu-id="9f619-118">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="9f619-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="9f619-119">Élevé</span><span class="sxs-lookup"><span data-stu-id="9f619-119">High</span></span>      |
| [<span data-ttu-id="9f619-120">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f619-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="9f619-121">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-121">Medium</span></span>      |
| [<span data-ttu-id="9f619-122">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="9f619-123">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-123">Medium</span></span>      |
| [<span data-ttu-id="9f619-124">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="9f619-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="9f619-125">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-125">Medium</span></span>      |
| [<span data-ttu-id="9f619-126">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="9f619-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="9f619-127">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-127">Medium</span></span>      |
| [<span data-ttu-id="9f619-128">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="9f619-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="9f619-129">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-129">Medium</span></span>      |
| [<span data-ttu-id="9f619-130">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="9f619-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="9f619-131">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-131">Medium</span></span>      |
| [<span data-ttu-id="9f619-132">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9f619-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="9f619-133">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-133">Medium</span></span>      |
| [<span data-ttu-id="9f619-134">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="9f619-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="9f619-135">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-135">Medium</span></span>      |
| [<span data-ttu-id="9f619-136">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="9f619-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="9f619-137">Moyenne</span><span class="sxs-lookup"><span data-stu-id="9f619-137">Medium</span></span>      |
| [<span data-ttu-id="9f619-138">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="9f619-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="9f619-139">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-139">Low</span></span>      |
| [<span data-ttu-id="9f619-140">~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli</span><span class="sxs-lookup"><span data-stu-id="9f619-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="9f619-141">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-141">Low</span></span>      |
| [<span data-ttu-id="9f619-142">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="9f619-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="9f619-143">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-143">Low</span></span>      |
| [<span data-ttu-id="9f619-144">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="9f619-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="9f619-145">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-145">Low</span></span>      |
| [<span data-ttu-id="9f619-146">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="9f619-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="9f619-147">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-147">Low</span></span>      |
| [<span data-ttu-id="9f619-148">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="9f619-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="9f619-149">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-149">Low</span></span>      |
| [<span data-ttu-id="9f619-150">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="9f619-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="9f619-151">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-151">Low</span></span>      |
| [<span data-ttu-id="9f619-152">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9f619-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="9f619-153">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-153">Low</span></span>      |
| [<span data-ttu-id="9f619-154">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="9f619-155">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-155">Low</span></span>      |
| [<span data-ttu-id="9f619-156">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="9f619-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="9f619-157">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-157">Low</span></span>      |
| [<span data-ttu-id="9f619-158">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="9f619-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="9f619-159">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-159">Low</span></span>      |
| [<span data-ttu-id="9f619-160">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f619-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="9f619-161">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-161">Low</span></span>      |
| [<span data-ttu-id="9f619-162">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="9f619-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="9f619-163">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-163">Low</span></span>      |
| [<span data-ttu-id="9f619-164">Les clés de tableaux d’octets et de chaînes ne sont pas générées par client par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="9f619-165">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-165">Low</span></span>      |
| [<span data-ttu-id="9f619-166">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="9f619-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="9f619-167">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-167">Low</span></span>      |
| [<span data-ttu-id="9f619-168">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="9f619-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="9f619-169">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-169">Low</span></span>      |
| [<span data-ttu-id="9f619-170">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="9f619-171">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-171">Low</span></span>      |
| [<span data-ttu-id="9f619-172">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="9f619-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="9f619-173">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-173">Low</span></span>      |
| [<span data-ttu-id="9f619-174">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="9f619-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="9f619-175">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-175">Low</span></span>      |
| [<span data-ttu-id="9f619-176">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9f619-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="9f619-177">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-177">Low</span></span>      |
| [<span data-ttu-id="9f619-178">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="9f619-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="9f619-179">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-179">Low</span></span>      |
| [<span data-ttu-id="9f619-180">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="9f619-181">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-181">Low</span></span>      |
| [<span data-ttu-id="9f619-182">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9f619-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="9f619-183">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-183">Low</span></span>      |
| [<span data-ttu-id="9f619-184">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="9f619-185">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-185">Low</span></span>      |
| [<span data-ttu-id="9f619-186">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="9f619-187">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-187">Low</span></span>      |
| [<span data-ttu-id="9f619-188">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="9f619-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="9f619-189">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-189">Low</span></span>      |
| [<span data-ttu-id="9f619-190">Les infos/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9f619-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="9f619-191">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-191">Low</span></span>      |
| [<span data-ttu-id="9f619-192">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="9f619-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="9f619-193">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-193">Low</span></span>      |
| [<span data-ttu-id="9f619-194">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="9f619-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="9f619-195">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-195">Low</span></span>      |
| [<span data-ttu-id="9f619-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="9f619-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="9f619-197">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-197">Low</span></span>      |
| [<span data-ttu-id="9f619-198">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9f619-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="9f619-199">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-199">Low</span></span>      |
| [<span data-ttu-id="9f619-200">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9f619-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="9f619-201">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-201">Low</span></span>      |
| [<span data-ttu-id="9f619-202">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9f619-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="9f619-203">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-203">Low</span></span>      |
| [<span data-ttu-id="9f619-204">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="9f619-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="9f619-205">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-205">Low</span></span>      |
| [<span data-ttu-id="9f619-206">DbFunction. Schema étant null ou une chaîne vide, il est configuré pour être dans le schéma par défaut du modèle</span><span class="sxs-lookup"><span data-stu-id="9f619-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="9f619-207">Faible</span><span class="sxs-lookup"><span data-stu-id="9f619-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="9f619-208">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="9f619-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="9f619-209">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="9f619-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="9f619-210">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-210">**Old behavior**</span></span>

<span data-ttu-id="9f619-211">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="9f619-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="9f619-212">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="9f619-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="9f619-213">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-213">**New behavior**</span></span>

<span data-ttu-id="9f619-214">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="9f619-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="9f619-215">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9f619-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="9f619-216">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-216">**Why**</span></span>

<span data-ttu-id="9f619-217">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="9f619-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="9f619-218">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="9f619-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="9f619-219">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="9f619-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="9f619-220">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="9f619-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="9f619-221">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="9f619-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="9f619-222">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="9f619-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="9f619-223">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-223">**Mitigations**</span></span>

<span data-ttu-id="9f619-224">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="9f619-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="9f619-225">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9f619-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="9f619-226">Suivi de problème no 15498</span><span class="sxs-lookup"><span data-stu-id="9f619-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="9f619-227">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-227">**Old behavior**</span></span>

<span data-ttu-id="9f619-228">Avant la version 3.0, EF Core ciblait .NET Standard 2.0 et s’exécutait sur toutes les plateformes qui prennent en charge cette norme, y compris .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9f619-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="9f619-229">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-229">**New behavior**</span></span>

<span data-ttu-id="9f619-230">À partir de la version 3.0, EF Core cible .NET Standard 2.1 et s’exécute sur toutes les plateformes qui prennent en charge cette norme.</span><span class="sxs-lookup"><span data-stu-id="9f619-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="9f619-231">Cela n'inclut pas .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9f619-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="9f619-232">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-232">**Why**</span></span>

<span data-ttu-id="9f619-233">Cela fait partie d’une décision stratégique dans les technologies .NET de concentrer l’énergie sur .NET Core et d’autres plateformes .NET modernes, telles que Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9f619-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="9f619-234">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-234">**Mitigations**</span></span>

<span data-ttu-id="9f619-235">Envisagez de passer à une plateforme .NET moderne.</span><span class="sxs-lookup"><span data-stu-id="9f619-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="9f619-236">Si ce n’est pas possible, continuez à utiliser EF Core 2.1 ou EF Core 2.2, qui prennent tous deux en charge .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9f619-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="9f619-237">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f619-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="9f619-238">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="9f619-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="9f619-239">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-239">**Old behavior**</span></span>

<span data-ttu-id="9f619-240">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f619-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="9f619-241">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-241">**New behavior**</span></span>

<span data-ttu-id="9f619-242">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f619-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="9f619-243">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-243">**Why**</span></span>

<span data-ttu-id="9f619-244">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="9f619-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="9f619-245">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="9f619-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="9f619-246">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="9f619-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="9f619-247">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="9f619-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="9f619-248">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-248">**Mitigations**</span></span>

<span data-ttu-id="9f619-249">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="9f619-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="9f619-250">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="9f619-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="9f619-251">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="9f619-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="9f619-252">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-252">**Old behavior**</span></span>

<span data-ttu-id="9f619-253">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="9f619-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="9f619-254">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-254">**New behavior**</span></span>

<span data-ttu-id="9f619-255">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="9f619-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="9f619-256">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-256">**Why**</span></span>

<span data-ttu-id="9f619-257">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9f619-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="9f619-258">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-258">**Mitigations**</span></span>

<span data-ttu-id="9f619-259">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` comme outil général :</span><span class="sxs-lookup"><span data-stu-id="9f619-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="9f619-260">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="9f619-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="9f619-261">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="9f619-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="9f619-262">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="9f619-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="9f619-263">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-263">**Old behavior**</span></span>

<span data-ttu-id="9f619-264">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="9f619-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="9f619-265">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-265">**New behavior**</span></span>

<span data-ttu-id="9f619-266">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="9f619-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="9f619-267">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="9f619-268">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="9f619-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="9f619-269">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="9f619-270">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="9f619-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="9f619-271">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-271">**Why**</span></span>

<span data-ttu-id="9f619-272">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="9f619-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="9f619-273">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="9f619-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="9f619-274">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-274">**Mitigations**</span></span>

<span data-ttu-id="9f619-275">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="9f619-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="9f619-276">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="9f619-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="9f619-277">Suivi de problème no 15704</span><span class="sxs-lookup"><span data-stu-id="9f619-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="9f619-278">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-278">**Old behavior**</span></span>

<span data-ttu-id="9f619-279">Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.</span><span class="sxs-lookup"><span data-stu-id="9f619-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="9f619-280">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-280">**New behavior**</span></span>

<span data-ttu-id="9f619-281">À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9f619-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="9f619-282">Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="9f619-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="9f619-283">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-283">**Why**</span></span>

<span data-ttu-id="9f619-284">La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="9f619-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="9f619-285">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-285">**Mitigations**</span></span>

<span data-ttu-id="9f619-286">Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="9f619-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="9f619-287">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="9f619-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="9f619-288">Suivi de problème no 13518</span><span class="sxs-lookup"><span data-stu-id="9f619-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="9f619-289">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-289">**Old behavior**</span></span>

<span data-ttu-id="9f619-290">Avant EF Core 3.0, la même instance d’entité était utilisée pour chaque occurrence d’une entité avec un type et un ID donnés.</span><span class="sxs-lookup"><span data-stu-id="9f619-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="9f619-291">Cela correspond au comportement des requêtes de suivi.</span><span class="sxs-lookup"><span data-stu-id="9f619-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="9f619-292">Par exemple, la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="9f619-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="9f619-293">retournait la même instance `Category` pour chaque `Product` associé à la catégorie donnée.</span><span class="sxs-lookup"><span data-stu-id="9f619-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="9f619-294">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-294">**New behavior**</span></span>

<span data-ttu-id="9f619-295">À compter de EF Core 3.0, différentes instances d’entité sont créées lorsqu’une entité avec un type et un ID donnés est rencontrée à différents emplacements dans le graphe retourné.</span><span class="sxs-lookup"><span data-stu-id="9f619-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="9f619-296">Par exemple, la requête ci-dessus retourne à présent une nouvelle instance `Category` pour chaque `Product` même si deux produits sont associés à la même catégorie.</span><span class="sxs-lookup"><span data-stu-id="9f619-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="9f619-297">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-297">**Why**</span></span>

<span data-ttu-id="9f619-298">La résolution de l’identité (autrement dit, le fait de déterminer qu’une entité a les mêmes type et ID qu’une entité précédemment rencontrée) améliore les performances et la surcharge de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="9f619-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="9f619-299">Cela agit généralement à l’opposé de la raison pour laquelle aucune requête de suivi n’est utilisée en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="9f619-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="9f619-300">En outre, même si la résolution de l’identité peut parfois être utile, elle n’est pas nécessaire si les entités doivent être sérialisées et envoyées à un client, ce qui est courant pour les requêtes sans suivi.</span><span class="sxs-lookup"><span data-stu-id="9f619-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="9f619-301">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-301">**Mitigations**</span></span>

<span data-ttu-id="9f619-302">Utilisez une requête de suivi si la résolution de l’identité est requise.</span><span class="sxs-lookup"><span data-stu-id="9f619-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="9f619-303">~~L’exécution de requêtes est enregistrée au niveau du débogage~~ rétabli</span><span class="sxs-lookup"><span data-stu-id="9f619-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="9f619-304">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="9f619-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="9f619-305">Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application.</span><span class="sxs-lookup"><span data-stu-id="9f619-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="9f619-306">Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :</span><span class="sxs-lookup"><span data-stu-id="9f619-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="9f619-307">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="9f619-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="9f619-308">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="9f619-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="9f619-309">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-309">**Old behavior**</span></span>

<span data-ttu-id="9f619-310">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="9f619-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="9f619-311">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="9f619-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="9f619-312">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-312">**New behavior**</span></span>

<span data-ttu-id="9f619-313">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="9f619-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="9f619-314">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-314">**Why**</span></span>

<span data-ttu-id="9f619-315">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9f619-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="9f619-316">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-316">**Mitigations**</span></span>

<span data-ttu-id="9f619-317">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="9f619-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="9f619-318">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="9f619-318">This can be avoided by:</span></span>
* <span data-ttu-id="9f619-319">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="9f619-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="9f619-320">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="9f619-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="9f619-321">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="9f619-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="9f619-322">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="9f619-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="9f619-323">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="9f619-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="9f619-324">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="9f619-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="9f619-325">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-325">**Old behavior**</span></span>

<span data-ttu-id="9f619-326">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="9f619-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="9f619-327">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-327">**New behavior**</span></span>

<span data-ttu-id="9f619-328">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="9f619-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="9f619-329">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9f619-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="9f619-330">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="9f619-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="9f619-331">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-331">**Why**</span></span>

<span data-ttu-id="9f619-332">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="9f619-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="9f619-333">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-333">**Mitigations**</span></span>

<span data-ttu-id="9f619-334">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="9f619-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="9f619-335">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="9f619-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="9f619-336">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9f619-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="9f619-337">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="9f619-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="9f619-338">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="9f619-339">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="9f619-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="9f619-340">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-340">**Old behavior**</span></span>

<span data-ttu-id="9f619-341">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="9f619-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="9f619-342">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-342">**New behavior**</span></span>

<span data-ttu-id="9f619-343">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="9f619-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="9f619-344">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="9f619-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="9f619-345">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-345">**Why**</span></span>

<span data-ttu-id="9f619-346">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9f619-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="9f619-347">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-347">**Mitigations**</span></span>

<span data-ttu-id="9f619-348">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="9f619-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="9f619-349">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="9f619-350">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="9f619-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="9f619-351">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="9f619-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="9f619-352">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-352">**Old behavior**</span></span>

<span data-ttu-id="9f619-353">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="9f619-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="9f619-354">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-354">**New behavior**</span></span>

<span data-ttu-id="9f619-355">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="9f619-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="9f619-356">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-356">**Why**</span></span>

<span data-ttu-id="9f619-357">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="9f619-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="9f619-358">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-358">**Mitigations**</span></span>

<span data-ttu-id="9f619-359">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="9f619-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="9f619-360">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="9f619-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="9f619-361">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="9f619-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="9f619-362">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-362">**Old behavior**</span></span>

<span data-ttu-id="9f619-363">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/keyless-entity-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="9f619-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="9f619-364">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="9f619-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="9f619-365">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-365">**New behavior**</span></span>

<span data-ttu-id="9f619-366">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="9f619-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="9f619-367">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="9f619-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="9f619-368">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-368">**Why**</span></span>

<span data-ttu-id="9f619-369">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="9f619-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="9f619-370">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="9f619-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="9f619-371">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="9f619-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="9f619-372">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-372">**Mitigations**</span></span>

<span data-ttu-id="9f619-373">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="9f619-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="9f619-374">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="9f619-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="9f619-375">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="9f619-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="9f619-376">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9f619-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="9f619-377">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="9f619-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="9f619-378">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="9f619-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="9f619-379">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="9f619-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="9f619-380">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-380">**Old behavior**</span></span>

<span data-ttu-id="9f619-381">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="9f619-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="9f619-382">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-382">**New behavior**</span></span>

<span data-ttu-id="9f619-383">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="9f619-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="9f619-384">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="9f619-385">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="9f619-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="9f619-386">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="9f619-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="9f619-387">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-387">For example:</span></span>

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

<span data-ttu-id="9f619-388">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="9f619-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="9f619-389">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-389">**Why**</span></span>

<span data-ttu-id="9f619-390">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="9f619-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="9f619-391">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="9f619-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="9f619-392">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-392">**Mitigations**</span></span>

<span data-ttu-id="9f619-393">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9f619-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="9f619-394">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="9f619-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="9f619-395">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="9f619-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="9f619-396">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-396">**Old behavior**</span></span>

<span data-ttu-id="9f619-397">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9f619-397">Consider the following model:</span></span>
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
<span data-ttu-id="9f619-398">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="9f619-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="9f619-399">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-399">**New behavior**</span></span>

<span data-ttu-id="9f619-400">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="9f619-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="9f619-401">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="9f619-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="9f619-402">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-402">**Mitigations**</span></span>

<span data-ttu-id="9f619-403">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="9f619-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="9f619-404">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="9f619-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="9f619-405">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="9f619-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="9f619-406">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="9f619-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="9f619-407">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-407">**Old behavior**</span></span>

<span data-ttu-id="9f619-408">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9f619-408">Consider the following model:</span></span>
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
<span data-ttu-id="9f619-409">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="9f619-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="9f619-410">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-410">**New behavior**</span></span>

<span data-ttu-id="9f619-411">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="9f619-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="9f619-412">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="9f619-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="9f619-413">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-413">**Why**</span></span>

<span data-ttu-id="9f619-414">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="9f619-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="9f619-415">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-415">**Mitigations**</span></span>

<span data-ttu-id="9f619-416">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="9f619-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="9f619-417">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="9f619-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="9f619-418">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="9f619-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="9f619-419">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="9f619-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="9f619-420">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-420">**Old behavior**</span></span>

<span data-ttu-id="9f619-421">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9f619-421">Consider the following model:</span></span>
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

<span data-ttu-id="9f619-422">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="9f619-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="9f619-423">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-423">**New behavior**</span></span>

<span data-ttu-id="9f619-424">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="9f619-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="9f619-425">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-425">**Why**</span></span>

<span data-ttu-id="9f619-426">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="9f619-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="9f619-427">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-427">**Mitigations**</span></span>

<span data-ttu-id="9f619-428">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="9f619-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="9f619-429">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="9f619-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="9f619-430">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="9f619-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="9f619-431">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-431">**Old behavior**</span></span>

<span data-ttu-id="9f619-432">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="9f619-432">Consider the following model:</span></span>
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
<span data-ttu-id="9f619-433">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="9f619-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="9f619-434">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="9f619-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="9f619-435">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-435">**New behavior**</span></span>

<span data-ttu-id="9f619-436">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="9f619-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="9f619-437">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="9f619-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="9f619-438">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-438">For example:</span></span>

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

<span data-ttu-id="9f619-439">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-439">**Why**</span></span>

<span data-ttu-id="9f619-440">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="9f619-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="9f619-441">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-441">**Mitigations**</span></span>

<span data-ttu-id="9f619-442">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="9f619-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="9f619-443">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9f619-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="9f619-444">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="9f619-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="9f619-445">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-445">**Old behavior**</span></span>

<span data-ttu-id="9f619-446">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="9f619-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="9f619-447">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-447">**New behavior**</span></span>

<span data-ttu-id="9f619-448">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="9f619-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="9f619-449">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-449">**Why**</span></span>

<span data-ttu-id="9f619-450">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="9f619-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="9f619-451">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="9f619-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="9f619-452">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-452">**Mitigations**</span></span>

<span data-ttu-id="9f619-453">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="9f619-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="9f619-454">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="9f619-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="9f619-455">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="9f619-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="9f619-456">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-456">**Old behavior**</span></span>

<span data-ttu-id="9f619-457">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9f619-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="9f619-458">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-458">**New behavior**</span></span>

<span data-ttu-id="9f619-459">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9f619-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="9f619-460">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="9f619-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="9f619-461">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-461">**Why**</span></span>

<span data-ttu-id="9f619-462">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="9f619-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="9f619-463">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-463">**Mitigations**</span></span>

<span data-ttu-id="9f619-464">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="9f619-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="9f619-465">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="9f619-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="9f619-466">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-466">Backing fields are used by default</span></span>

[<span data-ttu-id="9f619-467">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="9f619-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="9f619-468">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-468">**Old behavior**</span></span>

<span data-ttu-id="9f619-469">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="9f619-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="9f619-470">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="9f619-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="9f619-471">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-471">**New behavior**</span></span>

<span data-ttu-id="9f619-472">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="9f619-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="9f619-473">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="9f619-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="9f619-474">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-474">**Why**</span></span>

<span data-ttu-id="9f619-475">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="9f619-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="9f619-476">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-476">**Mitigations**</span></span>

<span data-ttu-id="9f619-477">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9f619-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="9f619-478">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="9f619-479">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="9f619-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="9f619-480">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="9f619-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="9f619-481">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-481">**Old behavior**</span></span>

<span data-ttu-id="9f619-482">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="9f619-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="9f619-483">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="9f619-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="9f619-484">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-484">**New behavior**</span></span>

<span data-ttu-id="9f619-485">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9f619-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="9f619-486">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-486">**Why**</span></span>

<span data-ttu-id="9f619-487">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="9f619-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="9f619-488">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-488">**Mitigations**</span></span>

<span data-ttu-id="9f619-489">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="9f619-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="9f619-490">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9f619-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="9f619-491">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="9f619-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="9f619-492">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-492">**Old behavior**</span></span>

<span data-ttu-id="9f619-493">Avant le EF Core 3,0, une propriété pourrait être spécifiée par une valeur de chaîne et si aucune propriété portant ce nom n’a été trouvée sur le type .NET, EF Core essaiera de la faire correspondre à un champ à l’aide de règles de Convention.</span><span class="sxs-lookup"><span data-stu-id="9f619-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="9f619-494">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-494">**New behavior**</span></span>

<span data-ttu-id="9f619-495">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="9f619-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="9f619-496">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-496">**Why**</span></span>

<span data-ttu-id="9f619-497">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="9f619-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="9f619-498">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-498">**Mitigations**</span></span>

<span data-ttu-id="9f619-499">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="9f619-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="9f619-500">Dans une version ultérieure de EF Core après 3,0, nous prévoyons de réactiver explicitement la configuration d’un nom de champ différent du nom de la propriété (voir le problème [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)) :</span><span class="sxs-lookup"><span data-stu-id="9f619-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="9f619-501">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f619-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="9f619-502">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="9f619-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="9f619-503">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-503">**Old behavior**</span></span>

<span data-ttu-id="9f619-504">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9f619-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="9f619-505">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-505">**New behavior**</span></span>

<span data-ttu-id="9f619-506">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9f619-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="9f619-507">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-507">**Why**</span></span>

<span data-ttu-id="9f619-508">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f619-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="9f619-509">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="9f619-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="9f619-510">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-510">**Mitigations**</span></span>

<span data-ttu-id="9f619-511">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9f619-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="9f619-512">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="9f619-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="9f619-513">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="9f619-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="9f619-514">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-514">**Old behavior**</span></span>

<span data-ttu-id="9f619-515">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="9f619-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="9f619-516">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="9f619-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="9f619-517">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-517">**New behavior**</span></span>

<span data-ttu-id="9f619-518">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="9f619-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="9f619-519">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f619-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="9f619-520">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="9f619-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="9f619-521">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="9f619-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="9f619-522">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-522">**Why**</span></span>

<span data-ttu-id="9f619-523">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="9f619-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="9f619-524">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-524">**Mitigations**</span></span>

<span data-ttu-id="9f619-525">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="9f619-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="9f619-526">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="9f619-527">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="9f619-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="9f619-528">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-528">**Old behavior**</span></span>

<span data-ttu-id="9f619-529">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="9f619-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="9f619-530">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="9f619-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="9f619-531">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-531">**New behavior**</span></span>

<span data-ttu-id="9f619-532">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="9f619-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="9f619-533">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-533">**Why**</span></span>

<span data-ttu-id="9f619-534">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="9f619-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="9f619-535">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-535">**Mitigations**</span></span>

<span data-ttu-id="9f619-536">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="9f619-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="9f619-537">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="9f619-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="9f619-538">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="9f619-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="9f619-539">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="9f619-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="9f619-540">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="9f619-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="9f619-541">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-541">**Old behavior**</span></span>

<span data-ttu-id="9f619-542">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="9f619-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="9f619-543">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-543">**New behavior**</span></span>

<span data-ttu-id="9f619-544">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="9f619-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="9f619-545">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-545">**Why**</span></span>

<span data-ttu-id="9f619-546">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9f619-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="9f619-547">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-547">**Mitigations**</span></span>

<span data-ttu-id="9f619-548">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="9f619-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="9f619-549">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9f619-549">This isn't common.</span></span>
<span data-ttu-id="9f619-550">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="9f619-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="9f619-551">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="9f619-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="9f619-552">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="9f619-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="9f619-553">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="9f619-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="9f619-554">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-554">**Old behavior**</span></span>

<span data-ttu-id="9f619-555">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="9f619-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="9f619-556">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="9f619-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="9f619-557">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="9f619-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="9f619-558">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-558">**New behavior**</span></span>

<span data-ttu-id="9f619-559">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9f619-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="9f619-560">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="9f619-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="9f619-561">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="9f619-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="9f619-562">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="9f619-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="9f619-563">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-563">**Why**</span></span>

<span data-ttu-id="9f619-564">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="9f619-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="9f619-565">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-565">**Mitigations**</span></span>

<span data-ttu-id="9f619-566">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="9f619-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="9f619-567">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="9f619-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="9f619-568">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="9f619-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="9f619-569">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-569">**Old behavior**</span></span>

<span data-ttu-id="9f619-570">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9f619-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="9f619-571">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-571">**New behavior**</span></span>

<span data-ttu-id="9f619-572">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="9f619-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="9f619-573">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-573">**Why**</span></span>

<span data-ttu-id="9f619-574">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="9f619-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="9f619-575">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-575">**Mitigations**</span></span>

<span data-ttu-id="9f619-576">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="9f619-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="9f619-577">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9f619-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="9f619-578">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="9f619-579">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="9f619-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="9f619-580">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="9f619-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="9f619-581">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-581">**Old behavior**</span></span>

<span data-ttu-id="9f619-582">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="9f619-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="9f619-583">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="9f619-584">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="9f619-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="9f619-585">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9f619-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="9f619-586">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-586">**New behavior**</span></span>

<span data-ttu-id="9f619-587">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="9f619-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="9f619-588">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-588">**Why**</span></span>

<span data-ttu-id="9f619-589">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="9f619-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="9f619-590">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-590">**Mitigations**</span></span>

<span data-ttu-id="9f619-591">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="9f619-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="9f619-592">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9f619-592">This is not common.</span></span>
<span data-ttu-id="9f619-593">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="9f619-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="9f619-594">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="9f619-595">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="9f619-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="9f619-596">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="9f619-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="9f619-597">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-597">**Old behavior**</span></span>

<span data-ttu-id="9f619-598">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="9f619-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="9f619-599">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="9f619-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="9f619-600">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-600">**New behavior**</span></span>

<span data-ttu-id="9f619-601">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="9f619-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="9f619-602">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-602">**Why**</span></span>

<span data-ttu-id="9f619-603">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="9f619-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="9f619-604">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-604">**Mitigations**</span></span>

<span data-ttu-id="9f619-605">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="9f619-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="9f619-606">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="9f619-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="9f619-607">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="9f619-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="9f619-608">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9f619-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="9f619-609">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="9f619-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="9f619-610">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-610">**Old behavior**</span></span>

<span data-ttu-id="9f619-611">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="9f619-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="9f619-612">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-612">**New behavior**</span></span>

<span data-ttu-id="9f619-613">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="9f619-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="9f619-614">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-614">**Why**</span></span>

<span data-ttu-id="9f619-615">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="9f619-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="9f619-616">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-616">**Mitigations**</span></span>

<span data-ttu-id="9f619-617">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="9f619-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="9f619-618">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="9f619-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="9f619-619">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="9f619-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="9f619-620">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="9f619-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="9f619-621">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-621">**Old behavior**</span></span>

<span data-ttu-id="9f619-622">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9f619-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="9f619-623">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-623">**New behavior**</span></span>

<span data-ttu-id="9f619-624">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="9f619-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="9f619-625">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-625">**Why**</span></span>

<span data-ttu-id="9f619-626">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="9f619-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="9f619-627">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="9f619-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="9f619-628">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-628">**Mitigations**</span></span>

<span data-ttu-id="9f619-629">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="9f619-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="9f619-630">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="9f619-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="9f619-631">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="9f619-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="9f619-632">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-632">**Old behavior**</span></span>

<span data-ttu-id="9f619-633">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="9f619-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="9f619-634">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-634">**New behavior**</span></span>

<span data-ttu-id="9f619-635">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="9f619-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="9f619-636">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="9f619-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="9f619-637">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-637">**Why**</span></span>

<span data-ttu-id="9f619-638">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="9f619-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="9f619-639">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-639">**Mitigations**</span></span>

<span data-ttu-id="9f619-640">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9f619-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="9f619-641">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="9f619-641">Metadata API changes</span></span>

[<span data-ttu-id="9f619-642">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="9f619-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9f619-643">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-643">**New behavior**</span></span>

<span data-ttu-id="9f619-644">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="9f619-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="9f619-645">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-645">**Why**</span></span>

<span data-ttu-id="9f619-646">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="9f619-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="9f619-647">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-647">**Mitigations**</span></span>

<span data-ttu-id="9f619-648">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="9f619-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="9f619-649">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="9f619-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="9f619-650">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="9f619-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9f619-651">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-651">**New behavior**</span></span>

<span data-ttu-id="9f619-652">Les méthodes d’extension spécifiques au fournisseur seront aplaties :</span><span class="sxs-lookup"><span data-stu-id="9f619-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="9f619-653">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-653">**Why**</span></span>

<span data-ttu-id="9f619-654">Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="9f619-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="9f619-655">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-655">**Mitigations**</span></span>

<span data-ttu-id="9f619-656">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="9f619-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="9f619-657">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="9f619-658">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="9f619-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="9f619-659">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-659">**Old behavior**</span></span>

<span data-ttu-id="9f619-660">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="9f619-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9f619-661">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-661">**New behavior**</span></span>

<span data-ttu-id="9f619-662">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="9f619-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9f619-663">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-663">**Why**</span></span>

<span data-ttu-id="9f619-664">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="9f619-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="9f619-665">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-665">**Mitigations**</span></span>

<span data-ttu-id="9f619-666">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f619-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="9f619-667">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="9f619-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="9f619-668">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9f619-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="9f619-669">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-669">**Old behavior**</span></span>

<span data-ttu-id="9f619-670">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="9f619-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="9f619-671">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-671">**New behavior**</span></span>

<span data-ttu-id="9f619-672">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="9f619-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="9f619-673">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-673">**Why**</span></span>

<span data-ttu-id="9f619-674">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="9f619-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="9f619-675">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-675">**Mitigations**</span></span>

<span data-ttu-id="9f619-676">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="9f619-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9f619-677">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9f619-678">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="9f619-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="9f619-679">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-679">**Old behavior**</span></span>

<span data-ttu-id="9f619-680">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="9f619-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="9f619-681">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-681">**New behavior**</span></span>

<span data-ttu-id="9f619-682">Maintenant, les valeurs Guid sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="9f619-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="9f619-683">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-683">**Why**</span></span>

<span data-ttu-id="9f619-684">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="9f619-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="9f619-685">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="9f619-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9f619-686">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-686">**Mitigations**</span></span>

<span data-ttu-id="9f619-687">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="9f619-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="9f619-688">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="9f619-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="9f619-689">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="9f619-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9f619-690">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="9f619-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9f619-691">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="9f619-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="9f619-692">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-692">**Old behavior**</span></span>

<span data-ttu-id="9f619-693">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="9f619-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="9f619-694">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="9f619-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="9f619-695">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-695">**New behavior**</span></span>

<span data-ttu-id="9f619-696">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="9f619-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="9f619-697">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-697">**Why**</span></span>

<span data-ttu-id="9f619-698">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="9f619-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9f619-699">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-699">**Mitigations**</span></span>

<span data-ttu-id="9f619-700">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="9f619-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="9f619-701">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="9f619-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="9f619-702">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="9f619-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="9f619-703">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="9f619-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="9f619-704">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="9f619-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="9f619-705">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-705">**Old behavior**</span></span>

<span data-ttu-id="9f619-706">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="9f619-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="9f619-707">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-707">**New behavior**</span></span>

<span data-ttu-id="9f619-708">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="9f619-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="9f619-709">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-709">**Why**</span></span>

<span data-ttu-id="9f619-710">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="9f619-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="9f619-711">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="9f619-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="9f619-712">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-712">**Mitigations**</span></span>

<span data-ttu-id="9f619-713">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="9f619-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="9f619-714">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="9f619-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="9f619-715">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="9f619-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="9f619-716">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="9f619-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="9f619-717">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="9f619-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="9f619-718">Suivi de problème no 16400</span><span class="sxs-lookup"><span data-stu-id="9f619-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="9f619-719">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-719">**Old behavior**</span></span>

<span data-ttu-id="9f619-720">Avant EF Core 3.0, `UseRowNumberForPaging` pouvait être utilisé pour générer SQL pour la pagination qui est compatible avec SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="9f619-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="9f619-721">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-721">**New behavior**</span></span>

<span data-ttu-id="9f619-722">À compter de EF Core 3.0, EF génère uniquement SQL pour la pagination qui est uniquement compatible avec les versions de SQL Server ultérieures.</span><span class="sxs-lookup"><span data-stu-id="9f619-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="9f619-723">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-723">**Why**</span></span>

<span data-ttu-id="9f619-724">Nous effectuons cette modification, car [SQL Server 2008 n’est plus un produit pris en charge](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) et la mise à jour de cette fonctionnalité pour fonctionner avec les modifications de requête effectuées dans EF Core 3.0 est un travail significatif.</span><span class="sxs-lookup"><span data-stu-id="9f619-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="9f619-725">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-725">**Mitigations**</span></span>

<span data-ttu-id="9f619-726">Nous vous recommandons d’effectuer la mise à jour vers une version plus récente de SQL Server ou d’utiliser un niveau de compatibilité plus élevé, afin que le SQL généré soit pris en charge.</span><span class="sxs-lookup"><span data-stu-id="9f619-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="9f619-727">Cela dit, si vous ne pouvez pas faire cela, veuillez [commenter le problème de suivi](https://github.com/aspnet/EntityFrameworkCore/issues/16400) en incluant des détails.</span><span class="sxs-lookup"><span data-stu-id="9f619-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="9f619-728">Nous pourrions revisiter cette décision en fonction des commentaires.</span><span class="sxs-lookup"><span data-stu-id="9f619-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="9f619-729">Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9f619-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="9f619-730">Suivi du problème no 16119</span><span class="sxs-lookup"><span data-stu-id="9f619-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="9f619-731">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-731">**Old behavior**</span></span>

<span data-ttu-id="9f619-732">`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.</span><span class="sxs-lookup"><span data-stu-id="9f619-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="9f619-733">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-733">**New behavior**</span></span>

<span data-ttu-id="9f619-734">Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="9f619-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="9f619-735">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-735">**Why**</span></span>

<span data-ttu-id="9f619-736">Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="9f619-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="9f619-737">Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.</span><span class="sxs-lookup"><span data-stu-id="9f619-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="9f619-738">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-738">**Mitigations**</span></span>

<span data-ttu-id="9f619-739">Mettre à jour des extensions pour suivre le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="9f619-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="9f619-740">Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f619-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="9f619-741">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="9f619-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="9f619-742">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="9f619-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="9f619-743">**Changement**</span><span class="sxs-lookup"><span data-stu-id="9f619-743">**Change**</span></span>

<span data-ttu-id="9f619-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="9f619-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="9f619-745">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-745">**Why**</span></span>

<span data-ttu-id="9f619-746">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="9f619-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="9f619-747">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-747">**Mitigations**</span></span>

<span data-ttu-id="9f619-748">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="9f619-748">Use the new name.</span></span> <span data-ttu-id="9f619-749">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="9f619-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="9f619-750">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="9f619-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="9f619-751">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="9f619-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="9f619-752">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-752">**Old behavior**</span></span>

<span data-ttu-id="9f619-753">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="9f619-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="9f619-754">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="9f619-755">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-755">**New behavior**</span></span>

<span data-ttu-id="9f619-756">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="9f619-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="9f619-757">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="9f619-758">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-758">**Why**</span></span>

<span data-ttu-id="9f619-759">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="9f619-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="9f619-760">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-760">**Mitigations**</span></span>

<span data-ttu-id="9f619-761">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="9f619-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="9f619-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="9f619-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="9f619-763">Suivi du problème no 15997</span><span class="sxs-lookup"><span data-stu-id="9f619-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="9f619-764">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-764">**Old behavior**</span></span>

<span data-ttu-id="9f619-765">Avant EF Core 3.0, ces méthodes étaient protégées.</span><span class="sxs-lookup"><span data-stu-id="9f619-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="9f619-766">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-766">**New behavior**</span></span>

<span data-ttu-id="9f619-767">À compter d’EF Core 3.0, ces méthodes sont publiques.</span><span class="sxs-lookup"><span data-stu-id="9f619-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="9f619-768">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-768">**Why**</span></span>

<span data-ttu-id="9f619-769">Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide.</span><span class="sxs-lookup"><span data-stu-id="9f619-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="9f619-770">Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.</span><span class="sxs-lookup"><span data-stu-id="9f619-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="9f619-771">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-771">**Mitigations**</span></span>

<span data-ttu-id="9f619-772">Modifiez l’accessibilité de n’importe quel remplacements.</span><span class="sxs-lookup"><span data-stu-id="9f619-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="9f619-773">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9f619-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="9f619-774">Suivi du problème no 11506</span><span class="sxs-lookup"><span data-stu-id="9f619-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="9f619-775">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-775">**Old behavior**</span></span>

<span data-ttu-id="9f619-776">Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.</span><span class="sxs-lookup"><span data-stu-id="9f619-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="9f619-777">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-777">**New behavior**</span></span>

<span data-ttu-id="9f619-778">À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="9f619-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="9f619-779">Ce qui signifie que la dépendance ne circule pas de manière transitive dans d’autres projets et que vous ne pouvez plus, par défaut, faire référence à son assembly.</span><span class="sxs-lookup"><span data-stu-id="9f619-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="9f619-780">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-780">**Why**</span></span>

<span data-ttu-id="9f619-781">Ce package est uniquement destiné à être utilisé au moment du design.</span><span class="sxs-lookup"><span data-stu-id="9f619-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="9f619-782">Les applications déployées ne doivent pas y faire référence.</span><span class="sxs-lookup"><span data-stu-id="9f619-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="9f619-783">Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="9f619-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="9f619-784">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-784">**Mitigations**</span></span>

<span data-ttu-id="9f619-785">Si vous devez référencer ce package pour écraser le comportement au moment du design d’EF Core, vous pouvez mettre à jour des métadonnées d’élément PackageReference dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="9f619-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="9f619-786">Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées.</span><span class="sxs-lookup"><span data-stu-id="9f619-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="9f619-787">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9f619-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="9f619-788">Suivi du problème no 14824</span><span class="sxs-lookup"><span data-stu-id="9f619-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="9f619-789">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-789">**Old behavior**</span></span>

<span data-ttu-id="9f619-790">Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="9f619-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="9f619-791">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-791">**New behavior**</span></span>

<span data-ttu-id="9f619-792">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9f619-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9f619-793">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-793">**Why**</span></span>

<span data-ttu-id="9f619-794">La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="9f619-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="9f619-795">Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="9f619-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="9f619-796">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-796">**Mitigations**</span></span>

<span data-ttu-id="9f619-797">SQLitePCL.raw version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="9f619-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9f619-798">Pour plus d’informations, consultez les [notes de publication](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="9f619-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="9f619-799">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9f619-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="9f619-800">Suivi de problème no 14825</span><span class="sxs-lookup"><span data-stu-id="9f619-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="9f619-801">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-801">**Old behavior**</span></span>

<span data-ttu-id="9f619-802">Les packages spatiaux dépendaient précédemment de la version 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="9f619-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="9f619-803">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-803">**New behavior**</span></span>

<span data-ttu-id="9f619-804">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9f619-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9f619-805">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-805">**Why**</span></span>

<span data-ttu-id="9f619-806">La version 2.0.0 de NetTopologySuite vise à résoudre plusieurs problèmes de facilité d’utilisation rencontrés par les utilisateurs EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f619-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="9f619-807">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-807">**Mitigations**</span></span>

<span data-ttu-id="9f619-808">NetTopologySuite version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="9f619-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9f619-809">Pour plus d’informations, consultez les [notes de publication](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="9f619-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="9f619-810">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="9f619-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="9f619-811">Suivi de problème no 13573</span><span class="sxs-lookup"><span data-stu-id="9f619-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="9f619-812">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-812">**Old behavior**</span></span>

<span data-ttu-id="9f619-813">Un type d’entité avec plusieurs propriétés de navigation unidirectionnelle autoréférencées et les clés étrangères correspondantes a été incorrectement configuré en tant que relation unique.</span><span class="sxs-lookup"><span data-stu-id="9f619-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="9f619-814">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-814">For example:</span></span>

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

<span data-ttu-id="9f619-815">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-815">**New behavior**</span></span>

<span data-ttu-id="9f619-816">Ce scénario est maintenant détecté dans la génération de modèle et une exception est levée, indiquant que le modèle est ambigu.</span><span class="sxs-lookup"><span data-stu-id="9f619-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="9f619-817">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-817">**Why**</span></span>

<span data-ttu-id="9f619-818">Le modèle résultant est ambigu et probablement erroné dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="9f619-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="9f619-819">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-819">**Mitigations**</span></span>

<span data-ttu-id="9f619-820">Utilisez la configuration complète de la relation.</span><span class="sxs-lookup"><span data-stu-id="9f619-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="9f619-821">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9f619-821">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="9f619-822">DbFunction. Schema étant null ou une chaîne vide, il est configuré pour être dans le schéma par défaut du modèle</span><span class="sxs-lookup"><span data-stu-id="9f619-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="9f619-823">#12757 du problème de suivi</span><span class="sxs-lookup"><span data-stu-id="9f619-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="9f619-824">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-824">**Old behavior**</span></span>

<span data-ttu-id="9f619-825">Un DbFunction configuré avec un schéma sous forme de chaîne vide a été traité comme une fonction intégrée sans schéma.</span><span class="sxs-lookup"><span data-stu-id="9f619-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="9f619-826">Par exemple, le code suivant `DatePart` mappera la `DATEPART` fonction CLR à la fonction intégrée sur SqlServer.</span><span class="sxs-lookup"><span data-stu-id="9f619-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="9f619-827">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="9f619-827">**New behavior**</span></span>

<span data-ttu-id="9f619-828">Tous les mappages de DbFunction sont considérés comme mappés à des fonctions définies par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9f619-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="9f619-829">Par conséquent, la valeur de chaîne vide placerait la fonction dans le schéma par défaut pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="9f619-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="9f619-830">Ce peut être le schéma configuré explicitement via l’API `modelBuilder.HasDefaultSchema()` Fluent `dbo` ou sinon.</span><span class="sxs-lookup"><span data-stu-id="9f619-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="9f619-831">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="9f619-831">**Why**</span></span>

<span data-ttu-id="9f619-832">Le schéma précédemment vide était un moyen de traiter que la fonction est intégrée, mais cette logique s’applique uniquement à SqlServer, où les fonctions intégrées n’appartiennent à aucun schéma.</span><span class="sxs-lookup"><span data-stu-id="9f619-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="9f619-833">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="9f619-833">**Mitigations**</span></span>

<span data-ttu-id="9f619-834">Configurez la traduction de DbFunction manuellement pour la mapper à une fonction intégrée.</span><span class="sxs-lookup"><span data-stu-id="9f619-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
