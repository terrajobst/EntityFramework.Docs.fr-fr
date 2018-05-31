---
title: 'Enregistrement de base : EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006661"
---
# <a name="basic-save"></a><span data-ttu-id="aefac-102">Enregistrement de base</span><span class="sxs-lookup"><span data-stu-id="aefac-102">Basic Save</span></span>

<span data-ttu-id="aefac-103">Découvrez comment ajouter, modifier et supprimer des données à l’aide de vos classes de contexte et d’entité.</span><span class="sxs-lookup"><span data-stu-id="aefac-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="aefac-104">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="aefac-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="aefac-105">Ajout de données</span><span class="sxs-lookup"><span data-stu-id="aefac-105">Adding Data</span></span>

<span data-ttu-id="aefac-106">Utilisez la méthode *DbSet.Add* pour ajouter de nouvelles instances de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="aefac-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="aefac-107">Les données seront insérées dans la base de données lorsque vous appelez *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aefac-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="aefac-108">Les méthodes Add, Attach et Update fonctionnent toutes sur le graphique complet des entités passées, comme décrit dans la section [Données associées](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="aefac-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="aefac-109">Vous pouvez aussi utiliser la propriété EntityEntry.State pour définir l’état d’une seule entité.</span><span class="sxs-lookup"><span data-stu-id="aefac-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="aefac-110">Par exemple, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="aefac-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="aefac-111">Mise à jour des données</span><span class="sxs-lookup"><span data-stu-id="aefac-111">Updating Data</span></span>

<span data-ttu-id="aefac-112">EF détecte automatiquement les modifications apportées à une entité existante qui est suivie par le contexte.</span><span class="sxs-lookup"><span data-stu-id="aefac-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="aefac-113">Cela inclut les entités que vous chargez/demandez à partir de la base de données et des entités qui ont été précédemment ajoutées et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="aefac-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="aefac-114">Modifiez simplement les valeurs affectées aux propriétés, puis appelez *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aefac-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="aefac-115">Suppression de données</span><span class="sxs-lookup"><span data-stu-id="aefac-115">Deleting Data</span></span>

<span data-ttu-id="aefac-116">Utilisez la méthode *DbSet.Remove* pour supprimer les instances de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="aefac-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="aefac-117">Si l’entité existe déjà dans la base de données, elle sera supprimée pendant *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aefac-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="aefac-118">Si l’entité n’a pas encore été enregistrée dans la base de données (autrement dit, si elle est suivie comme ajoutée), elle sera supprimée du contexte et ne pourra plus être insérée lorsque *SaveChanges* est appelé.</span><span class="sxs-lookup"><span data-stu-id="aefac-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="aefac-119">Plusieurs opérations dans un seul SaveChanges</span><span class="sxs-lookup"><span data-stu-id="aefac-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="aefac-120">Vous pouvez combiner plusieurs opérations d’ajout/mise à jour/suppression en un seul appel à *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aefac-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="aefac-121">Pour la plupart des fournisseurs de base de données, *SaveChanges* est transactionnel.</span><span class="sxs-lookup"><span data-stu-id="aefac-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="aefac-122">Cela signifie que toutes les opérations réussissent ou échouent complètement et que les opérations ne sont jamais partiellement appliquées.</span><span class="sxs-lookup"><span data-stu-id="aefac-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
