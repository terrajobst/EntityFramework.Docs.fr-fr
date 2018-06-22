---
title: Amorçage de données - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163198"
---
# <a name="data-seeding"></a>Amorçage des données

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 2.1.

Amorçage de données permet de fournir les données initiales pour remplir une base de données. Contrairement à dans EF6, dans EF Core, amorçage de données est associé à un type d’entité dans le cadre de la configuration de modèle. Puis EF Core [migrations](xref:core/managing-schemas/migrations/index) peut calculer automatiquement les insérer, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.

Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Pour ajouter des entités qui ont une relation de valeurs de clé étrangère doivent être spécifiés. Les propriétés de clé étrangères sont fréquemment dans l’état de l’ombre, donc pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Une fois que les entités ont été ajoutées, il est recommandé d’utiliser [migrations](xref:core/managing-schemas/migrations/index) pour appliquer les modifications. 

Vous pouvez également utiliser `context.Database.EnsureCreated()` pour créer une nouvelle base de données contenant les données de valeur initiale, par exemple pour une base de données de test ou lorsque vous utilisez le fournisseur en mémoire. Notez que si la base de données existe déjà, `EnsureCreated()` ni met à jour le schéma ni les données de valeur initiale de la base de données.
