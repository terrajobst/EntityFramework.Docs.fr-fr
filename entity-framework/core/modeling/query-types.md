---
title: Types de requêtes - EF Core
author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 54d960e2e2236e2d4185dedc48f51035f5c10e93
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250723"
---
# <a name="query-types"></a><span data-ttu-id="65cd8-102">Types de requêtes</span><span class="sxs-lookup"><span data-stu-id="65cd8-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="65cd8-103">Cette fonctionnalité est une nouveauté dans EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="65cd8-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="65cd8-104">En plus des types d’entité, un modèle EF Core peut contenir _types de requête_, qui peut être utilisé pour effectuer des requêtes de base de données sur des données qui n’est pas mappées aux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="65cd8-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="65cd8-105">Comparer des types de requête aux types d’entité</span><span class="sxs-lookup"><span data-stu-id="65cd8-105">Compare query types to entity types</span></span>

<span data-ttu-id="65cd8-106">Types de requêtes sont comme les types d’entités qui elles :</span><span class="sxs-lookup"><span data-stu-id="65cd8-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="65cd8-107">Peuvent être ajoutées au modèle, soit dans `OnModelCreating` ou via une propriété « set » sur une dérivée _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="65cd8-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="65cd8-108">Prend en charge la plupart des mêmes fonctionnalités de mappage, comme les propriétés de navigation et de mappage de l’héritage.</span><span class="sxs-lookup"><span data-stu-id="65cd8-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="65cd8-109">Sur des magasins relationnels, ils peuvent configurer les objets de base de données cible et les colonnes par le biais de méthodes de l’API fluent ou des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="65cd8-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="65cd8-110">Toutefois, ils sont différents à partir de l’entité types dans ce qu’il :</span><span class="sxs-lookup"><span data-stu-id="65cd8-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="65cd8-111">Ne nécessitent pas une clé à définir.</span><span class="sxs-lookup"><span data-stu-id="65cd8-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="65cd8-112">Ne sont jamais suivis pour les modifications sur le _DbContext_ et par conséquent sont jamais insérées, mises à jour ou supprimées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="65cd8-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="65cd8-113">Ne sont jamais découverts par convention.</span><span class="sxs-lookup"><span data-stu-id="65cd8-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="65cd8-114">Prennent uniquement en charge un sous-ensemble des fonctionnalités de mappage de navigation - en particulier :</span><span class="sxs-lookup"><span data-stu-id="65cd8-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="65cd8-115">Ils ne peuvent jamais agir en tant que la terminaison principale d’une relation.</span><span class="sxs-lookup"><span data-stu-id="65cd8-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="65cd8-116">Ils peuvent contenir uniquement des propriétés de navigation de référence qui pointe vers des entités.</span><span class="sxs-lookup"><span data-stu-id="65cd8-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="65cd8-117">Entités ne peut pas contenir les propriétés de navigation aux types de requêtes.</span><span class="sxs-lookup"><span data-stu-id="65cd8-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="65cd8-118">Sont traitées sur le _ModelBuilder_ à l’aide de la `Query` méthode plutôt que la `Entity` (méthode).</span><span class="sxs-lookup"><span data-stu-id="65cd8-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="65cd8-119">Sont mappées sur le _DbContext_ via les propriétés de type `DbQuery<T>` au lieu de `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="65cd8-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="65cd8-120">Sont mappées aux objets de base de données en utilisant le `ToView` (méthode), plutôt que `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="65cd8-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="65cd8-121">Peut être mappé à un _définition de requête_ : une définition de requête est une requête secondaire déclarée dans le modèle qui fait Office de source de données pour un type de requête.</span><span class="sxs-lookup"><span data-stu-id="65cd8-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="65cd8-122">Scénarios d'utilisation</span><span class="sxs-lookup"><span data-stu-id="65cd8-122">Usage scenarios</span></span>

<span data-ttu-id="65cd8-123">Les principaux scénarios d’utilisation pour les types de requêtes sont notamment :</span><span class="sxs-lookup"><span data-stu-id="65cd8-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="65cd8-124">Agissant comme le type de retour pour ad hoc `FromSql()` requêtes.</span><span class="sxs-lookup"><span data-stu-id="65cd8-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="65cd8-125">Mappage avec les vues de base de données.</span><span class="sxs-lookup"><span data-stu-id="65cd8-125">Mapping to database views.</span></span>
- <span data-ttu-id="65cd8-126">Mappage de tables qui n’ont pas d’une clé primaire définie.</span><span class="sxs-lookup"><span data-stu-id="65cd8-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="65cd8-127">Mappage à des requêtes définies dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="65cd8-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="65cd8-128">Mappage aux objets de base de données</span><span class="sxs-lookup"><span data-stu-id="65cd8-128">Mapping to database objects</span></span>

<span data-ttu-id="65cd8-129">Mappage d’un type de requête à un objet de base de données est obtenue à l’aide de la `ToView` API fluent.</span><span class="sxs-lookup"><span data-stu-id="65cd8-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="65cd8-130">Du point de vue d’EF Core, l’objet de base de données spécifié dans cette méthode est un _vue_, ce qui signifie qu’il est traité comme une source de la requête en lecture seule et ne peut pas être la cible de mise à jour, insérer ou supprimer des opérations.</span><span class="sxs-lookup"><span data-stu-id="65cd8-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="65cd8-131">Toutefois, cela ne signifie pas que l’objet de base de données soit réellement nécessaire pour être une vue de base de données, il peut également être une table de base de données qui est traitée comme étant en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="65cd8-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="65cd8-132">À l’inverse, pour les types d’entité, EF Core suppose qu’un objet de base de données spécifiée dans le `ToTable` méthode peut être traitée comme un _table_, ce qui signifie qu’il peut être utilisé en tant que requête source, mais également ciblé par la mise à jour, supprimer et insérer opérations.</span><span class="sxs-lookup"><span data-stu-id="65cd8-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="65cd8-133">En fait, vous pouvez spécifier le nom d’une vue de base de données dans `ToTable` et tout devrait fonctionner correctement tant que la vue est configurée pour être mis à jour sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="65cd8-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="65cd8-134">Exemple</span><span class="sxs-lookup"><span data-stu-id="65cd8-134">Example</span></span>

<span data-ttu-id="65cd8-135">L’exemple suivant montre comment utiliser le Type de requête pour interroger une vue de base de données.</span><span class="sxs-lookup"><span data-stu-id="65cd8-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="65cd8-136">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="65cd8-136">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="65cd8-137">Tout d’abord, nous définissons un modèle simple de Blog et Post :</span><span class="sxs-lookup"><span data-stu-id="65cd8-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="65cd8-138">Ensuite, nous définissons une vue de base de données simple qui nous permettra d’interroger le nombre de messages associée à chaque blog :</span><span class="sxs-lookup"><span data-stu-id="65cd8-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="65cd8-139">Ensuite, nous définissons une classe pour contenir le résultat de la vue de base de données :</span><span class="sxs-lookup"><span data-stu-id="65cd8-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="65cd8-140">Ensuite, nous configurer le type de requête dans _OnModelCreating_ à l’aide de la `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="65cd8-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="65cd8-141">Nous utilisons des API de configuration fluent standard pour configurer le mappage pour le Type de requête :</span><span class="sxs-lookup"><span data-stu-id="65cd8-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="65cd8-142">Enfin, nous pouvons interroger la vue de base de données de manière standard :</span><span class="sxs-lookup"><span data-stu-id="65cd8-142">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="65cd8-143">Notez que nous avons également défini une propriété de niveau requête (DbQuery) d’agir en tant que racine pour les requêtes sur ce type de contexte.</span><span class="sxs-lookup"><span data-stu-id="65cd8-143">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
