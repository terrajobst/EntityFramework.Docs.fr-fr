---
title: 'Filtres de requête globale : EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417728"
---
# <a name="global-query-filters"></a><span data-ttu-id="bc4ec-102">Filtres de requête globale</span><span class="sxs-lookup"><span data-stu-id="bc4ec-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="bc4ec-103">Cette fonctionnalité date de la publication d’EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="bc4ec-104">Les filtres de requête globale permet aux prédicats de requête LINQ (expression booléenne généralement passée à l’opérateur de requête LINQ *Where*) d’être appliqués sur des types d’entité dans le modèle de métadonnées (généralement dans *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="bc4ec-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="bc4ec-105">Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces types d’entités, y compris ceux référencés indirectement, par exemple à l’aide d’Include ou de références de propriété de navigation directe.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="bc4ec-106">Voici deux applications courantes de cette fonctionnalité :</span><span class="sxs-lookup"><span data-stu-id="bc4ec-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="bc4ec-107">**Suppression réversible** : un type d’entité définit une propriété *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="bc4ec-108">**Architecture multilocataire** : un type d’entité définit une propriété *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="bc4ec-109">Exemple</span><span class="sxs-lookup"><span data-stu-id="bc4ec-109">Example</span></span>

<span data-ttu-id="bc4ec-110">L’exemple suivant montre comment utiliser les filtres de requête globale pour implémenter des comportements de suppression réversible et d’architecture multilocataire dans un modèle de création de blogs simple.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="bc4ec-111">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="bc4ec-112">Tout d’abord définissez les entités :</span><span class="sxs-lookup"><span data-stu-id="bc4ec-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="bc4ec-113">Notez la déclaration d’un champ _tenantId_ sur l’entité _Blog_.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="bc4ec-114">Celui-ci doit servir à associer chaque instance de blog à un client spécifique.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="bc4ec-115">Une propriété _IsDeleted_ est également définie sur le type d’entité _Post_.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="bc4ec-116">Elle est utilisée pour déterminer si une instance _Post_ a été « supprimée de façon réversible ».</span><span class="sxs-lookup"><span data-stu-id="bc4ec-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="bc4ec-117">Autrement dit, l’instance est marquée comme supprimée sans que les données sous-jacentes soient réellement supprimées.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="bc4ec-118">Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de l’API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="bc4ec-119">Les expressions de prédicat passées aux appels _HasQueryFilter_ seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="bc4ec-120">Notez l’utilisation d’un champ de niveau d’instance DbContext : `_tenantId` permet de définir le client en cours.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="bc4ec-121">Les filtres au niveau du modèle utilisent la valeur de l’instance de contexte correcte (c’est-à-dire celle qui exécute la requête).</span><span class="sxs-lookup"><span data-stu-id="bc4ec-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="bc4ec-122">Il n’est actuellement pas possible de définir plusieurs filtres de requête sur la même entité ; seul le dernier sera appliqué.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="bc4ec-123">Toutefois, vous pouvez définir un filtre unique avec plusieurs conditions à l’aide de l’opérateur _and_ logique ([`&&` dans C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="bc4ec-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="bc4ec-124">Désactivation des filtres</span><span class="sxs-lookup"><span data-stu-id="bc4ec-124">Disabling Filters</span></span>

<span data-ttu-id="bc4ec-125">Les filtres peuvent être désactivés pour des requêtes LINQ individuelles à l’aide de l’opérateur `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="bc4ec-126">Limites</span><span class="sxs-lookup"><span data-stu-id="bc4ec-126">Limitations</span></span>

<span data-ttu-id="bc4ec-127">Les filtres de requête globale présentent les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc4ec-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="bc4ec-128">Les filtres ne peuvent être définis que pour le type d’entité racine d’une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="bc4ec-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
