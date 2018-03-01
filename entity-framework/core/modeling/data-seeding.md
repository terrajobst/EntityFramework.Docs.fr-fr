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
# <a name="data-seeding"></a><span data-ttu-id="e0001-102">Amorçage des données</span><span class="sxs-lookup"><span data-stu-id="e0001-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="e0001-103">Cette fonctionnalité est une nouveauté dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e0001-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e0001-104">Amorçage de données permet de fournir les données initiales pour remplir une base de données.</span><span class="sxs-lookup"><span data-stu-id="e0001-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="e0001-105">Contrairement à dans EF6, dans EF Core, amorçage de données est associé à un type d’entité dans le cadre de la configuration de modèle.</span><span class="sxs-lookup"><span data-stu-id="e0001-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="e0001-106">Les migrations EF Core peuvent automatiquement calculez les insérer, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.</span><span class="sxs-lookup"><span data-stu-id="e0001-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="e0001-107">Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="e0001-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="e0001-108">Pour ajouter des entités qui ont une relation de valeurs de clé étrangère doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="e0001-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="e0001-109">Les propriétés de clé étrangères sont fréquemment dans l’état de l’ombre, donc pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :</span><span class="sxs-lookup"><span data-stu-id="e0001-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
