---
title: Héritage - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995894"
---
# <a name="inheritance"></a><span data-ttu-id="cfa10-102">Héritage</span><span class="sxs-lookup"><span data-stu-id="cfa10-102">Inheritance</span></span>

<span data-ttu-id="cfa10-103">L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cfa10-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="cfa10-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="cfa10-104">Conventions</span></span>

<span data-ttu-id="cfa10-105">Par convention, il incombe au fournisseur de base de données de déterminer comment l’héritage est représenté dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cfa10-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="cfa10-106">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour comment ceci est géré avec un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="cfa10-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="cfa10-107">EF n'est que le programme d’installation l’héritage si deux ou plusieurs des types hérités sont explicitement inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="cfa10-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="cfa10-108">EF n’analyse pas pour les types de base ou dérivés qui ne figuraient pas dans le cas contraire dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="cfa10-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="cfa10-109">Vous pouvez inclure des types dans le modèle en exposant un *DbSet<TEntity>*  pour chaque type dans la hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="cfa10-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="cfa10-110">Si vous ne souhaitez pas exposer un *DbSet<TEntity>*  pour une ou plusieurs entités dans la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’ils sont inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="cfa10-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="cfa10-111">Et si vous ne vous fiez conventions, vous pouvez spécifier le type de base explicitement à l’aide de `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="cfa10-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="cfa10-112">Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité à partir de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="cfa10-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cfa10-113">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="cfa10-113">Data Annotations</span></span>

<span data-ttu-id="cfa10-114">Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="cfa10-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cfa10-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="cfa10-115">Fluent API</span></span>

<span data-ttu-id="cfa10-116">Le fournisseur de base de données que vous utilisez dépend de l’API Fluent pour l’héritage.</span><span class="sxs-lookup"><span data-stu-id="cfa10-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="cfa10-117">Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="cfa10-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
