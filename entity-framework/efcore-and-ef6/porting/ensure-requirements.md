---
title: Portage des exigences de EF6 à EF Core-valider
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565342"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="4ded0-102">Avant le portage de EF6 vers EF Core: Valider les exigences de votre application</span><span class="sxs-lookup"><span data-stu-id="4ded0-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="4ded0-103">Avant de commencer le processus de Portage, il est important de vérifier que EF Core répond aux exigences d’accès aux données pour votre application.</span><span class="sxs-lookup"><span data-stu-id="4ded0-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="4ded0-104">Fonctionnalités manquantes</span><span class="sxs-lookup"><span data-stu-id="4ded0-104">Missing features</span></span>

<span data-ttu-id="4ded0-105">Assurez-vous que EF Core dispose de toutes les fonctionnalités que vous devez utiliser dans votre application.</span><span class="sxs-lookup"><span data-stu-id="4ded0-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="4ded0-106">Consultez [comparaison des fonctionnalités](../features.md) pour une comparaison détaillée de la façon dont la fonctionnalité définie dans EF Core est comparée à EF6.</span><span class="sxs-lookup"><span data-stu-id="4ded0-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="4ded0-107">Si des fonctionnalités requises sont manquantes, assurez-vous que vous pouvez compenser l’absence de ces fonctionnalités avant de procéder au portage vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="4ded0-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="4ded0-108">Changements de comportement</span><span class="sxs-lookup"><span data-stu-id="4ded0-108">Behavior changes</span></span>

<span data-ttu-id="4ded0-109">Il s’agit d’une liste non exhaustive de modifications de comportement entre EF6 et EF Core.</span><span class="sxs-lookup"><span data-stu-id="4ded0-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="4ded0-110">Il est important de garder à l’esprit que le port de votre application peut modifier le comportement de votre application, mais qu’elle n’apparaît pas en tant qu’erreurs de compilation après l’échange de EF Core.</span><span class="sxs-lookup"><span data-stu-id="4ded0-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="4ded0-111">DbSet. Add/Attach et le comportement du graphique</span><span class="sxs-lookup"><span data-stu-id="4ded0-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="4ded0-112">Dans EF6, l' `DbSet.Add()` appel de sur une entité entraîne une recherche récursive pour toutes les entités référencées dans ses propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="4ded0-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="4ded0-113">Toutes les entités qui sont détectées et qui ne sont pas déjà suivies par le contexte sont également marquées comme ajoutées.</span><span class="sxs-lookup"><span data-stu-id="4ded0-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="4ded0-114">`DbSet.Attach()`se comporte de la même manière, sauf que toutes les entités sont marquées comme inchangées.</span><span class="sxs-lookup"><span data-stu-id="4ded0-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="4ded0-115">**EF Core effectue une recherche récursive similaire, mais avec des règles légèrement différentes.**</span><span class="sxs-lookup"><span data-stu-id="4ded0-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="4ded0-116">L’entité racine est toujours dans l’État demandé (ajouté pour `DbSet.Add` et inchangé pour `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="4ded0-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="4ded0-117">**Pour les entités qui sont détectées lors de la recherche récursive des propriétés de navigation:**</span><span class="sxs-lookup"><span data-stu-id="4ded0-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="4ded0-118">**Si la clé primaire de l’entité est générée par le magasin**</span><span class="sxs-lookup"><span data-stu-id="4ded0-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="4ded0-119">Si la clé primaire n’est pas définie sur une valeur, l’État est défini sur ajouté.</span><span class="sxs-lookup"><span data-stu-id="4ded0-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="4ded0-120">La valeur de clé primaire est considérée comme «non définie» si elle est affectée à la valeur CLR par défaut pour le type de `0` propriété `int`(par `string`exemple, pour, `null` pour, etc.).</span><span class="sxs-lookup"><span data-stu-id="4ded0-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="4ded0-121">Si la clé primaire est définie sur une valeur, l’État est défini sur inchangé.</span><span class="sxs-lookup"><span data-stu-id="4ded0-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="4ded0-122">Si la clé primaire n’est pas générée par la base de données, l’entité est placée dans le même État que la racine.</span><span class="sxs-lookup"><span data-stu-id="4ded0-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="4ded0-123">Initialisation de la base de données Code First</span><span class="sxs-lookup"><span data-stu-id="4ded0-123">Code First database initialization</span></span>

<span data-ttu-id="4ded0-124">**EF6 dispose d’un grand nombre de Magic pour la sélection de la connexion de base de données et l’initialisation de la base de données. Parmi ces règles, citons les suivantes:**</span><span class="sxs-lookup"><span data-stu-id="4ded0-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="4ded0-125">Si aucune configuration n’est effectuée, EF6 sélectionne une base de données sur SQL Express ou sur une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="4ded0-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="4ded0-126">Si une chaîne de connexion portant le même nom que le contexte se trouve dans `App/Web.config` le fichier d’application, cette connexion est utilisée.</span><span class="sxs-lookup"><span data-stu-id="4ded0-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="4ded0-127">Si la base de données n’existe pas, elle est créée.</span><span class="sxs-lookup"><span data-stu-id="4ded0-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="4ded0-128">Si aucune des tables du modèle n’existe dans la base de données, le schéma du modèle actuel est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="4ded0-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="4ded0-129">Si les migrations sont activées, elles sont utilisées pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="4ded0-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="4ded0-130">Si la base de données existe et que EF6 a déjà créé le schéma, la compatibilité du schéma avec le modèle actuel est vérifiée.</span><span class="sxs-lookup"><span data-stu-id="4ded0-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="4ded0-131">Une exception est levée si le modèle a changé depuis la création du schéma.</span><span class="sxs-lookup"><span data-stu-id="4ded0-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="4ded0-132">**EF Core n’effectue pas l’une de ces magie.**</span><span class="sxs-lookup"><span data-stu-id="4ded0-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="4ded0-133">La connexion de base de données doit être configurée de manière explicite dans le code.</span><span class="sxs-lookup"><span data-stu-id="4ded0-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="4ded0-134">Aucune initialisation n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="4ded0-134">No initialization is performed.</span></span> <span data-ttu-id="4ded0-135">Vous devez utiliser `DbContext.Database.Migrate()` pour appliquer des migrations ( `DbContext.Database.EnsureCreated()` ou `EnsureDeleted()` et pour créer/supprimer la base de données sans utiliser les migrations).</span><span class="sxs-lookup"><span data-stu-id="4ded0-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="4ded0-136">Convention d’affectation de noms de table Code First</span><span class="sxs-lookup"><span data-stu-id="4ded0-136">Code First table naming convention</span></span>

<span data-ttu-id="4ded0-137">EF6 exécute le nom de la classe d’entité via un service de pluralisation pour calculer le nom de la table par défaut à laquelle l’entité est mappée.</span><span class="sxs-lookup"><span data-stu-id="4ded0-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="4ded0-138">EF Core utilise le nom de la `DbSet` propriété dans laquelle l’entité est exposée dans le contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="4ded0-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="4ded0-139">Si l’entité n’a pas de `DbSet` propriété, le nom de la classe est utilisé.</span><span class="sxs-lookup"><span data-stu-id="4ded0-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
