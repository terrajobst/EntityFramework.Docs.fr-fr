---
title: Héritage-EF Core
description: Comment configurer l’héritage de type d’entité à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824677"
---
# <a name="inheritance"></a><span data-ttu-id="37c1b-103">Héritage</span><span class="sxs-lookup"><span data-stu-id="37c1b-103">Inheritance</span></span>

<span data-ttu-id="37c1b-104">L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="37c1b-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="37c1b-105">Conventions</span><span class="sxs-lookup"><span data-stu-id="37c1b-105">Conventions</span></span>

<span data-ttu-id="37c1b-106">Par défaut, il revient au fournisseur de base de données de déterminer la façon dont l’héritage sera représenté dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="37c1b-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="37c1b-107">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour savoir comment cela est géré avec un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="37c1b-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="37c1b-108">EF ne configure l’héritage que si deux ou plusieurs types hérités sont explicitement inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="37c1b-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="37c1b-109">EF ne recherche pas les types de base ou dérivés qui n’étaient pas inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="37c1b-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="37c1b-110">Vous pouvez inclure des types dans le modèle en exposant un *DbSet\<tente >* pour chaque type dans la hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="37c1b-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="37c1b-111">Si vous ne souhaitez pas exposer un *DbSet\<tente >* pour une ou plusieurs entités de la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’elles sont incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="37c1b-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="37c1b-112">Et si vous ne vous fiez pas aux conventions, vous pouvez spécifier explicitement le type de base à l’aide de `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="37c1b-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="37c1b-113">Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="37c1b-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="37c1b-114">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="37c1b-114">Data Annotations</span></span>

<span data-ttu-id="37c1b-115">Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="37c1b-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="37c1b-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="37c1b-116">Fluent API</span></span>

<span data-ttu-id="37c1b-117">L’API Fluent pour l’héritage dépend du fournisseur de base de données que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="37c1b-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="37c1b-118">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="37c1b-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
