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
# <a name="data-seeding"></a><span data-ttu-id="8a999-102">Amorçage des données</span><span class="sxs-lookup"><span data-stu-id="8a999-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="8a999-103">Cette fonctionnalité est une nouveauté d’EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8a999-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="8a999-104">L’amorçage des données permet de fournir des données initiales pour remplir une base de données.</span><span class="sxs-lookup"><span data-stu-id="8a999-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="8a999-105">Contrairement à dans EF6, dans EF Core, données d’amorçage sont associée à un type d’entité dans le cadre de la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="8a999-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="8a999-106">Puis EF Core [migrations](xref:core/managing-schemas/migrations/index) peut calculer automatiquement les éléments insérant, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.</span><span class="sxs-lookup"><span data-stu-id="8a999-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="8a999-107">Par exemple, vous pouvez utiliser ceci pour configurer les données de valeur initiale pour un `Blog` dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="8a999-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="8a999-108">Pour ajouter des entités ayant une relation de valeurs de clés étrangères doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8a999-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="8a999-109">Les propriétés de clé étrangères sont fréquemment dans l’état de clichés instantanés, pour être en mesure de définir les valeurs à une classe anonyme doit être utilisé :</span><span class="sxs-lookup"><span data-stu-id="8a999-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="8a999-110">Une fois que les entités ont été ajoutées, il est recommandé d’utiliser [migrations](xref:core/managing-schemas/migrations/index) pour appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="8a999-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="8a999-111">Vous pouvez également utiliser `context.Database.EnsureCreated()` pour créer une nouvelle base de données contenant les données d’amorçage, par exemple pour une base de données de test ou lorsque vous utilisez le fournisseur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="8a999-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="8a999-112">Notez que si la base de données existe déjà, `EnsureCreated()` sera ni mettre à jour le schéma ni les données d’amorçage dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8a999-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
