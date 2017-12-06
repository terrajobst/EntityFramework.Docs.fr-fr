---
title: Enregistrement de base - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Enregistrement de base

Découvrez comment ajouter, modifier et supprimer des données à l’aide de vos classes de contexte et d’entité.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) sur GitHub.

## <a name="adding-data"></a>Ajout de données

Utilisez le *DbSet.Add* méthode pour ajouter de nouvelles instances de vos classes d’entité. Les données seront insérées dans la base de données lorsque vous appelez *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

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
