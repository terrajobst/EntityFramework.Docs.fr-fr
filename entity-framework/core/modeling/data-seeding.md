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
---
# <a name="data-seeding"></a><span data-ttu-id="343f3-102">Amorçage des données</span><span class="sxs-lookup"><span data-stu-id="343f3-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="343f3-103">Cette fonctionnalité est une nouveauté dans EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="343f3-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="343f3-104">Amorçage de données permet de fournir les données initiales pour remplir une base de données.</span><span class="sxs-lookup"><span data-stu-id="343f3-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="343f3-105">Contrairement à dans EF6, dans EF Core, amorçage de données est associé à un type d’entité dans le cadre de la configuration de modèle.</span><span class="sxs-lookup"><span data-stu-id="343f3-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="343f3-106">Puis EF Core [migrations](xref:core/managing-schemas/migrations/index) peut calculer automatiquement les insérer, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.</span><span class="sxs-lookup"><span data-stu-id="343f3-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="343f3-107">Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="343f3-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="343f3-108">Pour ajouter des entités qui ont une relation de valeurs de clé étrangère doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="343f3-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="343f3-109">Les propriétés de clé étrangères sont fréquemment dans l’état de l’ombre, donc pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :</span><span class="sxs-lookup"><span data-stu-id="343f3-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="343f3-110">Une fois que les entités ont été ajoutées, il est recommandé d’utiliser [migrations](xref:core/managing-schemas/migrations/index) pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="343f3-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="343f3-111">Vous pouvez également utiliser `context.Database.EnsureCreated()` pour créer une nouvelle base de données contenant les données de valeur initiale, par exemple pour une base de données de test ou lorsque vous utilisez le fournisseur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="343f3-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="343f3-112">Notez que si la base de données existe déjà, `EnsureCreated()` ni met à jour le schéma ni les données de valeur initiale de la base de données.</span><span class="sxs-lookup"><span data-stu-id="343f3-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
