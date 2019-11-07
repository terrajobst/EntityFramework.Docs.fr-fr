---
title: Index-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655699"
---
# <a name="indexes"></a><span data-ttu-id="187e3-102">Index</span><span class="sxs-lookup"><span data-stu-id="187e3-102">Indexes</span></span>

<span data-ttu-id="187e3-103">Les index sont un concept commun entre de nombreux magasins de données.</span><span class="sxs-lookup"><span data-stu-id="187e3-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="187e3-104">Bien que leur implémentation dans le magasin de données puisse varier, elles sont utilisées pour rendre les recherches basées sur une colonne (ou un ensemble de colonnes) plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="187e3-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="187e3-105">Conventions</span><span class="sxs-lookup"><span data-stu-id="187e3-105">Conventions</span></span>

<span data-ttu-id="187e3-106">Par Convention, un index est créé dans chaque propriété (ou ensemble de propriétés) utilisée comme clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="187e3-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="187e3-107">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="187e3-107">Data Annotations</span></span>

<span data-ttu-id="187e3-108">Les index ne peuvent pas être créés à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="187e3-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="187e3-109">API Fluent</span><span class="sxs-lookup"><span data-stu-id="187e3-109">Fluent API</span></span>

<span data-ttu-id="187e3-110">Vous pouvez utiliser l’API Fluent pour spécifier un index sur une seule propriété.</span><span class="sxs-lookup"><span data-stu-id="187e3-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="187e3-111">Par défaut, les index sont non uniques.</span><span class="sxs-lookup"><span data-stu-id="187e3-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="187e3-112">Vous pouvez également spécifier qu’un index doit être unique, ce qui signifie que deux entités ne peuvent pas avoir la même valeur pour la ou les propriétés spécifiées.</span><span class="sxs-lookup"><span data-stu-id="187e3-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="187e3-113">Vous pouvez également spécifier un index sur plusieurs colonnes.</span><span class="sxs-lookup"><span data-stu-id="187e3-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="187e3-114">Il n’existe qu’un seul index par jeu de propriétés distinct.</span><span class="sxs-lookup"><span data-stu-id="187e3-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="187e3-115">Si vous utilisez l’API Fluent pour configurer un index sur un ensemble de propriétés pour lequel un index est déjà défini, soit par Convention, soit par configuration précédente, vous allez modifier la définition de cet index.</span><span class="sxs-lookup"><span data-stu-id="187e3-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="187e3-116">Cela est utile si vous souhaitez configurer davantage un index qui a été créé par Convention.</span><span class="sxs-lookup"><span data-stu-id="187e3-116">This is useful if you want to further configure an index that was created by convention.</span></span>
