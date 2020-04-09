---
title: Nouveautés d’EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417523"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="66dd6-102">Fonctionnalités incluses dans EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="66dd6-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="66dd6-103">Plateformes</span><span class="sxs-lookup"><span data-stu-id="66dd6-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="66dd6-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="66dd6-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="66dd6-105">Inclut la console, WPF, WinForms, ASP.NET 4, etc.</span><span class="sxs-lookup"><span data-stu-id="66dd6-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="66dd6-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="66dd6-106">.NET Standard 1.3</span></span>

<span data-ttu-id="66dd6-107">Inclut ASP.NET Core ciblant à la fois .NET Framework et .NET Core sur Windows, OSX et Linux.</span><span class="sxs-lookup"><span data-stu-id="66dd6-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="66dd6-108">Modélisation</span><span class="sxs-lookup"><span data-stu-id="66dd6-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="66dd6-109">Modélisation de base</span><span class="sxs-lookup"><span data-stu-id="66dd6-109">Basic modelling</span></span>

<span data-ttu-id="66dd6-110">Basée sur les entités OCT avec des propriétés get/set de types scalaires communs (`int`, `string`, etc.).</span><span class="sxs-lookup"><span data-stu-id="66dd6-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="66dd6-111">Relations et propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="66dd6-111">Relationships and navigation properties</span></span>

<span data-ttu-id="66dd6-112">Les relations un-à-plusieurs et un-à-zéro-ou un-à-un peuvent être spécifiées dans le modèle en fonction d’une clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="66dd6-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="66dd6-113">Les propriétés de navigation de types de collection ou de référence simples peuvent être associées à ces relations.</span><span class="sxs-lookup"><span data-stu-id="66dd6-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="66dd6-114">Conventions intégrées</span><span class="sxs-lookup"><span data-stu-id="66dd6-114">Built-in conventions</span></span>

<span data-ttu-id="66dd6-115">Ces conventions génèrent un modèle initial basé sur la forme des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="66dd6-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="66dd6-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="66dd6-116">Fluent API</span></span>

<span data-ttu-id="66dd6-117">Permet de remplacer la méthode `OnModelCreating` sur votre contexte pour configurer davantage le modèle détecté par convention.</span><span class="sxs-lookup"><span data-stu-id="66dd6-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="66dd6-118">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-118">Data annotations</span></span>

<span data-ttu-id="66dd6-119">Il s’agit d’attributs qui peuvent être ajoutés à vos propriétés/classes d’entité et qui influenceront le modèle EF.</span><span class="sxs-lookup"><span data-stu-id="66dd6-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="66dd6-120">Par exemple, l’ajout de la mention `[Required]` informera EF qu’une propriété est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="66dd6-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="66dd6-121">Mappage de tables relationnelles</span><span class="sxs-lookup"><span data-stu-id="66dd6-121">Relational Table mapping</span></span>

<span data-ttu-id="66dd6-122">Permet de mapper des entités à des tables ou des colonnes.</span><span class="sxs-lookup"><span data-stu-id="66dd6-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="66dd6-123">Génération de valeur de clé</span><span class="sxs-lookup"><span data-stu-id="66dd6-123">Key value generation</span></span>

<span data-ttu-id="66dd6-124">Inclut la génération côté client et la génération de base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="66dd6-125">Valeurs générées de base de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-125">Database generated values</span></span>

<span data-ttu-id="66dd6-126">Permet à la base de données de générer des valeurs par insertion (valeurs par défaut) ou par mise à jour (colonnes calculées).</span><span class="sxs-lookup"><span data-stu-id="66dd6-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="66dd6-127">Séquences dans SQL Server</span><span class="sxs-lookup"><span data-stu-id="66dd6-127">Sequences in SQL Server</span></span>

<span data-ttu-id="66dd6-128">Permet de définir des objets de la séquence dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="66dd6-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="66dd6-129">Contraintes uniques</span><span class="sxs-lookup"><span data-stu-id="66dd6-129">Unique constraints</span></span>

<span data-ttu-id="66dd6-130">Permet de définir d’autres clés ainsi que les relations ciblant ces clés.</span><span class="sxs-lookup"><span data-stu-id="66dd6-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="66dd6-131">Index</span><span class="sxs-lookup"><span data-stu-id="66dd6-131">Indexes</span></span>

<span data-ttu-id="66dd6-132">La définition d’index dans le modèle introduit automatiquement les index dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="66dd6-133">Les index uniques sont également pris en charge.</span><span class="sxs-lookup"><span data-stu-id="66dd6-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="66dd6-134">Propriétés d’état de clichés instantanés</span><span class="sxs-lookup"><span data-stu-id="66dd6-134">Shadow state properties</span></span>

<span data-ttu-id="66dd6-135">Permet de définir dans le modèle des propriétés qui ne sont pas déclarées ni stockées dans la classe .NET, mais qui peuvent être suivies et mises à jour par EF Core.</span><span class="sxs-lookup"><span data-stu-id="66dd6-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="66dd6-136">Elles sont couramment utilisées pour les propriétés de clé étrangère quand l’exposition de ces dernières dans l’objet n’est pas souhaitée.</span><span class="sxs-lookup"><span data-stu-id="66dd6-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="66dd6-137">Modèle d'héritage de table par hiérarchie</span><span class="sxs-lookup"><span data-stu-id="66dd6-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="66dd6-138">Permet d’enregistrer les entités d’une hiérarchie d’héritage dans une table unique à l’aide d’une colonne de discriminateur pour identifier le type d’entité pour un enregistrement donné dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="66dd6-139">Validation de modèle</span><span class="sxs-lookup"><span data-stu-id="66dd6-139">Model validation</span></span>

<span data-ttu-id="66dd6-140">Détecte les modèles non valides dans le modèle et fournit des messages d’erreur utiles.</span><span class="sxs-lookup"><span data-stu-id="66dd6-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="66dd6-141">Suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="66dd6-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="66dd6-142">Suivi des modifications par instantané</span><span class="sxs-lookup"><span data-stu-id="66dd6-142">Snapshot change tracking</span></span>

<span data-ttu-id="66dd6-143">Permet de détecter automatiquement les modifications apportées aux entités en comparant l’état actuel avec une copie (instantané) de l’état d’origine.</span><span class="sxs-lookup"><span data-stu-id="66dd6-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="66dd6-144">Suivi des modifications par notification</span><span class="sxs-lookup"><span data-stu-id="66dd6-144">Notification change tracking</span></span>

<span data-ttu-id="66dd6-145">Permet aux entités d’avertir le traceur de modifications dès que des valeurs de propriété sont modifiées.</span><span class="sxs-lookup"><span data-stu-id="66dd6-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="66dd6-146">État du suivi de l’accès</span><span class="sxs-lookup"><span data-stu-id="66dd6-146">Accessing tracked state</span></span>

<span data-ttu-id="66dd6-147">Via `DbContext.Entry` et `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="66dd6-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="66dd6-148">Attachement d’entités/graphes détachés</span><span class="sxs-lookup"><span data-stu-id="66dd6-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="66dd6-149">La nouvelle API `DbContext.AttachGraph` permet de rattacher des entités à un contexte pour pouvoir enregistrer des entités modifiées ou de nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="66dd6-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="66dd6-150">Enregistrement des données</span><span class="sxs-lookup"><span data-stu-id="66dd6-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="66dd6-151">Fonctionnalité d’enregistrement de base</span><span class="sxs-lookup"><span data-stu-id="66dd6-151">Basic save functionality</span></span>

<span data-ttu-id="66dd6-152">Permet de conserver les modifications apportées aux instances dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="66dd6-153">Accès concurrentiel optimiste</span><span class="sxs-lookup"><span data-stu-id="66dd6-153">Optimistic Concurrency</span></span>

<span data-ttu-id="66dd6-154">Évite le remplacement de modifications apportées par un autre utilisateur, sachant que les données ont été extraites de la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="66dd6-155">SaveChanges asynchrone</span><span class="sxs-lookup"><span data-stu-id="66dd6-155">Async SaveChanges</span></span>

<span data-ttu-id="66dd6-156">Peut libérer le thread actuel pour traiter d’autres requêtes pendant que la base de données traite les commandes émises à partir de `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="66dd6-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="66dd6-157">Transactions de base de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-157">Database Transactions</span></span>

<span data-ttu-id="66dd6-158">Signifie que `SaveChanges` est toujours atomique (en d’autres termes, soit sa réussite est complète, soit aucune modification n’est apportée à la base de données).</span><span class="sxs-lookup"><span data-stu-id="66dd6-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="66dd6-159">Il existe également des API liées aux transactions pour autoriser le partage de transactions entre des instances de contexte, etc.</span><span class="sxs-lookup"><span data-stu-id="66dd6-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="66dd6-160">Relationnel : traitement par lot d’instructions</span><span class="sxs-lookup"><span data-stu-id="66dd6-160">Relational: Batching of statements</span></span>

<span data-ttu-id="66dd6-161">Offre de meilleures performances en regroupant les différentes commandes INSERT/UPDATE/DELETE dans une seule boucle pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="66dd6-162">Requête</span><span class="sxs-lookup"><span data-stu-id="66dd6-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="66dd6-163">Prise en charge de base de LINQ</span><span class="sxs-lookup"><span data-stu-id="66dd6-163">Basic LINQ support</span></span>

<span data-ttu-id="66dd6-164">Offre la possibilité d’utiliser LINQ pour récupérer des données à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="66dd6-165">Évaluation du client/serveur mixte</span><span class="sxs-lookup"><span data-stu-id="66dd6-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="66dd6-166">Permet aux requêtes de contenir la logique qui ne peut pas être évaluée dans la base de données et qui doit par conséquent être évaluée une fois les données récupérées dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="66dd6-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="66dd6-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="66dd6-167">NoTracking</span></span>

<span data-ttu-id="66dd6-168">Permet d’accélérer l’exécution des requêtes quand le contexte n’a pas besoin de surveiller les changements apportés aux instances d’entité (cela s’avère utile si les résultats en lecture seule).</span><span class="sxs-lookup"><span data-stu-id="66dd6-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="66dd6-169">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="66dd6-169">Eager loading</span></span>

<span data-ttu-id="66dd6-170">Fournit les méthodes `Include` et `ThenInclude` pour identifier les données associées qui doivent également être extraites durant l’interrogation.</span><span class="sxs-lookup"><span data-stu-id="66dd6-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="66dd6-171">Requête asynchrone</span><span class="sxs-lookup"><span data-stu-id="66dd6-171">Async query</span></span>

<span data-ttu-id="66dd6-172">Peut libérer le thread actuel (et ses ressources associées) pour traiter d’autres requêtes pendant que la base de données traite la requête.</span><span class="sxs-lookup"><span data-stu-id="66dd6-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="66dd6-173">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="66dd6-173">Raw SQL queries</span></span>

<span data-ttu-id="66dd6-174">Fournit la méthode `DbSet.FromSql` pour utiliser des requêtes SQL brutes pour extraire des données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="66dd6-175">Ces requêtes peuvent également être composées à l’aide de LINQ.</span><span class="sxs-lookup"><span data-stu-id="66dd6-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="66dd6-176">Gestion du schéma de base de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="66dd6-177">API de création/suppression de base de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="66dd6-178">Elles sont principalement conçues tester l’emplacement où vous souhaitez créer/supprimer rapidement la base de données sans utiliser de migrations.</span><span class="sxs-lookup"><span data-stu-id="66dd6-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="66dd6-179">Migrations de base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="66dd6-179">Relational database migrations</span></span>

<span data-ttu-id="66dd6-180">Permet à un schéma de base de données relationnelle d’évoluer à travers le temps au fur et à mesure que votre modèle change.</span><span class="sxs-lookup"><span data-stu-id="66dd6-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="66dd6-181">Ingénierie à rebours à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-181">Reverse engineer from database</span></span>

<span data-ttu-id="66dd6-182">Permet de générer automatiquement un modèle EF basé sur un schéma de base de données relationnelle existante.</span><span class="sxs-lookup"><span data-stu-id="66dd6-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="66dd6-183">Fournisseurs de bases de données</span><span class="sxs-lookup"><span data-stu-id="66dd6-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="66dd6-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="66dd6-184">SQL Server</span></span>

<span data-ttu-id="66dd6-185">Se connecte à Microsoft SQL Server 2008 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="66dd6-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="66dd6-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="66dd6-186">SQLite</span></span>

<span data-ttu-id="66dd6-187">Se connecte à une base de données SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="66dd6-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="66dd6-188">En mémoire</span><span class="sxs-lookup"><span data-stu-id="66dd6-188">In-Memory</span></span>

<span data-ttu-id="66dd6-189">Fonctionnalité conçue pour tester facilement sans vous connecter à une base de données réelle.</span><span class="sxs-lookup"><span data-stu-id="66dd6-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="66dd6-190">Fournisseurs tiers</span><span class="sxs-lookup"><span data-stu-id="66dd6-190">3rd party providers</span></span>

<span data-ttu-id="66dd6-191">Plusieurs fournisseurs sont disponibles pour d’autres moteurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="66dd6-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="66dd6-192">Consultez [Fournisseurs de bases de données](../providers/index.md) pour en obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="66dd6-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
