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
# <a name="basic-save"></a><span data-ttu-id="b3c2f-102">Enregistrement de base</span><span class="sxs-lookup"><span data-stu-id="b3c2f-102">Basic Save</span></span>

<span data-ttu-id="b3c2f-103">Découvrez comment ajouter, modifier et supprimer des données à l’aide de vos classes de contexte et d’entité.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="b3c2f-104">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="b3c2f-105">Ajout de données</span><span class="sxs-lookup"><span data-stu-id="b3c2f-105">Adding Data</span></span>

<span data-ttu-id="b3c2f-106">Utilisez le *DbSet.Add* méthode pour ajouter de nouvelles instances de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="b3c2f-107">Les données seront insérées dans la base de données lorsque vous appelez *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a><span data-ttu-id="b3c2f-108">Mise à jour des données</span><span class="sxs-lookup"><span data-stu-id="b3c2f-108">Updating Data</span></span>

<span data-ttu-id="b3c2f-109">EF détecte automatiquement les modifications apportées à une entité existante qui est suivie par le contexte.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-109">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="b3c2f-110">Cela inclut les entités qui vous charge/requête à partir de la base de données et des entités qui ont été précédemment ajoutées et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-110">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="b3c2f-111">Modifiez simplement les valeurs affectées aux propriétés, puis appelez *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-111">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="b3c2f-112">Suppression de données</span><span class="sxs-lookup"><span data-stu-id="b3c2f-112">Deleting Data</span></span>

<span data-ttu-id="b3c2f-113">Utilisez le *DbSet.Remove* méthode pour supprimer les instances des classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-113">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="b3c2f-114">Si l’entité existe déjà dans la base de données, il sera supprimé pendant *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-114">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="b3c2f-115">Si l’entité n’a pas encore été enregistrée pour la base de données (autrement dit, il est suivi comme ajouté), elle sera supprimée à partir du contexte et ne pourra plus être inséré lorsque *SaveChanges* est appelée.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-115">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="b3c2f-116">Plusieurs opérations dans un seul SaveChanges</span><span class="sxs-lookup"><span data-stu-id="b3c2f-116">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="b3c2f-117">Vous pouvez combiner plusieurs opérations d’ajout/mise à jour/suppression en un seul appel à *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-117">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="b3c2f-118">Pour la plupart des fournisseurs de base de données, *SaveChanges* est transactionnelle.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-118">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="b3c2f-119">Cela signifie que toutes les opérations réussissent ou échouent et les opérations de ne jamais être gauche partiellement appliquées.</span><span class="sxs-lookup"><span data-stu-id="b3c2f-119">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
