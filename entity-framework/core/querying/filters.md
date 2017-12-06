---
title: "Filtres de requête globale - EF Core"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="bdc3f-102">Filtres de requête globale</span><span class="sxs-lookup"><span data-stu-id="bdc3f-102">Global Query Filters</span></span>

<span data-ttu-id="bdc3f-103">Les filtres de requête globale sont des prédicats de requête LINQ (une expression booléenne est généralement passé dans le LINQ *où* opérateur de requête) appliqué aux Types d’entités dans le modèle de métadonnées (généralement dans *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="bdc3f-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="bdc3f-104">Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces Types d’entités, y compris les références de propriété de navigation référencée indirectement, par exemple via l’utilisation d’inclure ou direct de Types d’entités.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="bdc3f-105">Certaines applications courantes de cette fonctionnalité sont :</span><span class="sxs-lookup"><span data-stu-id="bdc3f-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="bdc3f-106">**La suppression réversible** -un Type d’entité définit une *IsDeleted* propriété.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="bdc3f-107">**Une architecture mutualisée** -un Type d’entité définit une *TenantId* propriété.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="bdc3f-108">Exemple</span><span class="sxs-lookup"><span data-stu-id="bdc3f-108">Example</span></span>

<span data-ttu-id="bdc3f-109">L’exemple suivant montre comment utiliser des filtres de requête globale pour implémenter des comportements de requête soft-suppression et d’une architecture mutualisée dans un modèle de création de blogs simple.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="bdc3f-110">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="bdc3f-111">Tout d’abord définir les entités :</span><span class="sxs-lookup"><span data-stu-id="bdc3f-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="bdc3f-112">Notez la déclaration d’un __tenantId_ champ sur la _Blog_ entité.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="bdc3f-113">Celui-ci doit servir à associer chaque instance de Blog à un client spécifique.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="bdc3f-114">Également défini est un _IsDeleted_ propriété sur le _Post_ type d’entité.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="bdc3f-115">Il est utilisé pour effectuer le suivi de l’un _Post_ instance a été « supprimé ».</span><span class="sxs-lookup"><span data-stu-id="bdc3f-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="bdc3f-116">Par exemple L’instance est marquée comme supprimée withouth supprimer physiquement les données sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="bdc3f-117">Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de la ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="bdc3f-118">Les expressions de prédicat passé à la _HasQueryFilter_ appels seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="bdc3f-119">Notez l’utilisation d’un champ de niveau instance DbContext : ```_tenantId``` permet de définir le client en cours.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="bdc3f-120">Les filtres au niveau du modèle utilise la valeur de l’instance de contexte correct.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="bdc3f-121">Par exemple L’instance qui exécute la requête.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="bdc3f-122">La désactivation des filtres</span><span class="sxs-lookup"><span data-stu-id="bdc3f-122">Disabling Filters</span></span>

<span data-ttu-id="bdc3f-123">Les filtres peuvent être désactivées pour les requêtes LINQ à l’aide de la ```IgnoreQueryFilters()``` opérateur.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="bdc3f-124">Limitations</span><span class="sxs-lookup"><span data-stu-id="bdc3f-124">Limitations</span></span>

<span data-ttu-id="bdc3f-125">Les filtres de requête globale présentent les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bdc3f-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="bdc3f-126">Les filtres ne peut pas contenir de références à des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="bdc3f-127">Filtres peuvent être définis uniquement pour la racine du Type d’entité d’une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="bdc3f-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
