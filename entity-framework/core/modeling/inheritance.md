---
title: Héritage - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052779"
---
# <a name="inheritance"></a><span data-ttu-id="8c0b1-102">Héritage</span><span class="sxs-lookup"><span data-stu-id="8c0b1-102">Inheritance</span></span>

<span data-ttu-id="8c0b1-103">L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8c0b1-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="8c0b1-104">Conventions</span></span>

<span data-ttu-id="8c0b1-105">Par convention, il incombe au fournisseur de base de données de déterminer comment l’héritage est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="8c0b1-106">Consultez [l’héritage (base de données relationnelle)](relational/inheritance.md) pour la gestion avec un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="8c0b1-107">EF sera uniquement le programme d’installation d’héritage si deux ou plusieurs types hérités sont explicitement inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="8c0b1-108">EF n’analyse pas pour les types de base ou dérivées qui ne figuraient pas dans le cas contraire dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="8c0b1-109">Vous pouvez inclure des types dans le modèle en exposant un *DbSet<TEntity>*  pour chaque type dans la hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="8c0b1-110">Si vous ne souhaitez pas exposer un *DbSet<TEntity>*  pour une ou plusieurs entités dans la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’ils sont inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="8c0b1-111">Et si vous ne vous fiez conventions, vous pouvez spécifier le type de base explicitement à l’aide de `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="8c0b1-112">Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité à partir de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8c0b1-113">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="8c0b1-113">Data Annotations</span></span>

<span data-ttu-id="8c0b1-114">Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8c0b1-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8c0b1-115">Fluent API</span></span>

<span data-ttu-id="8c0b1-116">Le fournisseur de base de données que vous utilisez dépend de l’API Fluent d’héritage.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="8c0b1-117">Consultez [l’héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="8c0b1-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
