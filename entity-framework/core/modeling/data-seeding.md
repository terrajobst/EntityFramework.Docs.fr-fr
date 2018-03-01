---
title: "Amorçage de données - EF Core"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a>Amorçage des données

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 2.1.

Amorçage de données permet de fournir les données initiales pour remplir une base de données. Contrairement à dans EF6, dans EF Core, amorçage de données est associé à un type d’entité dans le cadre de la configuration de modèle. Les migrations EF Core peuvent automatiquement calculez les insérer, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.

Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Pour ajouter des entités qui ont une relation de valeurs de clé étrangère doivent être spécifiés. Les propriétés de clé étrangères sont fréquemment dans l’état de l’ombre, donc pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
