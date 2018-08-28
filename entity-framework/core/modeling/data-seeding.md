---
title: L’amorçage des données - EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994477"
---
# <a name="data-seeding"></a>Amorçage des données

> [!NOTE]  
> Cette fonctionnalité est une nouveauté d’EF Core 2.1.

L’amorçage des données permet de fournir des données initiales pour remplir une base de données. Contrairement à dans EF6, dans EF Core, données d’amorçage sont associée à un type d’entité dans le cadre de la configuration du modèle. Puis EF Core [migrations](xref:core/managing-schemas/migrations/index) peut calculer automatiquement les éléments insérant, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.

Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Pour ajouter des entités ayant une relation de valeurs de clés étrangères doivent être spécifiés. Les propriétés de clé étrangères sont fréquemment dans l’état de clichés instantanés, pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Une fois que les entités ont été ajoutées, il est recommandé d’utiliser [migrations](xref:core/managing-schemas/migrations/index) pour appliquer les modifications. 

Vous pouvez également utiliser `context.Database.EnsureCreated()` pour créer une nouvelle base de données contenant les données d’amorçage, par exemple pour une base de données de test ou lorsque vous utilisez le fournisseur en mémoire. Notez que si la base de données existe déjà, `EnsureCreated()` sera ni mettre à jour le schéma ni les données d’amorçage dans la base de données.
