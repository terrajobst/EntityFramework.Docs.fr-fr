---
title: 'Enregistrement des données associées : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417544"
---
# <a name="saving-related-data"></a><span data-ttu-id="cf56e-102">Enregistrement des données associées</span><span class="sxs-lookup"><span data-stu-id="cf56e-102">Saving Related Data</span></span>

<span data-ttu-id="cf56e-103">En plus des entités isolées, vous pouvez également utiliser les relations définies dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="cf56e-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="cf56e-104">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="cf56e-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="cf56e-105">Ajout d’un graphique de nouvelles entités</span><span class="sxs-lookup"><span data-stu-id="cf56e-105">Adding a graph of new entities</span></span>

<span data-ttu-id="cf56e-106">Si vous créez plusieurs nouvelles entités associées, l’ajout d’une d’elles au contexte provoquera l’ajout des autres.</span><span class="sxs-lookup"><span data-stu-id="cf56e-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="cf56e-107">Dans l’exemple suivant, le blog et trois billets associés sont tous insérés dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="cf56e-108">Les billets sont trouvés et ajoutés, car ils sont accessibles via la propriété de navigation `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="cf56e-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="cf56e-109">Utilisez la propriété EntityEntry.State pour définir l’état d’une seule entité.</span><span class="sxs-lookup"><span data-stu-id="cf56e-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="cf56e-110">Par exemple : `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="cf56e-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="cf56e-111">Ajout d’une entité associée</span><span class="sxs-lookup"><span data-stu-id="cf56e-111">Adding a related entity</span></span>

<span data-ttu-id="cf56e-112">Si vous référencez une nouvelle entité à partir de la propriété de navigation d’une entité qui est déjà suivie par le contexte, l’entité est découverte et insérée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="cf56e-113">Dans l’exemple suivant, l’entité `post` est insérée car elle est ajoutée à la propriété `Posts` de l’entité `blog` qui a été récupérée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="cf56e-114">Modification de relations</span><span class="sxs-lookup"><span data-stu-id="cf56e-114">Changing relationships</span></span>

<span data-ttu-id="cf56e-115">Si vous modifiez la propriété de navigation d’une entité, les modifications correspondantes porteront sur la colonne de clé étrangère dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="cf56e-116">Dans l’exemple suivant, l’entité `post` est mise à jour pour appartenir à la nouvelle entité `blog`, car sa propriété de navigation `Blog` est définie pour pointer vers `blog`.</span><span class="sxs-lookup"><span data-stu-id="cf56e-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="cf56e-117">Notez que `blog` est également inséré dans la base de données, car il s’agit d’une nouvelle entité qui est référencée par la propriété de navigation d’une entité déjà suivie par le contexte (`post`).</span><span class="sxs-lookup"><span data-stu-id="cf56e-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="cf56e-118">Suppression de relations</span><span class="sxs-lookup"><span data-stu-id="cf56e-118">Removing relationships</span></span>

<span data-ttu-id="cf56e-119">Vous pouvez supprimer une relation en définissant une navigation de référence sur `null`, ou en supprimant de l’entité associée à partir d’une navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="cf56e-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="cf56e-120">Supprimer une relation peut avoir des effets secondaires sur l’entité dépendante, en fonction du comportement de suppression en cascade configuré dans la relation.</span><span class="sxs-lookup"><span data-stu-id="cf56e-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="cf56e-121">Par défaut, pour les relations requises, un comportement de suppression en cascade est configuré et l’entité dépendante/enfant est supprimée de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="cf56e-122">La suppression en cascade n’est pas configurée pour les relations facultatives par défaut, mais la propriété de clé étrangère est définie avec la valeur null.</span><span class="sxs-lookup"><span data-stu-id="cf56e-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="cf56e-123">Consultez [Relations obligatoires et facultatives](../modeling/relationships.md#required-and-optional-relationships) pour en savoir plus sur la configuration de la nécessité des relations.</span><span class="sxs-lookup"><span data-stu-id="cf56e-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="cf56e-124">Consultez [Suppression en cascade](cascade-delete.md) pour plus d’informations sur la façon dont les comportements de suppression en cascade fonctionnent, dont ils peuvent être configurés explicitement ou encore être sélectionnés par convention.</span><span class="sxs-lookup"><span data-stu-id="cf56e-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="cf56e-125">Dans l’exemple suivant, une suppression en cascade est configurée sur la relation entre `Blog` et `Post`, donc l’entité `post` est supprimée de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf56e-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
