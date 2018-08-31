---
title: Outils et extensions - EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e88bd01f79e05a9a3b0c0bbeecca89df178098a6
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152450"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="0184a-102">Outils et extensions EF Core</span><span class="sxs-lookup"><span data-stu-id="0184a-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="0184a-103">Les outils et les extensions fournissent des fonctionnalités supplémentaires pour Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0184a-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0184a-104">Les extensions sont générées par différentes sources et ne sont pas gérées dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0184a-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0184a-105">Quand vous envisagez une extension tierce, veillez à évaluer notamment la qualité, la gestion des licences, la compatibilité et la prise en charge pour vérifier qu’elle répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="0184a-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="0184a-106">Outils</span><span class="sxs-lookup"><span data-stu-id="0184a-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="0184a-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="0184a-107">LLBLGen Pro</span></span>

<span data-ttu-id="0184a-108">LLBLGen Pro est une solution de modélisation d’entités qui prend en charge Entity Framework et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0184a-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="0184a-109">Il vous permet de définir facilement votre modèle d’entités et de le mapper à votre base de données, en utilisant Database First ou Model First : vous pouvez donc commencer à écrire des requêtes immédiatement.</span><span class="sxs-lookup"><span data-stu-id="0184a-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="0184a-110">site web</span><span class="sxs-lookup"><span data-stu-id="0184a-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="0184a-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="0184a-111">Devart Entity Developer</span></span>

<span data-ttu-id="0184a-112">Entity Developer est un concepteur ORM pour ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access et LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="0184a-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="0184a-113">Vous pouvez utiliser les approches Model First et Database First pour concevoir votre modèle ORM, et pour générer du code C# ou Visual Basic .NET pour ce modèle.</span><span class="sxs-lookup"><span data-stu-id="0184a-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="0184a-114">Il introduit de nouvelles approches pour la conception de modèles ORM, accélère la productivité et facilite le développement d’applications de base de données.</span><span class="sxs-lookup"><span data-stu-id="0184a-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="0184a-115">site web</span><span class="sxs-lookup"><span data-stu-id="0184a-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="0184a-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="0184a-116">EF Core Power Tools</span></span>

<span data-ttu-id="0184a-117">Extensions Visual Studio 2017+.</span><span class="sxs-lookup"><span data-stu-id="0184a-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="0184a-118">Vous pouvez rétroconcevoir des classes DbContext et POCO à partir d’une base de données existante ou d’un projet de base de données SQL Server, et visualiser et examiner votre DbContext de différentes manières.</span><span class="sxs-lookup"><span data-stu-id="0184a-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="0184a-119">Wiki GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="0184a-120">Entity Framework Visual Editor</span><span class="sxs-lookup"><span data-stu-id="0184a-120">Entity Framework Visual Editor</span></span>

<span data-ttu-id="0184a-121">Extension de Visual Studio 2017 qui ajoute un concepteur ORM permettant de concevoir visuellement des classes Entity Framework 6, Core 2.0 et Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0184a-121">A Visual Studio 2017 extension that adds an ORM designer for visual design of Entity Framework 6, Core 2.0 and Core 2.1 classes.</span></span> <span data-ttu-id="0184a-122">Le code est généré à l’aide de modèles T4, donc il peut être totalement personnalisé pour répondre à tous les besoins.</span><span class="sxs-lookup"><span data-stu-id="0184a-122">Code is generated using T4 templates so can be completely customized to suit any needs.</span></span> <span data-ttu-id="0184a-123">Les associations d’héritage, unidirectionnelles et bidirectionnelles sont toutes prises en charge, tout comme les énumérations, la possibilité de colorer le code de vos classes et l’ajout de blocs de texte pour expliquer les parties potentiellement obscures de votre conception.</span><span class="sxs-lookup"><span data-stu-id="0184a-123">Inheritance, unidirectional and bidirectional associations are all supported, as are enumerations and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="0184a-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="0184a-124">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a><span data-ttu-id="0184a-125">Extensions</span><span class="sxs-lookup"><span data-stu-id="0184a-125">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="0184a-126">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="0184a-126">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="0184a-127">Plug-in pour Microsoft.EntityFrameworkCore qui prend en charge l’enregistrement automatique de l’historique de modification des données.</span><span class="sxs-lookup"><span data-stu-id="0184a-127">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="0184a-128">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-128">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="0184a-129">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="0184a-129">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="0184a-130">Extensions LINQ dynamiques pour Microsoft.EntityFrameworkCore qui ajoutent la prise en charge asynchrone</span><span class="sxs-lookup"><span data-stu-id="0184a-130">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="0184a-131">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-131">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="0184a-132">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="0184a-132">EFCore.Practices</span></span>

<span data-ttu-id="0184a-133">Tentative de capture de quelques bonnes pratiques dans une API qui prend en charge les tests, notamment un petit framework pour rechercher les requêtes N+1.</span><span class="sxs-lookup"><span data-stu-id="0184a-133">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="0184a-134">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-134">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="0184a-135">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="0184a-135">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="0184a-136">Bibliothèque de mise en cache de second niveau.</span><span class="sxs-lookup"><span data-stu-id="0184a-136">Second Level Caching Library.</span></span> <span data-ttu-id="0184a-137">La mise en cache de second niveau est un cache de requêtes.</span><span class="sxs-lookup"><span data-stu-id="0184a-137">Second level caching is a query cache.</span></span> <span data-ttu-id="0184a-138">Les résultats des commandes EF sont stockés dans le cache, de façon que les mêmes commandes EF récupèrent leurs données auprès du cache au lieu d’être réexécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="0184a-138">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="0184a-139">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-139">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="0184a-140">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="0184a-140">Detached.EntityFramework</span></span>

<span data-ttu-id="0184a-141">Charge et enregistre les graphes des entités détachées entières (l’entité avec ses entités enfants et ses listes).</span><span class="sxs-lookup"><span data-stu-id="0184a-141">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="0184a-142">Inspiré par [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="0184a-142">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="0184a-143">L’idée est également ajouter des plug-ins pour simplifier certaines tâches répétitives, comme l’audit et la pagination.</span><span class="sxs-lookup"><span data-stu-id="0184a-143">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="0184a-144">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-144">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="0184a-145">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="0184a-145">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="0184a-146">Récupérez la clé primaire (y compris les clés composites) de n’importe quelle entité sous la forme d’un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="0184a-146">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="0184a-147">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="0184a-148">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="0184a-148">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="0184a-149">Wrappers d’extension réactifs pour des versions observables à chaud des entités Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0184a-149">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="0184a-150">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-150">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="0184a-151">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="0184a-151">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="0184a-152">Ajoutez des déclencheurs à vos entités avec des événements d’insertion, de mise à jour et de suppression.</span><span class="sxs-lookup"><span data-stu-id="0184a-152">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="0184a-153">Il existe trois événements pour chacun de ceux-ci : avant, après et en cas d’échec.</span><span class="sxs-lookup"><span data-stu-id="0184a-153">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="0184a-154">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-154">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="0184a-155">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="0184a-155">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="0184a-156">Obtenez un accès typé à OriginalValue des propriétés de votre entité.</span><span class="sxs-lookup"><span data-stu-id="0184a-156">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="0184a-157">Les propriétés simples et complexes sont prises en charge ; la navigation et les collections ne le sont pas.</span><span class="sxs-lookup"><span data-stu-id="0184a-157">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="0184a-158">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-158">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="0184a-159">Geco</span><span class="sxs-lookup"><span data-stu-id="0184a-159">Geco</span></span>

<span data-ttu-id="0184a-160">Geco fournit un générateur de modèles inverses avec prise en charge de la pluralisation/singularisation et des modèles modifiables basés sur des chaînes interpolées C# 6.0, s’exécutant sur .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0184a-160">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="0184a-161">Il fournit également un générateur de scripts d’amorçage avec des scripts SQL Merge et un exécuteur de scripts.</span><span class="sxs-lookup"><span data-stu-id="0184a-161">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="0184a-162">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-162">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="0184a-163">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0184a-163">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="0184a-164">LinqKit.Microsoft.EntityFrameworkCore est un ensemble gratuit d’extensions pour les utilisateurs avancés LINQ to SQL et EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="0184a-164">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="0184a-165">Prend en charge Include(...) et IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="0184a-165">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="0184a-166">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-166">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="0184a-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0184a-167">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="0184a-168">NeinLinq.EntityFrameworkCore fournit des extensions pratiques pour l’utilisation de fournisseurs LINQ, comme Entity Framework, qui prennent en charge seulement un sous-ensemble mineur des fonctions .NET, la réutilisation de fonctions, la réécriture des requêtes (en les rendant même si nécessaire capables de traiter les valeurs null) et la génération de requêtes dynamiques avec des sélecteurs et des prédicats traduisibles.</span><span class="sxs-lookup"><span data-stu-id="0184a-168">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="0184a-169">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-169">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="0184a-170">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="0184a-170">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="0184a-171">Plug-in pour Microsoft.EntityFrameworkCore prenant en charge un référentiel, les modèles d’unités de travail et les bases de données multiples avec des transactions distribuées.</span><span class="sxs-lookup"><span data-stu-id="0184a-171">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="0184a-172">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-172">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="0184a-173">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="0184a-173">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="0184a-174">Chargement différé pour EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="0184a-174">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="0184a-175">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-175">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="0184a-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="0184a-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="0184a-177">Extensions EntityFrameworkCore pour les opérations en bloc (insertion, mise à jour, suppression).</span><span class="sxs-lookup"><span data-stu-id="0184a-177">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="0184a-178">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="0184a-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="0184a-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="0184a-180">Ajoute la pluralisation au moment du design sur EF Core.</span><span class="sxs-lookup"><span data-stu-id="0184a-180">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="0184a-181">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="0184a-181">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
