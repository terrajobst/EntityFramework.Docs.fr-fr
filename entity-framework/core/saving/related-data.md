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
# <a name="saving-related-data"></a>Enregistrer les données associées

En plus des entités isolées, vous pouvez également faire utiliser des relations définies dans votre modèle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) sur GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Ajout d’un graphique de nouvelles entités

Si vous créez plusieurs nouvelles entités connexes, les Ajout d’un d’eux au contexte provoquer les autres à ajouter trop.

Dans l’exemple suivant, le blog et trois publications associées sont tous insérées dans la base de données. Les publications sont trouvées et ajoutées, car ils sont accessibles via le `Blog.Posts` propriété de navigation.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Utilisez la propriété EntityEntry.State pour définir l’état de simplement une seule entité. Par exemple, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Ajout d’une entité connexe

Si vous référencez une nouvelle entité à partir de la propriété de navigation d’une entité est déjà suivie par le contexte, l’entité est découverts et insérée dans la base de données.

Dans l’exemple suivant, la `post` entité est insérée car elle est ajoutée à la `Posts` propriété de la `blog` entité qui a été lue à partir de la base de données.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Modification des relations

Si vous modifiez la propriété de navigation d’une entité, les modifications correspondantes porteront sur la colonne de clé étrangère dans la base de données.

Dans l’exemple suivant, la `post` mise à jour une entité pour appartenir au nouveau `blog` entité, car son `Blog` propriété de navigation est définie pour pointer vers `blog`. Notez que `blog` est également inséré dans la base de données, car il s’agit d’une nouvelle entité qui est référencée par la propriété de navigation d’une entité est déjà suivie par le contexte (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Suppression de relations

Vous pouvez supprimer une relation en définissant une navigation de la référence `null`, ou la suppression de l’entité associée à partir d’une navigation dans la collection.

Suppression d’une relation, vous pouvez avoir des effets secondaires sur l’entité dépendante, en fonction de l’option cascade supprimer le comportement configuré dans la relation.

Par défaut, pour les relations requises, un comportement de suppression en cascade est configuré et l’entité enfant/dépendantes est supprimée de la base de données. Suppression en cascade n’est pas configurée pour les relations facultatif, par défaut, mais la propriété de clé étrangère être définie avec la valeur null.

Consultez [relations obligatoires et facultatifs](../modeling/relationships.md#required-and-optional-relationships) pour en savoir plus sur la configuration de la requiredness des relations.

Consultez [suppression en Cascade](cascade-delete.md) pour plus d’informations sur les comportements de delete cascade fonctionnent, comment ils peuvent être configurés explicitement et comment elles sont sélectionnées par convention.

Dans l’exemple suivant, une suppression en cascade est configurée sur la relation entre `Blog` et `Post`, donc le `post` est supprimée de la base de données.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
