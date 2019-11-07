---
title: Outils et extensions - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654804"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ad074-102">Outils et extensions EF Core</span><span class="sxs-lookup"><span data-stu-id="ad074-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ad074-103">Ces outils et extensions fournissent des fonctionnalités supplémentaires pour Entity Framework Core 2.0 et ultérieur.</span><span class="sxs-lookup"><span data-stu-id="ad074-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ad074-104">Les extensions sont générées par des sources diverses et ne sont pas gérées dans le cadre du projet Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ad074-105">Quand vous envisagez une extension tierce, veillez à évaluer ses caractéristiques, notamment en termes de qualité, de gestion des licences, de compatibilité et de prise en charge, pour vérifier qu’elle répond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="ad074-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="ad074-106">Outils</span><span class="sxs-lookup"><span data-stu-id="ad074-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ad074-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ad074-107">LLBLGen Pro</span></span>

<span data-ttu-id="ad074-108">LLBLGen Pro est une solution de modélisation d’entités qui prend en charge Entity Framework et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ad074-109">Il vous permet de définir facilement votre modèle d’entités et de le mapper à votre base de données, en utilisant Database First ou Model First : vous pouvez donc commencer à écrire des requêtes immédiatement.</span><span class="sxs-lookup"><span data-stu-id="ad074-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="ad074-110">Site web</span><span class="sxs-lookup"><span data-stu-id="ad074-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ad074-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="ad074-111">Devart Entity Developer</span></span>

<span data-ttu-id="ad074-112">Entity Developer est un concepteur ORM pour ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access et LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="ad074-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ad074-113">Il prend en charge la conception visuelle de modèles EF Core, selon une approche Model First ou Database First, et la génération de code C# ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ad074-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span>

[<span data-ttu-id="ad074-114">Site web</span><span class="sxs-lookup"><span data-stu-id="ad074-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ad074-115">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="ad074-115">EF Core Power Tools</span></span>

<span data-ttu-id="ad074-116">EF Core Power Tools est une extension Visual Studio 2017 qui expose différentes tâches EF Core au moment du design dans une interface utilisateur simple.</span><span class="sxs-lookup"><span data-stu-id="ad074-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ad074-117">Elle inclut l’ingénierie à rebours des classes DbContext et d’entité à partir de bases de données existantes et des [packages DAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), la gestion des migrations de base de données et les visualisations de modèles.</span><span class="sxs-lookup"><span data-stu-id="ad074-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="ad074-118">Wiki GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ad074-119">Entity Framework Visual Editor</span><span class="sxs-lookup"><span data-stu-id="ad074-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ad074-120">Entity Framework Visual Editor est une extension de Visual Studio qui ajoute un concepteur ORM permettant de concevoir visuellement des classes EF 6 et EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ad074-121">Le code étant généré à l’aide de modèles T4, il peut être personnalisé pour répondre à tous les besoins.</span><span class="sxs-lookup"><span data-stu-id="ad074-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ad074-122">Il prend en charge les associations d’héritage, unidirectionnelles et bidirectionnelles, les énumérations ainsi que la possibilité de colorer le code de vos classes et d’ajouter des blocs de texte pour expliquer les parties potentiellement obscures de votre conception.</span><span class="sxs-lookup"><span data-stu-id="ad074-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="ad074-123">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ad074-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ad074-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ad074-124">CatFactory</span></span>

<span data-ttu-id="ad074-125">CatFactory est un moteur de génération de modèles automatique pour .NET Core qui peut automatiser la génération de classes DbContext, d’entités, de configurations de mappage et de classes de dépôts à partir d’une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ad074-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="ad074-126">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ad074-127">Générateur Entity Framework Core de LoreSoft</span><span class="sxs-lookup"><span data-stu-id="ad074-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ad074-128">Entity Framework Core Generator (efg) est un outil CLI .NET Core qui peut générer des modèles EF Core à partir d’une base de données existante, comme `dotnet ef dbcontext scaffold`, mais qui prend également en charge la [regénération](https://efg.loresoft.com/en/latest/regeneration/) de code safe à travers le remplacement de région ou l’analyse des fichiers de mappage.</span><span class="sxs-lookup"><span data-stu-id="ad074-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ad074-129">Cet outil prend en charge la génération de code de mappeur d’objet, de validation et de modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="ad074-129">This tool supports generating view models, validation, and object mapper code.</span></span>

<span data-ttu-id="ad074-130">[Tutoriel](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ad074-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ad074-131">Extensions</span><span class="sxs-lookup"><span data-stu-id="ad074-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ad074-132">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ad074-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ad074-133">Bibliothèque de plug-ins qui permet l’enregistrement automatique des changements de données effectués par EF Core dans une table d’historique.</span><span class="sxs-lookup"><span data-stu-id="ad074-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="ad074-134">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="ad074-135">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="ad074-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="ad074-136">Port .NET Core/.NET Standard de System.Linq.Dynamic qui inclut la prise en charge asynchrone avec EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="ad074-137">System.Linq.Dynamic provient d’un exemple Microsoft qui montre comment construire dynamiquement des requêtes LINQ à partir d’expressions de chaîne plutôt qu’à partir de code.</span><span class="sxs-lookup"><span data-stu-id="ad074-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="ad074-138">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ad074-139">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ad074-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ad074-140">Extension qui permet de stocker les résultats de requêtes EF Core dans un cache de second niveau afin que les exécutions ultérieures des mêmes requêtes puissent récupérer les données directement à partir du cache sans avoir à accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="ad074-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="ad074-141">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="ad074-142">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="ad074-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="ad074-143">Cette bibliothèque permet de récupérer les valeurs de la clé primaire (y compris les clés composites) à partir de n’importe quelle entité en tant que dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="ad074-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="ad074-144">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="ad074-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="ad074-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="ad074-146">Cette bibliothèque permet un accès fortement typé aux valeurs d’origine des propriétés d’entité.</span><span class="sxs-lookup"><span data-stu-id="ad074-146">This library enables strongly typed access to the original values of entity properties.</span></span>

[<span data-ttu-id="ad074-147">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="ad074-148">Geco</span><span class="sxs-lookup"><span data-stu-id="ad074-148">Geco</span></span>

<span data-ttu-id="ad074-149">Geco (Generator Console) est un générateur de code simple basé sur un projet de console qui s’exécute sur .NET Core et qui utilise des chaînes interpolées C# pour la génération de code.</span><span class="sxs-lookup"><span data-stu-id="ad074-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ad074-150">Geco inclut un générateur de modèle inverse pour EF Core avec prise en charge de la pluralisation, de la singularisation et des modèles modifiables.</span><span class="sxs-lookup"><span data-stu-id="ad074-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ad074-151">Il fournit également un générateur de script de données de départ, un exécuteur de scripts et un nettoyeur de base de données.</span><span class="sxs-lookup"><span data-stu-id="ad074-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="ad074-152">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="ad074-153">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ad074-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="ad074-154">LinqKit.Microsoft.EntityFrameworkCore est une version compatible avec EF Core de la bibliothèque LINQKit.</span><span class="sxs-lookup"><span data-stu-id="ad074-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="ad074-155">LINQKit est un ensemble gratuit d’extensions à l’attention des utilisateurs avancés LINQ to SQL et Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ad074-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="ad074-156">Il prend en charge des fonctionnalités avancées telles que la génération d’expressions de prédicat et l’utilisation de variables d’expression dans les sous-requêtes.</span><span class="sxs-lookup"><span data-stu-id="ad074-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="ad074-157">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ad074-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ad074-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="ad074-159">NeinLinq étend les fonctionnalités des fournisseurs LINQ comme Entity Framework pour permettre la réutilisation de fonctions, la réécriture des requêtes et la génération de requêtes dynamiques à l’aide de sélecteurs et de prédicats traduisibles.</span><span class="sxs-lookup"><span data-stu-id="ad074-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="ad074-160">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ad074-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ad074-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ad074-162">Plug-in pour Microsoft.EntityFrameworkCore prenant en charge un dépôt, les modèles d’unités de travail et les bases de données multiples avec des transactions distribuées.</span><span class="sxs-lookup"><span data-stu-id="ad074-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="ad074-163">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ad074-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ad074-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ad074-165">Extensions EF Core pour les opérations en bloc (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="ad074-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="ad074-166">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ad074-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ad074-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ad074-168">Ajoute la pluralisation au moment du design sur EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="ad074-169">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="ad074-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span><span class="sxs-lookup"><span data-stu-id="ad074-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="ad074-171">Méthode d’extension simple qui obtient l’instruction SQL générée par EF Core pour une requête LINQ donnée dans des scénarios simples.</span><span class="sxs-lookup"><span data-stu-id="ad074-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="ad074-172">La méthode ToSql est limitée aux scénarios simples, car EF Core peut générer plusieurs instructions SQL pour une seule requête LINQ et différentes instructions SQL en fonction des valeurs de paramètre.</span><span class="sxs-lookup"><span data-stu-id="ad074-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="ad074-173">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ad074-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ad074-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ad074-175">Reprise de l’attribut [Index] pour EF Core (avec l’extension pour la génération de modèles).</span><span class="sxs-lookup"><span data-stu-id="ad074-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="ad074-176">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ad074-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ad074-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ad074-178">Fournit un wrapper autour du fournisseur de base de données In-Memory EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ad074-179">Il fonctionne alors plus comme un fournisseur relationnel.</span><span class="sxs-lookup"><span data-stu-id="ad074-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="ad074-180">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ad074-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ad074-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ad074-182">Implémentation de la prise en charge temporelle pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="ad074-183">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ad074-184">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="ad074-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ad074-185">Cache des requêtes de second niveau hautes performances pour EF Core.</span><span class="sxs-lookup"><span data-stu-id="ad074-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="ad074-186">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ad074-187">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="ad074-187">Entity Framework Plus</span></span>

<span data-ttu-id="ad074-188">Étend votre DbContext avec des fonctionnalités telles que : Include Filter (Inclure le filtre), Auditing (Audit), Caching (Mise en cache), Query Future (Requête ultérieure), Batch Delete (Suppression par lot), Batch Update (Mise à jour par lot), et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="ad074-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="ad074-189">[Site web](https://entityframework-plus.net/)
[Référentiel GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ad074-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ad074-190">Extensions d’Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ad074-190">Entity Framework Extensions</span></span>

<span data-ttu-id="ad074-191">Étend votre DbContext avec des opérations en bloc hautes performances : BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="ad074-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="ad074-192">Site web</span><span class="sxs-lookup"><span data-stu-id="ad074-192">Website</span></span>](https://entityframework-extensions.net/)

### <a name="reconciler"></a><span data-ttu-id="ad074-193">Reconciler</span><span class="sxs-lookup"><span data-stu-id="ad074-193">Reconciler</span></span>

<span data-ttu-id="ad074-194">Met à jour un graphe d’entité du magasin avec un graphe donné en insérant, mettant à jour et supprimant les entités respectives.</span><span class="sxs-lookup"><span data-stu-id="ad074-194">Update an entity graph in store to a given one by inserting, updating and removing the respective entities.</span></span>

[<span data-ttu-id="ad074-195">Dépôt GitHub</span><span class="sxs-lookup"><span data-stu-id="ad074-195">GitHub repository</span></span>](https://github.com/jtheisen/reconciler)
