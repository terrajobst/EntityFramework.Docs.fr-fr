---
title: Héritage-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197287"
---
# <a name="inheritance"></a><span data-ttu-id="1cfc6-102">Héritage</span><span class="sxs-lookup"><span data-stu-id="1cfc6-102">Inheritance</span></span>

<span data-ttu-id="1cfc6-103">L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1cfc6-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="1cfc6-104">Conventions</span></span>

<span data-ttu-id="1cfc6-105">Par Convention, il revient au fournisseur de base de données de déterminer la façon dont l’héritage sera représenté dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="1cfc6-106">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour savoir comment cela est géré avec un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="1cfc6-107">EF ne configure l’héritage que si deux ou plusieurs types hérités sont explicitement inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="1cfc6-108">EF ne recherche pas les types de base ou dérivés qui n’étaient pas inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="1cfc6-109">Vous pouvez inclure des types dans le modèle en exposant un *DbSet<TEntity>*  pour chaque type dans la hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="1cfc6-110">Si vous ne souhaitez pas exposer un *DbSet<TEntity>*  pour une ou plusieurs entités dans la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’elles sont incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="1cfc6-111">Et si vous ne vous fiez pas aux conventions, vous pouvez spécifier explicitement le type `HasBaseType`de base à l’aide de.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="1cfc6-112">Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1cfc6-113">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="1cfc6-113">Data Annotations</span></span>

<span data-ttu-id="1cfc6-114">Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1cfc6-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="1cfc6-115">Fluent API</span></span>

<span data-ttu-id="1cfc6-116">L’API Fluent pour l’héritage dépend du fournisseur de base de données que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="1cfc6-117">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="1cfc6-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
