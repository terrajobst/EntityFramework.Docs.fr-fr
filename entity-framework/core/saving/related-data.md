---
title: Données - EF Core inhérentes à l’enregistrement
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a><span data-ttu-id="7149c-102">Enregistrer les données associées</span><span class="sxs-lookup"><span data-stu-id="7149c-102">Saving Related Data</span></span>

<span data-ttu-id="7149c-103">En plus des entités isolées, vous pouvez également faire utiliser des relations définies dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="7149c-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="7149c-104">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="7149c-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="7149c-105">Ajout d’un graphique de nouvelles entités</span><span class="sxs-lookup"><span data-stu-id="7149c-105">Adding a graph of new entities</span></span>

<span data-ttu-id="7149c-106">Si vous créez plusieurs nouvelles entités connexes, les Ajout d’un d’eux au contexte provoquer les autres à ajouter trop.</span><span class="sxs-lookup"><span data-stu-id="7149c-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="7149c-107">Dans l’exemple suivant, le blog et trois publications associées sont tous insérées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="7149c-108">Les publications sont trouvées et ajoutées, car ils sont accessibles via le `Blog.Posts` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="7149c-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="7149c-109">Utilisez la propriété EntityEntry.State pour définir l’état de simplement une seule entité.</span><span class="sxs-lookup"><span data-stu-id="7149c-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="7149c-110">Par exemple, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="7149c-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="7149c-111">Ajout d’une entité connexe</span><span class="sxs-lookup"><span data-stu-id="7149c-111">Adding a related entity</span></span>

<span data-ttu-id="7149c-112">Si vous référencez une nouvelle entité à partir de la propriété de navigation d’une entité est déjà suivie par le contexte, l’entité est découverts et insérée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="7149c-113">Dans l’exemple suivant, la `post` entité est insérée car elle est ajoutée à la `Posts` propriété de la `blog` entité qui a été lue à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="7149c-114">Modification des relations</span><span class="sxs-lookup"><span data-stu-id="7149c-114">Changing relationships</span></span>

<span data-ttu-id="7149c-115">Si vous modifiez la propriété de navigation d’une entité, les modifications correspondantes porteront sur la colonne de clé étrangère dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="7149c-116">Dans l’exemple suivant, la `post` mise à jour une entité pour appartenir au nouveau `blog` entité, car son `Blog` propriété de navigation est définie pour pointer vers `blog`.</span><span class="sxs-lookup"><span data-stu-id="7149c-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="7149c-117">Notez que `blog` est également inséré dans la base de données, car il s’agit d’une nouvelle entité qui est référencée par la propriété de navigation d’une entité est déjà suivie par le contexte (`post`).</span><span class="sxs-lookup"><span data-stu-id="7149c-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="7149c-118">Suppression de relations</span><span class="sxs-lookup"><span data-stu-id="7149c-118">Removing relationships</span></span>

<span data-ttu-id="7149c-119">Vous pouvez supprimer une relation en définissant une navigation de la référence `null`, ou la suppression de l’entité associée à partir d’une navigation dans la collection.</span><span class="sxs-lookup"><span data-stu-id="7149c-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="7149c-120">Suppression d’une relation, vous pouvez avoir des effets secondaires sur l’entité dépendante, en fonction de l’option cascade supprimer le comportement configuré dans la relation.</span><span class="sxs-lookup"><span data-stu-id="7149c-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="7149c-121">Par défaut, pour les relations requises, un comportement de suppression en cascade est configuré et l’entité enfant/dépendantes est supprimée de la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="7149c-122">Suppression en cascade n’est pas configurée pour les relations facultatif, par défaut, mais la propriété de clé étrangère être définie avec la valeur null.</span><span class="sxs-lookup"><span data-stu-id="7149c-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="7149c-123">Consultez [relations obligatoires et facultatifs](../modeling/relationships.md#required-and-optional-relationships) pour en savoir plus sur la configuration de la requiredness des relations.</span><span class="sxs-lookup"><span data-stu-id="7149c-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="7149c-124">Consultez [suppression en Cascade](cascade-delete.md) pour plus d’informations sur les comportements de delete cascade fonctionnent, comment ils peuvent être configurés explicitement et comment elles sont sélectionnées par convention.</span><span class="sxs-lookup"><span data-stu-id="7149c-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="7149c-125">Dans l’exemple suivant, une suppression en cascade est configurée sur la relation entre `Blog` et `Post`, donc le `post` est supprimée de la base de données.</span><span class="sxs-lookup"><span data-stu-id="7149c-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
