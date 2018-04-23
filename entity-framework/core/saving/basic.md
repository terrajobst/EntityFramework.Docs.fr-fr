---
title: Enregistrement de base - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>Enregistrement de base

Découvrez comment ajouter, modifier et supprimer des données à l’aide de vos classes de contexte et d’entité.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) sur GitHub.

## <a name="adding-data"></a>Ajout de données

Utilisez le *DbSet.Add* méthode pour ajouter de nouvelles instances de vos classes d’entité. Les données seront insérées dans la base de données lorsque vous appelez *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Les méthodes Add, attacher et mise à jour tout le travail sur le graphique complet d’entités passé, comme décrit dans la [les données associées](related-data.md) section. Ou bien, la propriété EntityEntry.State peut être utilisée pour définir l’état de simplement une seule entité. Par exemple, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Mise à jour des données

EF détecte automatiquement les modifications apportées à une entité existante qui est suivie par le contexte. Cela inclut les entités qui vous charge/requête à partir de la base de données et des entités qui ont été précédemment ajoutées et enregistrées dans la base de données.

Modifiez simplement les valeurs affectées aux propriétés, puis appelez *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Suppression de données

Utilisez le *DbSet.Remove* méthode pour supprimer les instances des classes d’entité.

Si l’entité existe déjà dans la base de données, il sera supprimé pendant *SaveChanges*. Si l’entité n’a pas encore été enregistrée pour la base de données (autrement dit, il est suivi comme ajouté), elle sera supprimée à partir du contexte et ne pourra plus être inséré lorsque *SaveChanges* est appelée.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Plusieurs opérations dans un seul SaveChanges

Vous pouvez combiner plusieurs opérations d’ajout/mise à jour/suppression en un seul appel à *SaveChanges*.

> [!NOTE]  
> Pour la plupart des fournisseurs de base de données, *SaveChanges* est transactionnelle. Cela signifie que toutes les opérations réussissent ou échouent et les opérations de ne jamais être gauche partiellement appliquées.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
