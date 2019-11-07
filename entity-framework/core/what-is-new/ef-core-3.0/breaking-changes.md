---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f02825f5303959997dca6e14e4efe64020b3cb22
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655878"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="3b95a-102">Dernières modifications incluses dans EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="3b95a-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="3b95a-103">Les modifications d’API et de comportement suivantes peuvent bloquer les applications existantes lors de leur mise à niveau vers 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b95a-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="3b95a-104">Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="3b95a-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="3b95a-105">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="3b95a-105">Summary</span></span>

| <span data-ttu-id="3b95a-106">**Modification avec rupture**</span><span class="sxs-lookup"><span data-stu-id="3b95a-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="3b95a-107">**Effet**</span><span class="sxs-lookup"><span data-stu-id="3b95a-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="3b95a-108">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="3b95a-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="3b95a-109">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-109">High</span></span>       |
| [<span data-ttu-id="3b95a-110">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="3b95a-111">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-111">High</span></span>      |
| [<span data-ttu-id="3b95a-112">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="3b95a-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="3b95a-113">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-113">High</span></span>      |
| [<span data-ttu-id="3b95a-114">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="3b95a-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="3b95a-115">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-115">High</span></span>      |
| [<span data-ttu-id="3b95a-116">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="3b95a-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="3b95a-117">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-117">High</span></span>      |
| [<span data-ttu-id="3b95a-118">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="3b95a-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="3b95a-119">Élevé</span><span class="sxs-lookup"><span data-stu-id="3b95a-119">High</span></span>      |
| [<span data-ttu-id="3b95a-120">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b95a-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="3b95a-121">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-121">Medium</span></span>      |
| [<span data-ttu-id="3b95a-122">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="3b95a-123">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-123">Medium</span></span>      |
| [<span data-ttu-id="3b95a-124">Le chargement hâtif des entités associées se produit désormais dans une seule requête</span><span class="sxs-lookup"><span data-stu-id="3b95a-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="3b95a-125">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-125">Medium</span></span>      |
| [<span data-ttu-id="3b95a-126">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="3b95a-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="3b95a-127">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-127">Medium</span></span>      |
| [<span data-ttu-id="3b95a-128">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="3b95a-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="3b95a-129">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-129">Medium</span></span>      |
| [<span data-ttu-id="3b95a-130">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="3b95a-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="3b95a-131">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-131">Medium</span></span>      |
| [<span data-ttu-id="3b95a-132">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="3b95a-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="3b95a-133">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-133">Medium</span></span>      |
| [<span data-ttu-id="3b95a-134">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="3b95a-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="3b95a-135">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-135">Medium</span></span>      |
| [<span data-ttu-id="3b95a-136">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="3b95a-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="3b95a-137">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-137">Medium</span></span>      |
| [<span data-ttu-id="3b95a-138">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="3b95a-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="3b95a-139">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-139">Medium</span></span>      |
| [<span data-ttu-id="3b95a-140">La méthode FromSql quand elle est utilisée avec une procédure stockée ne peut pas être composée</span><span class="sxs-lookup"><span data-stu-id="3b95a-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="3b95a-141">Moyenne</span><span class="sxs-lookup"><span data-stu-id="3b95a-141">Medium</span></span>      |
| [<span data-ttu-id="3b95a-142">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="3b95a-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="3b95a-143">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-143">Low</span></span>      |
| [<span data-ttu-id="3b95a-144">~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli</span><span class="sxs-lookup"><span data-stu-id="3b95a-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="3b95a-145">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-145">Low</span></span>      |
| [<span data-ttu-id="3b95a-146">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="3b95a-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="3b95a-147">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-147">Low</span></span>      |
| [<span data-ttu-id="3b95a-148">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="3b95a-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="3b95a-149">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-149">Low</span></span>      |
| [<span data-ttu-id="3b95a-150">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="3b95a-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="3b95a-151">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-151">Low</span></span>      |
| [<span data-ttu-id="3b95a-152">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="3b95a-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="3b95a-153">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-153">Low</span></span>      |
| [<span data-ttu-id="3b95a-154">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="3b95a-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="3b95a-155">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-155">Low</span></span>      |
| [<span data-ttu-id="3b95a-156">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3b95a-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="3b95a-157">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-157">Low</span></span>      |
| [<span data-ttu-id="3b95a-158">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="3b95a-159">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-159">Low</span></span>      |
| [<span data-ttu-id="3b95a-160">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="3b95a-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="3b95a-161">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-161">Low</span></span>      |
| [<span data-ttu-id="3b95a-162">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="3b95a-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="3b95a-163">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-163">Low</span></span>      |
| [<span data-ttu-id="3b95a-164">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3b95a-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="3b95a-165">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-165">Low</span></span>      |
| [<span data-ttu-id="3b95a-166">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="3b95a-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="3b95a-167">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-167">Low</span></span>      |
| [<span data-ttu-id="3b95a-168">Les clés de tableaux d’octets et de chaînes ne sont pas générées par client par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="3b95a-169">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-169">Low</span></span>      |
| [<span data-ttu-id="3b95a-170">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="3b95a-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="3b95a-171">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-171">Low</span></span>      |
| [<span data-ttu-id="3b95a-172">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="3b95a-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="3b95a-173">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-173">Low</span></span>      |
| [<span data-ttu-id="3b95a-174">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="3b95a-175">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-175">Low</span></span>      |
| [<span data-ttu-id="3b95a-176">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="3b95a-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="3b95a-177">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-177">Low</span></span>      |
| [<span data-ttu-id="3b95a-178">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="3b95a-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="3b95a-179">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-179">Low</span></span>      |
| [<span data-ttu-id="3b95a-180">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3b95a-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="3b95a-181">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-181">Low</span></span>      |
| [<span data-ttu-id="3b95a-182">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="3b95a-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="3b95a-183">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-183">Low</span></span>      |
| [<span data-ttu-id="3b95a-184">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="3b95a-185">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-185">Low</span></span>      |
| [<span data-ttu-id="3b95a-186">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3b95a-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="3b95a-187">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-187">Low</span></span>      |
| [<span data-ttu-id="3b95a-188">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="3b95a-189">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-189">Low</span></span>      |
| [<span data-ttu-id="3b95a-190">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="3b95a-191">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-191">Low</span></span>      |
| [<span data-ttu-id="3b95a-192">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="3b95a-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="3b95a-193">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-193">Low</span></span>      |
| [<span data-ttu-id="3b95a-194">Les infos/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3b95a-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="3b95a-195">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-195">Low</span></span>      |
| [<span data-ttu-id="3b95a-196">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="3b95a-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="3b95a-197">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-197">Low</span></span>      |
| [<span data-ttu-id="3b95a-198">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="3b95a-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="3b95a-199">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-199">Low</span></span>      |
| [<span data-ttu-id="3b95a-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="3b95a-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="3b95a-201">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-201">Low</span></span>      |
| [<span data-ttu-id="3b95a-202">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3b95a-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="3b95a-203">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-203">Low</span></span>      |
| [<span data-ttu-id="3b95a-204">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="3b95a-205">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-205">Low</span></span>      |
| [<span data-ttu-id="3b95a-206">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="3b95a-207">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-207">Low</span></span>      |
| [<span data-ttu-id="3b95a-208">Microsoft. Data. SqlClient est utilisé à la place de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="3b95a-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="3b95a-209">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-209">Low</span></span>      |
| [<span data-ttu-id="3b95a-210">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="3b95a-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="3b95a-211">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-211">Low</span></span>      |
| [<span data-ttu-id="3b95a-212">DbFunction. Schema étant null ou une chaîne vide, il est configuré pour être dans le schéma par défaut du modèle</span><span class="sxs-lookup"><span data-stu-id="3b95a-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="3b95a-213">Faible</span><span class="sxs-lookup"><span data-stu-id="3b95a-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="3b95a-214">Les requêtes LINQ ne sont plus évaluées sur le client</span><span class="sxs-lookup"><span data-stu-id="3b95a-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="3b95a-215">[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="3b95a-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="3b95a-216">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-216">**Old behavior**</span></span>

<span data-ttu-id="3b95a-217">Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.</span><span class="sxs-lookup"><span data-stu-id="3b95a-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="3b95a-218">Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="3b95a-219">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-219">**New behavior**</span></span>

<span data-ttu-id="3b95a-220">À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).</span><span class="sxs-lookup"><span data-stu-id="3b95a-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="3b95a-221">Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="3b95a-222">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-222">**Why**</span></span>

<span data-ttu-id="3b95a-223">L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.</span><span class="sxs-lookup"><span data-stu-id="3b95a-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="3b95a-224">Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.</span><span class="sxs-lookup"><span data-stu-id="3b95a-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="3b95a-225">Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.</span><span class="sxs-lookup"><span data-stu-id="3b95a-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="3b95a-226">Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="3b95a-227">Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="3b95a-228">De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.</span><span class="sxs-lookup"><span data-stu-id="3b95a-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="3b95a-229">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-229">**Mitigations**</span></span>

<span data-ttu-id="3b95a-230">Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="3b95a-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="3b95a-231">EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="3b95a-232">Suivi de problème no 15498</span><span class="sxs-lookup"><span data-stu-id="3b95a-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="3b95a-233">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-233">**Old behavior**</span></span>

<span data-ttu-id="3b95a-234">Avant la version 3.0, EF Core ciblait .NET Standard 2.0 et s’exécutait sur toutes les plateformes qui prennent en charge cette norme, y compris .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b95a-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="3b95a-235">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-235">**New behavior**</span></span>

<span data-ttu-id="3b95a-236">À partir de la version 3.0, EF Core cible .NET Standard 2.1 et s’exécute sur toutes les plateformes qui prennent en charge cette norme.</span><span class="sxs-lookup"><span data-stu-id="3b95a-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="3b95a-237">Cela n'inclut pas .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b95a-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="3b95a-238">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-238">**Why**</span></span>

<span data-ttu-id="3b95a-239">Cela fait partie d’une décision stratégique dans les technologies .NET de concentrer l’énergie sur .NET Core et d’autres plateformes .NET modernes, telles que Xamarin.</span><span class="sxs-lookup"><span data-stu-id="3b95a-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="3b95a-240">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-240">**Mitigations**</span></span>

<span data-ttu-id="3b95a-241">Envisagez de passer à une plateforme .NET moderne.</span><span class="sxs-lookup"><span data-stu-id="3b95a-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="3b95a-242">Si ce n’est pas possible, continuez à utiliser EF Core 2.1 ou EF Core 2.2, qui prennent tous deux en charge .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3b95a-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="3b95a-243">Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b95a-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="3b95a-244">Annonces de suivi de problème n°325</span><span class="sxs-lookup"><span data-stu-id="3b95a-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="3b95a-245">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-245">**Old behavior**</span></span>

<span data-ttu-id="3b95a-246">Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b95a-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="3b95a-247">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-247">**New behavior**</span></span>

<span data-ttu-id="3b95a-248">À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b95a-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="3b95a-249">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-249">**Why**</span></span>

<span data-ttu-id="3b95a-250">Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non.</span><span class="sxs-lookup"><span data-stu-id="3b95a-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="3b95a-251">De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.</span><span class="sxs-lookup"><span data-stu-id="3b95a-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="3b95a-252">Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.</span><span class="sxs-lookup"><span data-stu-id="3b95a-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="3b95a-253">Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.</span><span class="sxs-lookup"><span data-stu-id="3b95a-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="3b95a-254">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-254">**Mitigations**</span></span>

<span data-ttu-id="3b95a-255">Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.</span><span class="sxs-lookup"><span data-stu-id="3b95a-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="3b95a-256">L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="3b95a-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="3b95a-257">Suivi du problème n° 14016</span><span class="sxs-lookup"><span data-stu-id="3b95a-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="3b95a-258">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-258">**Old behavior**</span></span>

<span data-ttu-id="3b95a-259">Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="3b95a-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="3b95a-260">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-260">**New behavior**</span></span>

<span data-ttu-id="3b95a-261">À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global.</span><span class="sxs-lookup"><span data-stu-id="3b95a-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="3b95a-262">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-262">**Why**</span></span>

<span data-ttu-id="3b95a-263">Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b95a-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="3b95a-264">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-264">**Mitigations**</span></span>

<span data-ttu-id="3b95a-265">Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` comme outil général :</span><span class="sxs-lookup"><span data-stu-id="3b95a-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="3b95a-266">Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="3b95a-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="3b95a-267">FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés</span><span class="sxs-lookup"><span data-stu-id="3b95a-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="3b95a-268">Suivi du problème no 10996</span><span class="sxs-lookup"><span data-stu-id="3b95a-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="3b95a-269">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-269">**Old behavior**</span></span>

<span data-ttu-id="3b95a-270">Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.</span><span class="sxs-lookup"><span data-stu-id="3b95a-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="3b95a-271">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-271">**New behavior**</span></span>

<span data-ttu-id="3b95a-272">À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3b95a-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="3b95a-273">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="3b95a-274">Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="3b95a-275">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="3b95a-276">Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.</span><span class="sxs-lookup"><span data-stu-id="3b95a-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="3b95a-277">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-277">**Why**</span></span>

<span data-ttu-id="3b95a-278">Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="3b95a-279">De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.</span><span class="sxs-lookup"><span data-stu-id="3b95a-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="3b95a-280">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-280">**Mitigations**</span></span>

<span data-ttu-id="3b95a-281">Utilisez plutôt les nouveaux noms de méthode.</span><span class="sxs-lookup"><span data-stu-id="3b95a-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="3b95a-282">La méthode FromSql quand elle est utilisée avec une procédure stockée ne peut pas être composée</span><span class="sxs-lookup"><span data-stu-id="3b95a-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="3b95a-283">#15392 du problème de suivi</span><span class="sxs-lookup"><span data-stu-id="3b95a-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="3b95a-284">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-284">**Old behavior**</span></span>

<span data-ttu-id="3b95a-285">Avant le EF Core 3,0, la méthode FromSql a tenté de détecter si le SQL passé peut être composé.</span><span class="sxs-lookup"><span data-stu-id="3b95a-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="3b95a-286">Il a fait l’évaluation du client lorsque le SQL n’était pas composable comme une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="3b95a-287">La requête suivante a fonctionné en exécutant la procédure stockée sur le serveur et en procédant à l’opération FirstOrDefault côté client.</span><span class="sxs-lookup"><span data-stu-id="3b95a-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="3b95a-288">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-288">**New behavior**</span></span>

<span data-ttu-id="3b95a-289">À compter de EF Core 3,0, EF Core n’essaiera pas d’analyser le SQL.</span><span class="sxs-lookup"><span data-stu-id="3b95a-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="3b95a-290">Par conséquent, si vous effectuez une composition après FromSqlRaw/FromSqlInterpolated, EF Core compose le SQL en provoquant une sous-requête.</span><span class="sxs-lookup"><span data-stu-id="3b95a-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="3b95a-291">Par conséquent, si vous utilisez une procédure stockée avec la composition, vous obtiendrez une exception pour la syntaxe SQL non valide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="3b95a-292">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-292">**Why**</span></span>

<span data-ttu-id="3b95a-293">EF Core 3,0 ne prend pas en charge l’évaluation automatique du client, car il s’agit d’une erreur, comme expliqué [ici](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="3b95a-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="3b95a-294">**Atténuation**</span><span class="sxs-lookup"><span data-stu-id="3b95a-294">**Mitigation**</span></span>

<span data-ttu-id="3b95a-295">Si vous utilisez une procédure stockée dans FromSqlRaw/FromSqlInterpolated, vous savez qu’elle ne peut pas être composée. vous pouvez donc ajouter __AsEnumerable/AsAsyncEnumerable__ juste après l’appel de la méthode FromSql pour éviter toute composition côté serveur.</span><span class="sxs-lookup"><span data-stu-id="3b95a-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="3b95a-296">Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête</span><span class="sxs-lookup"><span data-stu-id="3b95a-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="3b95a-297">Suivi de problème no 15704</span><span class="sxs-lookup"><span data-stu-id="3b95a-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="3b95a-298">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-298">**Old behavior**</span></span>

<span data-ttu-id="3b95a-299">Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.</span><span class="sxs-lookup"><span data-stu-id="3b95a-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="3b95a-300">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-300">**New behavior**</span></span>

<span data-ttu-id="3b95a-301">À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="3b95a-302">Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="3b95a-303">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-303">**Why**</span></span>

<span data-ttu-id="3b95a-304">La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.</span><span class="sxs-lookup"><span data-stu-id="3b95a-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="3b95a-305">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-305">**Mitigations**</span></span>

<span data-ttu-id="3b95a-306">Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="3b95a-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="3b95a-307">Les requêtes sans suivi ne procèdent plus à la résolution de l’identité</span><span class="sxs-lookup"><span data-stu-id="3b95a-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="3b95a-308">Suivi de problème no 13518</span><span class="sxs-lookup"><span data-stu-id="3b95a-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="3b95a-309">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-309">**Old behavior**</span></span>

<span data-ttu-id="3b95a-310">Avant EF Core 3.0, la même instance d’entité était utilisée pour chaque occurrence d’une entité avec un type et un ID donnés.</span><span class="sxs-lookup"><span data-stu-id="3b95a-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="3b95a-311">Cela correspond au comportement des requêtes de suivi.</span><span class="sxs-lookup"><span data-stu-id="3b95a-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="3b95a-312">Par exemple, la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="3b95a-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="3b95a-313">retournait la même instance `Category` pour chaque `Product` associé à la catégorie donnée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="3b95a-314">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-314">**New behavior**</span></span>

<span data-ttu-id="3b95a-315">À compter de EF Core 3.0, différentes instances d’entité sont créées lorsqu’une entité avec un type et un ID donnés est rencontrée à différents emplacements dans le graphe retourné.</span><span class="sxs-lookup"><span data-stu-id="3b95a-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="3b95a-316">Par exemple, la requête ci-dessus retourne à présent une nouvelle instance `Category` pour chaque `Product` même si deux produits sont associés à la même catégorie.</span><span class="sxs-lookup"><span data-stu-id="3b95a-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="3b95a-317">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-317">**Why**</span></span>

<span data-ttu-id="3b95a-318">La résolution de l’identité (autrement dit, le fait de déterminer qu’une entité a les mêmes type et ID qu’une entité précédemment rencontrée) améliore les performances et la surcharge de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="3b95a-319">Cela agit généralement à l’opposé de la raison pour laquelle aucune requête de suivi n’est utilisée en premier lieu.</span><span class="sxs-lookup"><span data-stu-id="3b95a-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="3b95a-320">En outre, même si la résolution de l’identité peut parfois être utile, elle n’est pas nécessaire si les entités doivent être sérialisées et envoyées à un client, ce qui est courant pour les requêtes sans suivi.</span><span class="sxs-lookup"><span data-stu-id="3b95a-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="3b95a-321">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-321">**Mitigations**</span></span>

<span data-ttu-id="3b95a-322">Utilisez une requête de suivi si la résolution de l’identité est requise.</span><span class="sxs-lookup"><span data-stu-id="3b95a-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="3b95a-323">~~L’exécution de requêtes est enregistrée au niveau du débogage~~ rétabli</span><span class="sxs-lookup"><span data-stu-id="3b95a-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="3b95a-324">Suivi de problème n°14523</span><span class="sxs-lookup"><span data-stu-id="3b95a-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="3b95a-325">Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application.</span><span class="sxs-lookup"><span data-stu-id="3b95a-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="3b95a-326">Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :</span><span class="sxs-lookup"><span data-stu-id="3b95a-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="3b95a-327">Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités</span><span class="sxs-lookup"><span data-stu-id="3b95a-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="3b95a-328">Suivi de problème n°12378</span><span class="sxs-lookup"><span data-stu-id="3b95a-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="3b95a-329">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-329">**Old behavior**</span></span>

<span data-ttu-id="3b95a-330">Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="3b95a-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="3b95a-331">Généralement, ces valeurs temporaires étaient de grands nombres négatifs.</span><span class="sxs-lookup"><span data-stu-id="3b95a-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="3b95a-332">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-332">**New behavior**</span></span>

<span data-ttu-id="3b95a-333">À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="3b95a-334">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-334">**Why**</span></span>

<span data-ttu-id="3b95a-335">Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="3b95a-336">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-336">**Mitigations**</span></span>

<span data-ttu-id="3b95a-337">Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="3b95a-338">Vous pouvez éviter cela en :</span><span class="sxs-lookup"><span data-stu-id="3b95a-338">This can be avoided by:</span></span>
* <span data-ttu-id="3b95a-339">N’utilisant pas de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="3b95a-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="3b95a-340">Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="3b95a-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="3b95a-341">Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="3b95a-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="3b95a-342">Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.</span><span class="sxs-lookup"><span data-stu-id="3b95a-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="3b95a-343">DetectChanges honore les valeurs de clés générées par le magasin</span><span class="sxs-lookup"><span data-stu-id="3b95a-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="3b95a-344">Suivi de problème n°14616</span><span class="sxs-lookup"><span data-stu-id="3b95a-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="3b95a-345">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-345">**Old behavior**</span></span>

<span data-ttu-id="3b95a-346">Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="3b95a-347">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-347">**New behavior**</span></span>

<span data-ttu-id="3b95a-348">À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="3b95a-349">Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="3b95a-350">Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="3b95a-351">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-351">**Why**</span></span>

<span data-ttu-id="3b95a-352">Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.</span><span class="sxs-lookup"><span data-stu-id="3b95a-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="3b95a-353">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-353">**Mitigations**</span></span>

<span data-ttu-id="3b95a-354">Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.</span><span class="sxs-lookup"><span data-stu-id="3b95a-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="3b95a-355">La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.</span><span class="sxs-lookup"><span data-stu-id="3b95a-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="3b95a-356">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="3b95a-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="3b95a-357">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="3b95a-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="3b95a-358">Les suppressions en cascade se produisent désormais immédiatement par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="3b95a-359">Suivi de problème n°10114</span><span class="sxs-lookup"><span data-stu-id="3b95a-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="3b95a-360">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-360">**Old behavior**</span></span>

<span data-ttu-id="3b95a-361">Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="3b95a-362">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-362">**New behavior**</span></span>

<span data-ttu-id="3b95a-363">À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="3b95a-364">Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="3b95a-365">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-365">**Why**</span></span>

<span data-ttu-id="3b95a-366">Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="3b95a-367">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-367">**Mitigations**</span></span>

<span data-ttu-id="3b95a-368">Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="3b95a-369">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="3b95a-370">Le chargement hâtif des entités associées se produit désormais dans une seule requête</span><span class="sxs-lookup"><span data-stu-id="3b95a-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="3b95a-371">#18022 du problème de suivi</span><span class="sxs-lookup"><span data-stu-id="3b95a-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="3b95a-372">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-372">**Old behavior**</span></span>

<span data-ttu-id="3b95a-373">Avant le 3,0, le chargement des navigations de collections à l’aide d’opérateurs `Include` entraînait la génération de plusieurs requêtes sur une base de données relationnelle, une pour chaque type d’entité associée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="3b95a-374">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-374">**New behavior**</span></span>

<span data-ttu-id="3b95a-375">À partir de 3,0, EF Core génère une requête unique avec des JOINTUREs sur des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="3b95a-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="3b95a-376">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-376">**Why**</span></span>

<span data-ttu-id="3b95a-377">L’émission de plusieurs requêtes pour implémenter une seule requête LINQ a entraîné de nombreux problèmes, notamment des performances négatives lorsque plusieurs allers-retours de base de données étaient nécessaires et des problèmes de cohérence des données à mesure que chaque requête pouvait observer un état différent de la base de données.</span><span class="sxs-lookup"><span data-stu-id="3b95a-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="3b95a-378">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-378">**Mitigations**</span></span>

<span data-ttu-id="3b95a-379">Bien qu’techniquement, il ne s’agit pas d’une modification avec rupture, elle peut avoir un impact considérable sur les performances de l’application lorsqu’une seule requête contient un grand nombre d' `Include` opérateur dans les navigations de collection.</span><span class="sxs-lookup"><span data-stu-id="3b95a-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="3b95a-380">Pour plus d’informations et pour réécrire des requêtes de manière plus efficace, [consultez ce commentaire](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="3b95a-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="3b95a-381">La sémantique de DeleteBehavior.Restrict est désormais plus propre</span><span class="sxs-lookup"><span data-stu-id="3b95a-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="3b95a-382">Suivi de problème no 12661</span><span class="sxs-lookup"><span data-stu-id="3b95a-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="3b95a-383">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-383">**Old behavior**</span></span>

<span data-ttu-id="3b95a-384">Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.</span><span class="sxs-lookup"><span data-stu-id="3b95a-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="3b95a-385">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-385">**New behavior**</span></span>

<span data-ttu-id="3b95a-386">À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.</span><span class="sxs-lookup"><span data-stu-id="3b95a-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="3b95a-387">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-387">**Why**</span></span>

<span data-ttu-id="3b95a-388">Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.</span><span class="sxs-lookup"><span data-stu-id="3b95a-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="3b95a-389">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-389">**Mitigations**</span></span>

<span data-ttu-id="3b95a-390">Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="3b95a-391">Les types de requêtes sont regroupés avec les types d’entités</span><span class="sxs-lookup"><span data-stu-id="3b95a-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="3b95a-392">Suivi de problème n°14194</span><span class="sxs-lookup"><span data-stu-id="3b95a-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="3b95a-393">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-393">**Old behavior**</span></span>

<span data-ttu-id="3b95a-394">Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/keyless-entity-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="3b95a-395">Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).</span><span class="sxs-lookup"><span data-stu-id="3b95a-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="3b95a-396">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-396">**New behavior**</span></span>

<span data-ttu-id="3b95a-397">Un type de requête devient maintenant simplement un type d’entité sans clé primaire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="3b95a-398">Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="3b95a-399">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-399">**Why**</span></span>

<span data-ttu-id="3b95a-400">Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="3b95a-401">Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="3b95a-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="3b95a-402">De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.</span><span class="sxs-lookup"><span data-stu-id="3b95a-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="3b95a-403">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-403">**Mitigations**</span></span>

<span data-ttu-id="3b95a-404">Les parties suivantes de l’API sont désormais obsolètes :</span><span class="sxs-lookup"><span data-stu-id="3b95a-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="3b95a-405">**`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.</span><span class="sxs-lookup"><span data-stu-id="3b95a-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="3b95a-406">Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.</span><span class="sxs-lookup"><span data-stu-id="3b95a-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="3b95a-407">**`DbQuery<>`** - Utilisez plutôt `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="3b95a-408">**`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="3b95a-409">L’API de configuration pour les relations de type détenu a changé</span><span class="sxs-lookup"><span data-stu-id="3b95a-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="3b95a-410">[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="3b95a-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="3b95a-411">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-411">**Old behavior**</span></span>

<span data-ttu-id="3b95a-412">Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="3b95a-413">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-413">**New behavior**</span></span>

<span data-ttu-id="3b95a-414">À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="3b95a-415">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="3b95a-416">La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.</span><span class="sxs-lookup"><span data-stu-id="3b95a-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="3b95a-417">En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="3b95a-418">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-418">For example:</span></span>

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

<span data-ttu-id="3b95a-419">De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.</span><span class="sxs-lookup"><span data-stu-id="3b95a-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="3b95a-420">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-420">**Why**</span></span>

<span data-ttu-id="3b95a-421">Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.</span><span class="sxs-lookup"><span data-stu-id="3b95a-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="3b95a-422">Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="3b95a-423">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-423">**Mitigations**</span></span>

<span data-ttu-id="3b95a-424">Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="3b95a-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="3b95a-425">Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives</span><span class="sxs-lookup"><span data-stu-id="3b95a-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="3b95a-426">Suivi du problème no 9005</span><span class="sxs-lookup"><span data-stu-id="3b95a-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="3b95a-427">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-427">**Old behavior**</span></span>

<span data-ttu-id="3b95a-428">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="3b95a-428">Consider the following model:</span></span>
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
<span data-ttu-id="3b95a-429">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="3b95a-430">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-430">**New behavior**</span></span>

<span data-ttu-id="3b95a-431">À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.</span><span class="sxs-lookup"><span data-stu-id="3b95a-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="3b95a-432">Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="3b95a-433">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-433">**Mitigations**</span></span>

<span data-ttu-id="3b95a-434">Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="3b95a-435">Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="3b95a-436">Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété</span><span class="sxs-lookup"><span data-stu-id="3b95a-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="3b95a-437">Suivi du problème no 14154</span><span class="sxs-lookup"><span data-stu-id="3b95a-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="3b95a-438">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-438">**Old behavior**</span></span>

<span data-ttu-id="3b95a-439">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="3b95a-439">Consider the following model:</span></span>
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
<span data-ttu-id="3b95a-440">Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.</span><span class="sxs-lookup"><span data-stu-id="3b95a-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="3b95a-441">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-441">**New behavior**</span></span>

<span data-ttu-id="3b95a-442">À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="3b95a-443">Sinon, une exception est levée pendant la validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="3b95a-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="3b95a-444">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-444">**Why**</span></span>

<span data-ttu-id="3b95a-445">Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="3b95a-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="3b95a-446">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-446">**Mitigations**</span></span>

<span data-ttu-id="3b95a-447">Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence.</span><span class="sxs-lookup"><span data-stu-id="3b95a-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="3b95a-448">Vous pouvez en créer une dans un état fantôme :</span><span class="sxs-lookup"><span data-stu-id="3b95a-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="3b95a-449">Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés</span><span class="sxs-lookup"><span data-stu-id="3b95a-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="3b95a-450">Suivi du problème no 13998</span><span class="sxs-lookup"><span data-stu-id="3b95a-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="3b95a-451">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-451">**Old behavior**</span></span>

<span data-ttu-id="3b95a-452">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="3b95a-452">Consider the following model:</span></span>
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

<span data-ttu-id="3b95a-453">Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="3b95a-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="3b95a-454">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-454">**New behavior**</span></span>

<span data-ttu-id="3b95a-455">À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="3b95a-456">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-456">**Why**</span></span>

<span data-ttu-id="3b95a-457">L’ancien comportement n’était pas attendu.</span><span class="sxs-lookup"><span data-stu-id="3b95a-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="3b95a-458">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-458">**Mitigations**</span></span>

<span data-ttu-id="3b95a-459">La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :</span><span class="sxs-lookup"><span data-stu-id="3b95a-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="3b95a-460">La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale</span><span class="sxs-lookup"><span data-stu-id="3b95a-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="3b95a-461">Suivi de problème n°13274</span><span class="sxs-lookup"><span data-stu-id="3b95a-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="3b95a-462">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-462">**Old behavior**</span></span>

<span data-ttu-id="3b95a-463">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="3b95a-463">Consider the following model:</span></span>
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
<span data-ttu-id="3b95a-464">Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.</span><span class="sxs-lookup"><span data-stu-id="3b95a-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="3b95a-465">Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="3b95a-466">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-466">**New behavior**</span></span>

<span data-ttu-id="3b95a-467">À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.</span><span class="sxs-lookup"><span data-stu-id="3b95a-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="3b95a-468">Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="3b95a-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="3b95a-469">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-469">For example:</span></span>

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

<span data-ttu-id="3b95a-470">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-470">**Why**</span></span>

<span data-ttu-id="3b95a-471">Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.</span><span class="sxs-lookup"><span data-stu-id="3b95a-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="3b95a-472">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-472">**Mitigations**</span></span>

<span data-ttu-id="3b95a-473">Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.</span><span class="sxs-lookup"><span data-stu-id="3b95a-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="3b95a-474">La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3b95a-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="3b95a-475">Suivi du problème no 14218</span><span class="sxs-lookup"><span data-stu-id="3b95a-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="3b95a-476">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-476">**Old behavior**</span></span>

<span data-ttu-id="3b95a-477">Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.</span><span class="sxs-lookup"><span data-stu-id="3b95a-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="3b95a-478">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-478">**New behavior**</span></span>

<span data-ttu-id="3b95a-479">À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.</span><span class="sxs-lookup"><span data-stu-id="3b95a-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="3b95a-480">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-480">**Why**</span></span>

<span data-ttu-id="3b95a-481">Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="3b95a-482">Le nouveau comportement correspond également à EF6.</span><span class="sxs-lookup"><span data-stu-id="3b95a-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="3b95a-483">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-483">**Mitigations**</span></span>

<span data-ttu-id="3b95a-484">Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :</span><span class="sxs-lookup"><span data-stu-id="3b95a-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="3b95a-485">Chaque propriété utilise la génération indépendante de clé de type entier en mémoire</span><span class="sxs-lookup"><span data-stu-id="3b95a-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="3b95a-486">Suivi de problème n°6872</span><span class="sxs-lookup"><span data-stu-id="3b95a-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="3b95a-487">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-487">**Old behavior**</span></span>

<span data-ttu-id="3b95a-488">Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="3b95a-489">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-489">**New behavior**</span></span>

<span data-ttu-id="3b95a-490">À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="3b95a-491">De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="3b95a-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="3b95a-492">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-492">**Why**</span></span>

<span data-ttu-id="3b95a-493">Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="3b95a-494">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-494">**Mitigations**</span></span>

<span data-ttu-id="3b95a-495">Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3b95a-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="3b95a-496">Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="3b95a-497">Les champs de stockage sont utilisés par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-497">Backing fields are used by default</span></span>

[<span data-ttu-id="3b95a-498">Suivi de problème n°12430</span><span class="sxs-lookup"><span data-stu-id="3b95a-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="3b95a-499">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-499">**Old behavior**</span></span>

<span data-ttu-id="3b95a-500">Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3b95a-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="3b95a-501">L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.</span><span class="sxs-lookup"><span data-stu-id="3b95a-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="3b95a-502">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-502">**New behavior**</span></span>

<span data-ttu-id="3b95a-503">Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="3b95a-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="3b95a-504">Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.</span><span class="sxs-lookup"><span data-stu-id="3b95a-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="3b95a-505">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-505">**Why**</span></span>

<span data-ttu-id="3b95a-506">Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.</span><span class="sxs-lookup"><span data-stu-id="3b95a-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="3b95a-507">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-507">**Mitigations**</span></span>

<span data-ttu-id="3b95a-508">Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="3b95a-509">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="3b95a-510">Erreur si plusieurs champs de stockage compatibles sont détectés</span><span class="sxs-lookup"><span data-stu-id="3b95a-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="3b95a-511">Suivi de problème n°12523</span><span class="sxs-lookup"><span data-stu-id="3b95a-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="3b95a-512">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-512">**Old behavior**</span></span>

<span data-ttu-id="3b95a-513">Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.</span><span class="sxs-lookup"><span data-stu-id="3b95a-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="3b95a-514">Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.</span><span class="sxs-lookup"><span data-stu-id="3b95a-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="3b95a-515">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-515">**New behavior**</span></span>

<span data-ttu-id="3b95a-516">À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="3b95a-517">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-517">**Why**</span></span>

<span data-ttu-id="3b95a-518">Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.</span><span class="sxs-lookup"><span data-stu-id="3b95a-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="3b95a-519">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-519">**Mitigations**</span></span>

<span data-ttu-id="3b95a-520">Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="3b95a-521">Par exemple, à l’aide de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="3b95a-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="3b95a-522">Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ</span><span class="sxs-lookup"><span data-stu-id="3b95a-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="3b95a-523">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-523">**Old behavior**</span></span>

<span data-ttu-id="3b95a-524">Avant le EF Core 3,0, une propriété pourrait être spécifiée par une valeur de chaîne et si aucune propriété portant ce nom n’a été trouvée sur le type .NET, EF Core essaiera de la faire correspondre à un champ à l’aide de règles de Convention.</span><span class="sxs-lookup"><span data-stu-id="3b95a-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="3b95a-525">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-525">**New behavior**</span></span>

<span data-ttu-id="3b95a-526">À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="3b95a-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="3b95a-527">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-527">**Why**</span></span>

<span data-ttu-id="3b95a-528">Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.</span><span class="sxs-lookup"><span data-stu-id="3b95a-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="3b95a-529">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-529">**Mitigations**</span></span>

<span data-ttu-id="3b95a-530">Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.</span><span class="sxs-lookup"><span data-stu-id="3b95a-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="3b95a-531">Dans une version ultérieure de EF Core après 3,0, nous prévoyons de réactiver explicitement la configuration d’un nom de champ différent du nom de la propriété (voir le problème [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)) :</span><span class="sxs-lookup"><span data-stu-id="3b95a-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="3b95a-532">AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3b95a-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="3b95a-533">Suivi de problème no 14756</span><span class="sxs-lookup"><span data-stu-id="3b95a-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="3b95a-534">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-534">**Old behavior**</span></span>

<span data-ttu-id="3b95a-535">Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3b95a-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="3b95a-536">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-536">**New behavior**</span></span>

<span data-ttu-id="3b95a-537">À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="3b95a-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="3b95a-538">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-538">**Why**</span></span>

<span data-ttu-id="3b95a-539">Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="3b95a-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="3b95a-540">Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.</span><span class="sxs-lookup"><span data-stu-id="3b95a-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="3b95a-541">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-541">**Mitigations**</span></span>

<span data-ttu-id="3b95a-542">Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3b95a-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="3b95a-543">DbContext.Entry effectue maintenant un DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="3b95a-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="3b95a-544">Suivi de problème n°13552</span><span class="sxs-lookup"><span data-stu-id="3b95a-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="3b95a-545">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-545">**Old behavior**</span></span>

<span data-ttu-id="3b95a-546">Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="3b95a-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="3b95a-547">Cela garantissait que l’état exposé dans `EntityEntry` était à jour.</span><span class="sxs-lookup"><span data-stu-id="3b95a-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="3b95a-548">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-548">**New behavior**</span></span>

<span data-ttu-id="3b95a-549">À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="3b95a-550">Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="3b95a-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="3b95a-551">Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="3b95a-552">Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="3b95a-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="3b95a-553">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-553">**Why**</span></span>

<span data-ttu-id="3b95a-554">Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="3b95a-555">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-555">**Mitigations**</span></span>

<span data-ttu-id="3b95a-556">Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="3b95a-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="3b95a-557">Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="3b95a-558">Suivi de problème n°14617</span><span class="sxs-lookup"><span data-stu-id="3b95a-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="3b95a-559">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-559">**Old behavior**</span></span>

<span data-ttu-id="3b95a-560">Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.</span><span class="sxs-lookup"><span data-stu-id="3b95a-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="3b95a-561">Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="3b95a-562">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-562">**New behavior**</span></span>

<span data-ttu-id="3b95a-563">À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.</span><span class="sxs-lookup"><span data-stu-id="3b95a-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="3b95a-564">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-564">**Why**</span></span>

<span data-ttu-id="3b95a-565">Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.</span><span class="sxs-lookup"><span data-stu-id="3b95a-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="3b95a-566">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-566">**Mitigations**</span></span>

<span data-ttu-id="3b95a-567">Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.</span><span class="sxs-lookup"><span data-stu-id="3b95a-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="3b95a-568">Par exemple, avec l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="3b95a-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="3b95a-569">Ou avec des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="3b95a-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="3b95a-570">ILoggerFactory est désormais un service délimité</span><span class="sxs-lookup"><span data-stu-id="3b95a-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="3b95a-571">Suivi de problème n°14698</span><span class="sxs-lookup"><span data-stu-id="3b95a-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="3b95a-572">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-572">**Old behavior**</span></span>

<span data-ttu-id="3b95a-573">Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.</span><span class="sxs-lookup"><span data-stu-id="3b95a-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="3b95a-574">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-574">**New behavior**</span></span>

<span data-ttu-id="3b95a-575">À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.</span><span class="sxs-lookup"><span data-stu-id="3b95a-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="3b95a-576">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-576">**Why**</span></span>

<span data-ttu-id="3b95a-577">Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="3b95a-578">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-578">**Mitigations**</span></span>

<span data-ttu-id="3b95a-579">Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,</span><span class="sxs-lookup"><span data-stu-id="3b95a-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="3b95a-580">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="3b95a-580">This isn't common.</span></span>
<span data-ttu-id="3b95a-581">Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.</span><span class="sxs-lookup"><span data-stu-id="3b95a-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="3b95a-582">Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="3b95a-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="3b95a-583">Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées</span><span class="sxs-lookup"><span data-stu-id="3b95a-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="3b95a-584">Suivi de problème n°12780</span><span class="sxs-lookup"><span data-stu-id="3b95a-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="3b95a-585">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-585">**Old behavior**</span></span>

<span data-ttu-id="3b95a-586">Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="3b95a-587">Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="3b95a-588">Dans ces cas-là, toute tentative de chargement différé constituait une no-op.</span><span class="sxs-lookup"><span data-stu-id="3b95a-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="3b95a-589">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-589">**New behavior**</span></span>

<span data-ttu-id="3b95a-590">À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="3b95a-591">Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.</span><span class="sxs-lookup"><span data-stu-id="3b95a-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="3b95a-592">À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="3b95a-593">Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.</span><span class="sxs-lookup"><span data-stu-id="3b95a-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="3b95a-594">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-594">**Why**</span></span>

<span data-ttu-id="3b95a-595">Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="3b95a-596">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-596">**Mitigations**</span></span>

<span data-ttu-id="3b95a-597">Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.</span><span class="sxs-lookup"><span data-stu-id="3b95a-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="3b95a-598">La création excessive de fournisseurs de services internes est maintenant une erreur par défaut</span><span class="sxs-lookup"><span data-stu-id="3b95a-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="3b95a-599">Suivi de problème n°10236</span><span class="sxs-lookup"><span data-stu-id="3b95a-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="3b95a-600">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-600">**Old behavior**</span></span>

<span data-ttu-id="3b95a-601">Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="3b95a-602">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-602">**New behavior**</span></span>

<span data-ttu-id="3b95a-603">À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="3b95a-604">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-604">**Why**</span></span>

<span data-ttu-id="3b95a-605">Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.</span><span class="sxs-lookup"><span data-stu-id="3b95a-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="3b95a-606">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-606">**Mitigations**</span></span>

<span data-ttu-id="3b95a-607">En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="3b95a-608">Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="3b95a-609">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="3b95a-610">Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique</span><span class="sxs-lookup"><span data-stu-id="3b95a-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="3b95a-611">Suivi de problème no 9171</span><span class="sxs-lookup"><span data-stu-id="3b95a-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="3b95a-612">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-612">**Old behavior**</span></span>

<span data-ttu-id="3b95a-613">Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.</span><span class="sxs-lookup"><span data-stu-id="3b95a-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="3b95a-614">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="3b95a-615">Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="3b95a-616">En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="3b95a-617">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-617">**New behavior**</span></span>

<span data-ttu-id="3b95a-618">À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.</span><span class="sxs-lookup"><span data-stu-id="3b95a-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="3b95a-619">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-619">**Why**</span></span>

<span data-ttu-id="3b95a-620">L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="3b95a-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="3b95a-621">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-621">**Mitigations**</span></span>

<span data-ttu-id="3b95a-622">Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,</span><span class="sxs-lookup"><span data-stu-id="3b95a-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="3b95a-623">ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="3b95a-623">This is not common.</span></span>
<span data-ttu-id="3b95a-624">Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="3b95a-625">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="3b95a-626">Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask</span><span class="sxs-lookup"><span data-stu-id="3b95a-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="3b95a-627">Suivi du problème no 15184</span><span class="sxs-lookup"><span data-stu-id="3b95a-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="3b95a-628">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-628">**Old behavior**</span></span>

<span data-ttu-id="3b95a-629">Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :</span><span class="sxs-lookup"><span data-stu-id="3b95a-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="3b95a-630">`ValueGenerator.NextValueAsync()` (et classes dérivées)</span><span class="sxs-lookup"><span data-stu-id="3b95a-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="3b95a-631">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-631">**New behavior**</span></span>

<span data-ttu-id="3b95a-632">Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.</span><span class="sxs-lookup"><span data-stu-id="3b95a-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="3b95a-633">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-633">**Why**</span></span>

<span data-ttu-id="3b95a-634">Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.</span><span class="sxs-lookup"><span data-stu-id="3b95a-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="3b95a-635">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-635">**Mitigations**</span></span>

<span data-ttu-id="3b95a-636">Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.</span><span class="sxs-lookup"><span data-stu-id="3b95a-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="3b95a-637">Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.</span><span class="sxs-lookup"><span data-stu-id="3b95a-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="3b95a-638">Notez que cette opération annule la réduction d’allocation apportée par ce changement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="3b95a-639">L’annotation Relational:TypeMapping est désormais simplement TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3b95a-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="3b95a-640">Suivi de problème n°9913</span><span class="sxs-lookup"><span data-stu-id="3b95a-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="3b95a-641">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-641">**Old behavior**</span></span>

<span data-ttu-id="3b95a-642">Le nom d’annotation pour les annotations de mappage de types était « annotations ».</span><span class="sxs-lookup"><span data-stu-id="3b95a-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="3b95a-643">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-643">**New behavior**</span></span>

<span data-ttu-id="3b95a-644">Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».</span><span class="sxs-lookup"><span data-stu-id="3b95a-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="3b95a-645">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-645">**Why**</span></span>

<span data-ttu-id="3b95a-646">Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="3b95a-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="3b95a-647">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-647">**Mitigations**</span></span>

<span data-ttu-id="3b95a-648">Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.</span><span class="sxs-lookup"><span data-stu-id="3b95a-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="3b95a-649">La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="3b95a-650">ToTable sur un type dérivé lève une exception</span><span class="sxs-lookup"><span data-stu-id="3b95a-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="3b95a-651">Suivi de problème n°11811</span><span class="sxs-lookup"><span data-stu-id="3b95a-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="3b95a-652">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-652">**Old behavior**</span></span>

<span data-ttu-id="3b95a-653">Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="3b95a-654">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-654">**New behavior**</span></span>

<span data-ttu-id="3b95a-655">À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="3b95a-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="3b95a-656">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-656">**Why**</span></span>

<span data-ttu-id="3b95a-657">Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="3b95a-658">Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="3b95a-659">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-659">**Mitigations**</span></span>

<span data-ttu-id="3b95a-660">Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.</span><span class="sxs-lookup"><span data-stu-id="3b95a-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="3b95a-661">ForSqlServerHasIndex est remplacé par HasIndex</span><span class="sxs-lookup"><span data-stu-id="3b95a-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="3b95a-662">Suivi de problème n°12366</span><span class="sxs-lookup"><span data-stu-id="3b95a-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="3b95a-663">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-663">**Old behavior**</span></span>

<span data-ttu-id="3b95a-664">Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="3b95a-665">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-665">**New behavior**</span></span>

<span data-ttu-id="3b95a-666">À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.</span><span class="sxs-lookup"><span data-stu-id="3b95a-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="3b95a-667">Utilisez `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="3b95a-668">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-668">**Why**</span></span>

<span data-ttu-id="3b95a-669">Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.</span><span class="sxs-lookup"><span data-stu-id="3b95a-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="3b95a-670">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-670">**Mitigations**</span></span>

<span data-ttu-id="3b95a-671">Utilisez la nouvelle API, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="3b95a-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="3b95a-672">Changements apportés à l’API de métadonnées</span><span class="sxs-lookup"><span data-stu-id="3b95a-672">Metadata API changes</span></span>

[<span data-ttu-id="3b95a-673">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="3b95a-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3b95a-674">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-674">**New behavior**</span></span>

<span data-ttu-id="3b95a-675">Les propriétés suivantes ont été converties en méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="3b95a-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="3b95a-676">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-676">**Why**</span></span>

<span data-ttu-id="3b95a-677">Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="3b95a-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="3b95a-678">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-678">**Mitigations**</span></span>

<span data-ttu-id="3b95a-679">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="3b95a-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="3b95a-680">Modifications de l’API de métadonnées spécifiques au fournisseur</span><span class="sxs-lookup"><span data-stu-id="3b95a-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="3b95a-681">Suivi du problème no 214</span><span class="sxs-lookup"><span data-stu-id="3b95a-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3b95a-682">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-682">**New behavior**</span></span>

<span data-ttu-id="3b95a-683">Les méthodes d’extension spécifiques au fournisseur seront aplaties :</span><span class="sxs-lookup"><span data-stu-id="3b95a-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="3b95a-684">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-684">**Why**</span></span>

<span data-ttu-id="3b95a-685">Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.</span><span class="sxs-lookup"><span data-stu-id="3b95a-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="3b95a-686">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-686">**Mitigations**</span></span>

<span data-ttu-id="3b95a-687">Utilisez les nouvelles méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="3b95a-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="3b95a-688">EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="3b95a-689">Suivi de problème n°12151</span><span class="sxs-lookup"><span data-stu-id="3b95a-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="3b95a-690">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-690">**Old behavior**</span></span>

<span data-ttu-id="3b95a-691">Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.</span><span class="sxs-lookup"><span data-stu-id="3b95a-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3b95a-692">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-692">**New behavior**</span></span>

<span data-ttu-id="3b95a-693">À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.</span><span class="sxs-lookup"><span data-stu-id="3b95a-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3b95a-694">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-694">**Why**</span></span>

<span data-ttu-id="3b95a-695">Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="3b95a-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="3b95a-696">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-696">**Mitigations**</span></span>

<span data-ttu-id="3b95a-697">Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b95a-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="3b95a-698">Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="3b95a-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="3b95a-699">Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3b95a-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="3b95a-700">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-700">**Old behavior**</span></span>

<span data-ttu-id="3b95a-701">Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="3b95a-702">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-702">**New behavior**</span></span>

<span data-ttu-id="3b95a-703">À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="3b95a-704">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-704">**Why**</span></span>

<span data-ttu-id="3b95a-705">Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="3b95a-706">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-706">**Mitigations**</span></span>

<span data-ttu-id="3b95a-707">Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3b95a-708">Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3b95a-709">Suivi de problème #15078</span><span class="sxs-lookup"><span data-stu-id="3b95a-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="3b95a-710">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-710">**Old behavior**</span></span>

<span data-ttu-id="3b95a-711">Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b95a-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="3b95a-712">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-712">**New behavior**</span></span>

<span data-ttu-id="3b95a-713">Maintenant, les valeurs Guid sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="3b95a-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="3b95a-714">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-714">**Why**</span></span>

<span data-ttu-id="3b95a-715">Le format binaire des valeurs GUID n’est pas normalisé.</span><span class="sxs-lookup"><span data-stu-id="3b95a-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="3b95a-716">Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="3b95a-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3b95a-717">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-717">**Mitigations**</span></span>

<span data-ttu-id="3b95a-718">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="3b95a-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="3b95a-719">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="3b95a-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="3b95a-720">Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.</span><span class="sxs-lookup"><span data-stu-id="3b95a-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3b95a-721">Les valeurs char sont maintenant stockées au format TEXT sur SQLite</span><span class="sxs-lookup"><span data-stu-id="3b95a-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3b95a-722">Suivi de problème no 15020</span><span class="sxs-lookup"><span data-stu-id="3b95a-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="3b95a-723">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-723">**Old behavior**</span></span>

<span data-ttu-id="3b95a-724">Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite.</span><span class="sxs-lookup"><span data-stu-id="3b95a-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="3b95a-725">Par exemple, une valeur char de *A* était stockée comme valeur entière 65.</span><span class="sxs-lookup"><span data-stu-id="3b95a-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="3b95a-726">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-726">**New behavior**</span></span>

<span data-ttu-id="3b95a-727">Maintenant, les valeurs char sont stockées au format TEXT.</span><span class="sxs-lookup"><span data-stu-id="3b95a-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="3b95a-728">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-728">**Why**</span></span>

<span data-ttu-id="3b95a-729">Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.</span><span class="sxs-lookup"><span data-stu-id="3b95a-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3b95a-730">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-730">**Mitigations**</span></span>

<span data-ttu-id="3b95a-731">Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.</span><span class="sxs-lookup"><span data-stu-id="3b95a-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="3b95a-732">Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="3b95a-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="3b95a-733">Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.</span><span class="sxs-lookup"><span data-stu-id="3b95a-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="3b95a-734">Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante</span><span class="sxs-lookup"><span data-stu-id="3b95a-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="3b95a-735">Suivi de problème no 12978</span><span class="sxs-lookup"><span data-stu-id="3b95a-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="3b95a-736">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-736">**Old behavior**</span></span>

<span data-ttu-id="3b95a-737">Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="3b95a-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="3b95a-738">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-738">**New behavior**</span></span>

<span data-ttu-id="3b95a-739">Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).</span><span class="sxs-lookup"><span data-stu-id="3b95a-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="3b95a-740">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-740">**Why**</span></span>

<span data-ttu-id="3b95a-741">L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion.</span><span class="sxs-lookup"><span data-stu-id="3b95a-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="3b95a-742">L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.</span><span class="sxs-lookup"><span data-stu-id="3b95a-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="3b95a-743">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-743">**Mitigations**</span></span>

<span data-ttu-id="3b95a-744">Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais).</span><span class="sxs-lookup"><span data-stu-id="3b95a-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="3b95a-745">Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="3b95a-746">Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.</span><span class="sxs-lookup"><span data-stu-id="3b95a-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="3b95a-747">Le tableau de l’historique Migrations doit également être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="3b95a-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="3b95a-748">UseRowNumberForPaging a été supprimé</span><span class="sxs-lookup"><span data-stu-id="3b95a-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="3b95a-749">Suivi de problème no 16400</span><span class="sxs-lookup"><span data-stu-id="3b95a-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="3b95a-750">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-750">**Old behavior**</span></span>

<span data-ttu-id="3b95a-751">Avant EF Core 3.0, `UseRowNumberForPaging` pouvait être utilisé pour générer SQL pour la pagination qui est compatible avec SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="3b95a-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="3b95a-752">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-752">**New behavior**</span></span>

<span data-ttu-id="3b95a-753">À compter de EF Core 3.0, EF génère uniquement SQL pour la pagination qui est uniquement compatible avec les versions de SQL Server ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3b95a-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="3b95a-754">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-754">**Why**</span></span>

<span data-ttu-id="3b95a-755">Nous effectuons cette modification, car [SQL Server 2008 n’est plus un produit pris en charge](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) et la mise à jour de cette fonctionnalité pour fonctionner avec les modifications de requête effectuées dans EF Core 3.0 est un travail significatif.</span><span class="sxs-lookup"><span data-stu-id="3b95a-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="3b95a-756">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-756">**Mitigations**</span></span>

<span data-ttu-id="3b95a-757">Nous vous recommandons d’effectuer la mise à jour vers une version plus récente de SQL Server ou d’utiliser un niveau de compatibilité plus élevé, afin que le SQL généré soit pris en charge.</span><span class="sxs-lookup"><span data-stu-id="3b95a-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="3b95a-758">Cela dit, si vous ne pouvez pas faire cela, veuillez [commenter le problème de suivi](https://github.com/aspnet/EntityFrameworkCore/issues/16400) en incluant des détails.</span><span class="sxs-lookup"><span data-stu-id="3b95a-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="3b95a-759">Nous pourrions revisiter cette décision en fonction des commentaires.</span><span class="sxs-lookup"><span data-stu-id="3b95a-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="3b95a-760">Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3b95a-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="3b95a-761">Suivi du problème no 16119</span><span class="sxs-lookup"><span data-stu-id="3b95a-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="3b95a-762">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-762">**Old behavior**</span></span>

<span data-ttu-id="3b95a-763">`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.</span><span class="sxs-lookup"><span data-stu-id="3b95a-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="3b95a-764">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-764">**New behavior**</span></span>

<span data-ttu-id="3b95a-765">Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="3b95a-766">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-766">**Why**</span></span>

<span data-ttu-id="3b95a-767">Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="3b95a-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="3b95a-768">Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.</span><span class="sxs-lookup"><span data-stu-id="3b95a-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="3b95a-769">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-769">**Mitigations**</span></span>

<span data-ttu-id="3b95a-770">Mettre à jour des extensions pour suivre le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="3b95a-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="3b95a-771">Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b95a-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="3b95a-772">LogQueryPossibleExceptionWithAggregateOperator a été renommé</span><span class="sxs-lookup"><span data-stu-id="3b95a-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="3b95a-773">Suivi de problème #10985</span><span class="sxs-lookup"><span data-stu-id="3b95a-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="3b95a-774">**Changement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-774">**Change**</span></span>

<span data-ttu-id="3b95a-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="3b95a-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="3b95a-776">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-776">**Why**</span></span>

<span data-ttu-id="3b95a-777">Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="3b95a-778">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-778">**Mitigations**</span></span>

<span data-ttu-id="3b95a-779">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="3b95a-779">Use the new name.</span></span> <span data-ttu-id="3b95a-780">(Notez que le numéro d’ID événement n’a pas changé.)</span><span class="sxs-lookup"><span data-stu-id="3b95a-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="3b95a-781">Clarifier les noms de contrainte de clé étrangère dans l’API</span><span class="sxs-lookup"><span data-stu-id="3b95a-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="3b95a-782">Suivi de problème #10730</span><span class="sxs-lookup"><span data-stu-id="3b95a-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="3b95a-783">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-783">**Old behavior**</span></span>

<span data-ttu-id="3b95a-784">Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ».</span><span class="sxs-lookup"><span data-stu-id="3b95a-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="3b95a-785">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="3b95a-786">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-786">**New behavior**</span></span>

<span data-ttu-id="3b95a-787">À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ».</span><span class="sxs-lookup"><span data-stu-id="3b95a-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="3b95a-788">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="3b95a-789">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-789">**Why**</span></span>

<span data-ttu-id="3b95a-790">Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.</span><span class="sxs-lookup"><span data-stu-id="3b95a-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="3b95a-791">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-791">**Mitigations**</span></span>

<span data-ttu-id="3b95a-792">Utilisez le nouveau nom.</span><span class="sxs-lookup"><span data-stu-id="3b95a-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="3b95a-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques</span><span class="sxs-lookup"><span data-stu-id="3b95a-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="3b95a-794">Suivi du problème no 15997</span><span class="sxs-lookup"><span data-stu-id="3b95a-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="3b95a-795">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-795">**Old behavior**</span></span>

<span data-ttu-id="3b95a-796">Avant EF Core 3.0, ces méthodes étaient protégées.</span><span class="sxs-lookup"><span data-stu-id="3b95a-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="3b95a-797">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-797">**New behavior**</span></span>

<span data-ttu-id="3b95a-798">À compter d’EF Core 3.0, ces méthodes sont publiques.</span><span class="sxs-lookup"><span data-stu-id="3b95a-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="3b95a-799">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-799">**Why**</span></span>

<span data-ttu-id="3b95a-800">Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide.</span><span class="sxs-lookup"><span data-stu-id="3b95a-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="3b95a-801">Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.</span><span class="sxs-lookup"><span data-stu-id="3b95a-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="3b95a-802">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-802">**Mitigations**</span></span>

<span data-ttu-id="3b95a-803">Modifiez l’accessibilité de n’importe quel remplacements.</span><span class="sxs-lookup"><span data-stu-id="3b95a-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="3b95a-804">Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3b95a-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="3b95a-805">Suivi du problème no 11506</span><span class="sxs-lookup"><span data-stu-id="3b95a-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="3b95a-806">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-806">**Old behavior**</span></span>

<span data-ttu-id="3b95a-807">Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.</span><span class="sxs-lookup"><span data-stu-id="3b95a-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="3b95a-808">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-808">**New behavior**</span></span>

<span data-ttu-id="3b95a-809">À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3b95a-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="3b95a-810">Ce qui signifie que la dépendance ne circule pas de manière transitive dans d’autres projets et que vous ne pouvez plus, par défaut, faire référence à son assembly.</span><span class="sxs-lookup"><span data-stu-id="3b95a-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="3b95a-811">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-811">**Why**</span></span>

<span data-ttu-id="3b95a-812">Ce package est uniquement destiné à être utilisé au moment du design.</span><span class="sxs-lookup"><span data-stu-id="3b95a-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="3b95a-813">Les applications déployées ne doivent pas y faire référence.</span><span class="sxs-lookup"><span data-stu-id="3b95a-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="3b95a-814">Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="3b95a-815">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-815">**Mitigations**</span></span>

<span data-ttu-id="3b95a-816">Si vous devez référencer ce package pour écraser le comportement au moment du design d’EF Core, vous pouvez mettre à jour des métadonnées d’élément PackageReference dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="3b95a-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="3b95a-817">Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées.</span><span class="sxs-lookup"><span data-stu-id="3b95a-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="3b95a-818">SQLitePCL.raw mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="3b95a-819">Suivi du problème no 14824</span><span class="sxs-lookup"><span data-stu-id="3b95a-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="3b95a-820">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-820">**Old behavior**</span></span>

<span data-ttu-id="3b95a-821">Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="3b95a-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="3b95a-822">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-822">**New behavior**</span></span>

<span data-ttu-id="3b95a-823">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b95a-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3b95a-824">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-824">**Why**</span></span>

<span data-ttu-id="3b95a-825">La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="3b95a-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="3b95a-826">Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="3b95a-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="3b95a-827">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-827">**Mitigations**</span></span>

<span data-ttu-id="3b95a-828">SQLitePCL.raw version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="3b95a-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3b95a-829">Pour plus d’informations, consultez les [notes de publication](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="3b95a-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="3b95a-830">NetTopologySuite mis à jour vers la version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3b95a-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="3b95a-831">Suivi de problème no 14825</span><span class="sxs-lookup"><span data-stu-id="3b95a-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="3b95a-832">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-832">**Old behavior**</span></span>

<span data-ttu-id="3b95a-833">Les packages spatiaux dépendaient précédemment de la version 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="3b95a-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="3b95a-834">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-834">**New behavior**</span></span>

<span data-ttu-id="3b95a-835">Nous avons mis à jour notre package pour dépendre de la version 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3b95a-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3b95a-836">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-836">**Why**</span></span>

<span data-ttu-id="3b95a-837">La version 2.0.0 de NetTopologySuite vise à résoudre plusieurs problèmes de facilité d’utilisation rencontrés par les utilisateurs EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b95a-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="3b95a-838">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-838">**Mitigations**</span></span>

<span data-ttu-id="3b95a-839">NetTopologySuite version 2.0.0 inclut des changements cassants.</span><span class="sxs-lookup"><span data-stu-id="3b95a-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3b95a-840">Pour plus d’informations, consultez les [notes de publication](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="3b95a-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="3b95a-841">Microsoft. Data. SqlClient est utilisé à la place de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="3b95a-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="3b95a-842">#15636 du problème de suivi</span><span class="sxs-lookup"><span data-stu-id="3b95a-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="3b95a-843">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-843">**Old behavior**</span></span>

<span data-ttu-id="3b95a-844">Microsoft. EntityFrameworkCore. SqlServer était auparavant tributaire de System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3b95a-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="3b95a-845">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-845">**New behavior**</span></span>

<span data-ttu-id="3b95a-846">Nous avons mis à jour notre package pour dépendre de Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3b95a-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="3b95a-847">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-847">**Why**</span></span>

<span data-ttu-id="3b95a-848">Microsoft. Data. SqlClient est le pilote d’accès aux données phare pour la SQL Server à l’avenir, et System. Data. SqlClient ne représente plus le point de vue du développement.</span><span class="sxs-lookup"><span data-stu-id="3b95a-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="3b95a-849">Certaines fonctionnalités importantes, telles que Always Encrypted, sont uniquement disponibles sur Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="3b95a-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="3b95a-850">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-850">**Mitigations**</span></span>

<span data-ttu-id="3b95a-851">Si votre code prend une dépendance directe sur System. Data. SqlClient, vous devez le modifier pour qu’il fasse référence à Microsoft. Data. SqlClient à la place. étant donné que les deux packages maintiennent un très haut niveau de compatibilité d’API, il ne doit y avoir qu’une simple modification de package et d’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3b95a-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="3b95a-852">Plusieurs relations autoréférencées ambiguës doivent être configurées</span><span class="sxs-lookup"><span data-stu-id="3b95a-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="3b95a-853">Suivi de problème no 13573</span><span class="sxs-lookup"><span data-stu-id="3b95a-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="3b95a-854">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-854">**Old behavior**</span></span>

<span data-ttu-id="3b95a-855">Un type d’entité avec plusieurs propriétés de navigation unidirectionnelle autoréférencées et les clés étrangères correspondantes a été incorrectement configuré en tant que relation unique.</span><span class="sxs-lookup"><span data-stu-id="3b95a-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="3b95a-856">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-856">For example:</span></span>

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

<span data-ttu-id="3b95a-857">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-857">**New behavior**</span></span>

<span data-ttu-id="3b95a-858">Ce scénario est maintenant détecté dans la génération de modèle et une exception est levée, indiquant que le modèle est ambigu.</span><span class="sxs-lookup"><span data-stu-id="3b95a-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="3b95a-859">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-859">**Why**</span></span>

<span data-ttu-id="3b95a-860">Le modèle résultant est ambigu et probablement erroné dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="3b95a-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="3b95a-861">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-861">**Mitigations**</span></span>

<span data-ttu-id="3b95a-862">Utilisez la configuration complète de la relation.</span><span class="sxs-lookup"><span data-stu-id="3b95a-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="3b95a-863">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="3b95a-863">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="3b95a-864">DbFunction. Schema étant null ou une chaîne vide, il est configuré pour être dans le schéma par défaut du modèle</span><span class="sxs-lookup"><span data-stu-id="3b95a-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="3b95a-865">#12757 du problème de suivi</span><span class="sxs-lookup"><span data-stu-id="3b95a-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="3b95a-866">**Ancien comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-866">**Old behavior**</span></span>

<span data-ttu-id="3b95a-867">Un DbFunction configuré avec un schéma sous forme de chaîne vide a été traité comme une fonction intégrée sans schéma.</span><span class="sxs-lookup"><span data-stu-id="3b95a-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="3b95a-868">Par exemple, le code suivant mappe `DatePart` fonction CLR à `DATEPART` fonction intégrée sur SqlServer.</span><span class="sxs-lookup"><span data-stu-id="3b95a-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="3b95a-869">**Nouveau comportement**</span><span class="sxs-lookup"><span data-stu-id="3b95a-869">**New behavior**</span></span>

<span data-ttu-id="3b95a-870">Tous les mappages de DbFunction sont considérés comme mappés à des fonctions définies par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3b95a-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="3b95a-871">Par conséquent, la valeur de chaîne vide placerait la fonction dans le schéma par défaut pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="3b95a-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="3b95a-872">Ce peut être le schéma configuré explicitement via l’API Fluent `modelBuilder.HasDefaultSchema()` ou `dbo` dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="3b95a-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="3b95a-873">**Pourquoi ?**</span><span class="sxs-lookup"><span data-stu-id="3b95a-873">**Why**</span></span>

<span data-ttu-id="3b95a-874">Le schéma précédemment vide était un moyen de traiter que la fonction est intégrée, mais cette logique s’applique uniquement à SqlServer, où les fonctions intégrées n’appartiennent à aucun schéma.</span><span class="sxs-lookup"><span data-stu-id="3b95a-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="3b95a-875">**Atténuations**</span><span class="sxs-lookup"><span data-stu-id="3b95a-875">**Mitigations**</span></span>

<span data-ttu-id="3b95a-876">Configurez la traduction de DbFunction manuellement pour la mapper à une fonction intégrée.</span><span class="sxs-lookup"><span data-stu-id="3b95a-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
