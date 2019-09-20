---
title: Types d’entité sans clé-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: b968ac9602b9aa1f1c1e3181b6b76a64394d70f0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150842"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="91330-102">Types d’entité sans clé</span><span class="sxs-lookup"><span data-stu-id="91330-102">Keyless Entity Types</span></span>
> [!NOTE]
> <span data-ttu-id="91330-103">Cette fonctionnalité est une nouveauté d’EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="91330-103">This feature is new in EF Core 2.1.</span></span> <span data-ttu-id="91330-104">Avant le 3,0, ils étaient appelés types de requêtes</span><span class="sxs-lookup"><span data-stu-id="91330-104">Prior to 3.0 they were known as Query Types</span></span>

<span data-ttu-id="91330-105">En plus des types d’entités standard, un modèle de EF Core peut contenir des _types d’entité sans_clé, qui peuvent être utilisés pour exécuter des requêtes de base de données sur des données qui ne contiennent pas de valeurs de clés.</span><span class="sxs-lookup"><span data-stu-id="91330-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="91330-106">Caractéristiques des types d’entité sans clé</span><span class="sxs-lookup"><span data-stu-id="91330-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="91330-107">Les types d’entité sans clé prennent en charge un grand nombre des mêmes fonctionnalités de mappage que les types d’entités standard, tels que le mappage d’héritage et les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="91330-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="91330-108">Sur des magasins relationnels, ils peuvent configurer les objets de base de données cible et les colonnes par le biais de méthodes de l’API fluent ou des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="91330-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="91330-109">Toutefois, ils diffèrent des types d’entités standard en ce qu’ils :</span><span class="sxs-lookup"><span data-stu-id="91330-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="91330-110">Impossible d’avoir une clé définie.</span><span class="sxs-lookup"><span data-stu-id="91330-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="91330-111">Ne sont jamais suivis pour les modifications apportées à _DbContext_ et, par conséquent, ne sont jamais insérées, mises à jour ou supprimées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="91330-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="91330-112">Ne sont jamais découverts par convention.</span><span class="sxs-lookup"><span data-stu-id="91330-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="91330-113">Ne prennent en charge qu’un sous-ensemble de fonctionnalités de mappage de navigation, notamment :</span><span class="sxs-lookup"><span data-stu-id="91330-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="91330-114">Ils ne peuvent jamais agir en tant que la terminaison principale d’une relation.</span><span class="sxs-lookup"><span data-stu-id="91330-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="91330-115">Ils n’ont peut-être pas de navigation vers les entités détenues</span><span class="sxs-lookup"><span data-stu-id="91330-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="91330-116">Ils peuvent uniquement contenir des propriétés de navigation de référence pointant vers des entités normales.</span><span class="sxs-lookup"><span data-stu-id="91330-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="91330-117">Les entités ne peuvent pas contenir de propriétés de navigation pour les types d’entités Keyless.</span><span class="sxs-lookup"><span data-stu-id="91330-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="91330-118">Doit être configuré avec l' `.HasNoKey()` appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="91330-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="91330-119">Peut être mappé à une _requête de définition_.</span><span class="sxs-lookup"><span data-stu-id="91330-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="91330-120">Une requête de définition est une requête déclarée dans le modèle qui joue le rôle d’une source de données pour un type d’entité sans clé.</span><span class="sxs-lookup"><span data-stu-id="91330-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="91330-121">Scénarios d'utilisation</span><span class="sxs-lookup"><span data-stu-id="91330-121">Usage scenarios</span></span>

<span data-ttu-id="91330-122">Voici quelques-uns des principaux scénarios d’utilisation pour les types d’entité sans clé :</span><span class="sxs-lookup"><span data-stu-id="91330-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="91330-123">Servir de type de retour pour les [requêtes SQL brutes](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="91330-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="91330-124">Mappage à des vues de base de données qui ne contiennent pas de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="91330-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="91330-125">Mappage de tables qui n’ont pas d’une clé primaire définie.</span><span class="sxs-lookup"><span data-stu-id="91330-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="91330-126">Mappage à des requêtes définies dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="91330-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="91330-127">Mappage aux objets de base de données</span><span class="sxs-lookup"><span data-stu-id="91330-127">Mapping to database objects</span></span>

<span data-ttu-id="91330-128">Le mappage d’un type d’entité clé-inférieur à un objet de `ToTable` base `ToView` de données s’effectue à l’aide de l’API Fluent ou.</span><span class="sxs-lookup"><span data-stu-id="91330-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="91330-129">Du point de vue d’EF Core, l’objet de base de données spécifié dans cette méthode est un _vue_, ce qui signifie qu’il est traité comme une source de la requête en lecture seule et ne peut pas être la cible de mise à jour, insérer ou supprimer des opérations.</span><span class="sxs-lookup"><span data-stu-id="91330-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="91330-130">Toutefois, cela ne signifie pas que l’objet de base de données est réellement requis pour être une vue de base de données.</span><span class="sxs-lookup"><span data-stu-id="91330-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="91330-131">Il peut également s’agir d’une table de base de données qui sera traitée en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="91330-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="91330-132">À l’inverse, pour les types d’entités standard, EF Core suppose qu’un objet de base de `ToTable` données spécifié dans la méthode peut être traité comme une _table_, ce qui signifie qu’il peut être utilisé comme source de requête, mais également ciblé par les opérations Update, DELETE et insert.</span><span class="sxs-lookup"><span data-stu-id="91330-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="91330-133">En fait, vous pouvez spécifier le nom d’une vue de base de données dans `ToTable` et tout devrait fonctionner correctement tant que la vue est configurée pour être mis à jour sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="91330-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="91330-134">`ToView`suppose que l’objet existe déjà dans la base de données et qu’il n’est pas créé par des migrations.</span><span class="sxs-lookup"><span data-stu-id="91330-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="91330-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="91330-135">Example</span></span>

<span data-ttu-id="91330-136">L’exemple suivant montre comment utiliser les types d’entités Keyless pour interroger une vue de base de données.</span><span class="sxs-lookup"><span data-stu-id="91330-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="91330-137">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="91330-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="91330-138">Tout d’abord, nous définissons un modèle simple de Blog et Post :</span><span class="sxs-lookup"><span data-stu-id="91330-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="91330-139">Ensuite, nous définissons une vue de base de données simple qui nous permettra d’interroger le nombre de messages associée à chaque blog :</span><span class="sxs-lookup"><span data-stu-id="91330-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="91330-140">Ensuite, nous définissons une classe pour contenir le résultat de la vue de base de données :</span><span class="sxs-lookup"><span data-stu-id="91330-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="91330-141">Ensuite, vous configurez le type d’entité « sans clé `HasNoKey` » dans OnModelCreating à l’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="91330-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="91330-142">Nous utilisons l’API de configuration Fluent pour configurer le mappage pour le type d’entité « sans clé » :</span><span class="sxs-lookup"><span data-stu-id="91330-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="91330-143">Ensuite, nous configurons `DbContext` le pour inclure `DbSet<T>`les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="91330-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="91330-144">Enfin, nous pouvons interroger la vue de base de données de manière standard :</span><span class="sxs-lookup"><span data-stu-id="91330-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="91330-145">Notez que nous avons également défini une propriété de requête au niveau du contexte (DbSet) pour agir en tant que racine pour les requêtes sur ce type.</span><span class="sxs-lookup"><span data-stu-id="91330-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
