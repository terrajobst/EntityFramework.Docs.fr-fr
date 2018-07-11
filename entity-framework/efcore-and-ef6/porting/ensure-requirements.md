---
title: Portage depuis EF6 vers EF Core - valider les exigences
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949112"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="0fc95-102">Avant le portage depuis EF6 vers EF Core : valider les exigences de votre Application</span><span class="sxs-lookup"><span data-stu-id="0fc95-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="0fc95-103">Avant de commencer le processus de portage, il est important de valider qu’EF Core répond à l’accès aux données pour votre application.</span><span class="sxs-lookup"><span data-stu-id="0fc95-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="0fc95-104">Fonctionnalités manquantes</span><span class="sxs-lookup"><span data-stu-id="0fc95-104">Missing features</span></span>

<span data-ttu-id="0fc95-105">Assurez-vous que EF Core possède toutes les fonctionnalités que vous devez utiliser dans votre application.</span><span class="sxs-lookup"><span data-stu-id="0fc95-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="0fc95-106">Consultez [comparaison des fonctionnalités](../features.md) pour une comparaison détaillée de la fonctionnalité définie dans EF Core comparatif entre EF6.</span><span class="sxs-lookup"><span data-stu-id="0fc95-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="0fc95-107">Si toutes les fonctionnalités requises sont manquantes, vérifiez que vous pouvez pallier l’absence de ces fonctionnalités avant de porter vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc95-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="0fc95-108">Changements de comportement</span><span class="sxs-lookup"><span data-stu-id="0fc95-108">Behavior changes</span></span>

<span data-ttu-id="0fc95-109">Il s’agit d’une liste non exhaustive de certaines modifications de comportement entre EF6 et EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc95-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="0fc95-110">Il est important de garder ces pratiques comme votre port de votre application car elles peuvent changer la façon de votre application se comporte, mais n’apparaît pas comme des erreurs de compilation après le remplacement à EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fc95-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="0fc95-111">Comportement DbSet.Add/Attach et graph</span><span class="sxs-lookup"><span data-stu-id="0fc95-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="0fc95-112">Dans EF6, l’appel `DbSet.Add()` sur les résultats de l’entité dans une recherche récursive pour toutes les entités référencées dans ses propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="0fc95-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="0fc95-113">Toutes les entités qui sont trouvent et ne sont pas suivies par le contexte, sont également être marqué comme ajouté.</span><span class="sxs-lookup"><span data-stu-id="0fc95-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="0fc95-114">`DbSet.Attach()` se comporte de la même, mais toutes les entités sont marquées comme inchangé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="0fc95-115">**EF Core effectue une recherche récursive similaire, mais avec certaines des règles légèrement différentes.**</span><span class="sxs-lookup"><span data-stu-id="0fc95-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="0fc95-116">L’entité racine est toujours dans l’état requis (ajoutée pour `DbSet.Add` et inchangée pour `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="0fc95-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="0fc95-117">**Pour les entités qui se trouvent au cours de la recherche récursive des propriétés de navigation :**</span><span class="sxs-lookup"><span data-stu-id="0fc95-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="0fc95-118">**Si la clé primaire de l’entité est généré de magasin**</span><span class="sxs-lookup"><span data-stu-id="0fc95-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="0fc95-119">Si la clé primaire n’est pas définie sur une valeur, l’état est défini à ajouté.</span><span class="sxs-lookup"><span data-stu-id="0fc95-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="0fc95-120">La valeur de clé primaire est considérée comme « non définie » si elle est affectée à la valeur par défaut CLR pour le type de propriété (par exemple, `0` pour `int`, `null` pour `string`, etc..).</span><span class="sxs-lookup"><span data-stu-id="0fc95-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="0fc95-121">Si la clé primaire est définie sur une valeur, l’état a inchangé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="0fc95-122">Si la clé primaire n’est pas généré de base de données, l’entité est placée dans le même état que la racine.</span><span class="sxs-lookup"><span data-stu-id="0fc95-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="0fc95-123">Première initialisation de base de données de code</span><span class="sxs-lookup"><span data-stu-id="0fc95-123">Code First database initialization</span></span>

<span data-ttu-id="0fc95-124">**EF6 a beaucoup de magie qu’il effectue autour de la sélection de la connexion de base de données et de l’initialisation de la base de données. Certaines de ces règles sont les suivantes :**</span><span class="sxs-lookup"><span data-stu-id="0fc95-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="0fc95-125">Si aucune configuration n’est effectuée, EF6 sélectionne une base de données sur SQL Express ou LocalDb.</span><span class="sxs-lookup"><span data-stu-id="0fc95-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="0fc95-126">Si une chaîne de connexion avec le même nom que le contexte est dans les applications `App/Web.config` fichier, cette connexion est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0fc95-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="0fc95-127">Si la base de données n’existe pas, il est créé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="0fc95-128">Si aucune des tables à partir du modèle existent dans la base de données, le schéma pour le modèle actuel est ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="0fc95-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="0fc95-129">Si les migrations sont activées, ils sont utilisés pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="0fc95-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="0fc95-130">Si la base de données existe et EF6 avait précédemment créé le schéma, le schéma est vérifié pour assurer la compatibilité avec le modèle actuel.</span><span class="sxs-lookup"><span data-stu-id="0fc95-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="0fc95-131">Une exception est levée si le modèle a changé depuis que le schéma a été créé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="0fc95-132">**EF Core n’effectue aucun de cette commande magique.**</span><span class="sxs-lookup"><span data-stu-id="0fc95-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="0fc95-133">La connexion de base de données doit être configurée de manière explicite dans le code.</span><span class="sxs-lookup"><span data-stu-id="0fc95-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="0fc95-134">Aucune initialisation n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="0fc95-134">No initialization is performed.</span></span> <span data-ttu-id="0fc95-135">Vous devez utiliser `DbContext.Database.Migrate()` pour appliquer des migrations (ou `DbContext.Database.EnsureCreated()` et `EnsureDeleted()` pour créer ou supprimer de la base de données sans utiliser de migrations).</span><span class="sxs-lookup"><span data-stu-id="0fc95-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="0fc95-136">Code tout d’abord une convention d’affectation de noms de table</span><span class="sxs-lookup"><span data-stu-id="0fc95-136">Code First table naming convention</span></span>

<span data-ttu-id="0fc95-137">EF6 s’exécute le nom de classe d’entité via un service de pluralisation pour calculer le nom de table par défaut l’entité est mappée à.</span><span class="sxs-lookup"><span data-stu-id="0fc95-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="0fc95-138">EF Core utilise le nom de la `DbSet` propriété l’entité sur laquelle est exposée dans le contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="0fc95-139">Si l’entité n’a pas un `DbSet` propriété, le nom de classe est utilisé.</span><span class="sxs-lookup"><span data-stu-id="0fc95-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
