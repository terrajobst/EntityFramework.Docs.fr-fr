---
title: 'Filtres de requête globale : EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 4afc9fb0338d34845639d57013ac710445321940
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562440"
---
# <a name="global-query-filters"></a><span data-ttu-id="97d16-102">Filtres de requête globale</span><span class="sxs-lookup"><span data-stu-id="97d16-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="97d16-103">Cette fonctionnalité date de la publication d’EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="97d16-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="97d16-104">Les filtres de requête globale permet aux prédicats de requête LINQ (expression booléenne généralement passée à l’opérateur de requête LINQ *Where*) d’être appliqués sur des types d’entité dans le modèle de métadonnées (généralement dans *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="97d16-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="97d16-105">Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces types d’entités, y compris ceux référencés indirectement, par exemple à l’aide d’Include ou de références de propriété de navigation directe.</span><span class="sxs-lookup"><span data-stu-id="97d16-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="97d16-106">Voici deux applications courantes de cette fonctionnalité :</span><span class="sxs-lookup"><span data-stu-id="97d16-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="97d16-107">**Suppression réversible** : un type d’entité définit une propriété *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="97d16-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="97d16-108">**Architecture multilocataire** : un type d’entité définit une propriété *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="97d16-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="97d16-109">Exemple</span><span class="sxs-lookup"><span data-stu-id="97d16-109">Example</span></span>

<span data-ttu-id="97d16-110">L’exemple suivant montre comment utiliser les filtres de requête globale pour implémenter des comportements de suppression réversible et d’architecture multilocataire dans un modèle de création de blogs simple.</span><span class="sxs-lookup"><span data-stu-id="97d16-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="97d16-111">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="97d16-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="97d16-112">Tout d’abord définissez les entités :</span><span class="sxs-lookup"><span data-stu-id="97d16-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="97d16-113">Notez la déclaration d’un champ __tenantId_ sur l’entité _Blog_.</span><span class="sxs-lookup"><span data-stu-id="97d16-113">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="97d16-114">Celui-ci doit servir à associer chaque instance de blog à un client spécifique.</span><span class="sxs-lookup"><span data-stu-id="97d16-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="97d16-115">Une propriété _IsDeleted_ est également définie sur le type d’entité _Post_.</span><span class="sxs-lookup"><span data-stu-id="97d16-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="97d16-116">Elle est utilisée pour déterminer si une instance _Post_ a été « supprimée de façon réversible ».</span><span class="sxs-lookup"><span data-stu-id="97d16-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="97d16-117">Autrement dit, l’instance est marquée comme supprimée sans que les données sous-jacentes soient réellement supprimées.</span><span class="sxs-lookup"><span data-stu-id="97d16-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="97d16-118">Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de l’API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="97d16-118">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="97d16-119">Les expressions de prédicat passées aux appels _HasQueryFilter_ seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.</span><span class="sxs-lookup"><span data-stu-id="97d16-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="97d16-120">Notez l’utilisation d’un champ de niveau d’instance DbContext : ```_tenantId``` permet de définir le client en cours.</span><span class="sxs-lookup"><span data-stu-id="97d16-120">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="97d16-121">Les filtres au niveau du modèle utilisent la valeur de l’instance de contexte correcte (c’est-à-dire celle qui exécute la requête).</span><span class="sxs-lookup"><span data-stu-id="97d16-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="97d16-122">Désactivation des filtres</span><span class="sxs-lookup"><span data-stu-id="97d16-122">Disabling Filters</span></span>

<span data-ttu-id="97d16-123">Les filtres peuvent être désactivés pour des requêtes LINQ individuelles à l’aide de l’opérateur ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="97d16-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="97d16-124">Limitations</span><span class="sxs-lookup"><span data-stu-id="97d16-124">Limitations</span></span>

<span data-ttu-id="97d16-125">Les filtres de requête globale présentent les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="97d16-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="97d16-126">Les filtres ne peuvent pas contenir de références à des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="97d16-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="97d16-127">Les filtres ne peuvent être définis que pour le type d’entité racine d’une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="97d16-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
