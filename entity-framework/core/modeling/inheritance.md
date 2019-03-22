---
title: Héritage - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319125"
---
# <a name="inheritance"></a>Héritage

L’héritage dans le modèle EF est utilisé pour contrôler la façon dont l’héritage dans les classes d’entité est représentée dans la base de données.

## <a name="conventions"></a>Conventions

Par convention, il incombe au fournisseur de base de données de déterminer comment l’héritage est représenté dans la base de données. Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour comment ceci est géré avec un fournisseur de base de données relationnelle.

EF n'est que le programme d’installation l’héritage si deux ou plusieurs des types hérités sont explicitement inclus dans le modèle. EF n’analyse pas pour les types de base ou dérivés qui ne figuraient pas dans le cas contraire dans le modèle. Vous pouvez inclure des types dans le modèle en exposant un *DbSet<TEntity>*  pour chaque type dans la hiérarchie d’héritage.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si vous ne souhaitez pas exposer un *DbSet<TEntity>*  pour une ou plusieurs entités dans la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’ils sont inclus dans le modèle.
Et si vous ne vous fiez conventions, vous pouvez spécifier le type de base explicitement à l’aide de `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité à partir de la hiérarchie.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas utiliser des Annotations de données pour configurer l’héritage.

## <a name="fluent-api"></a>API Fluent

Le fournisseur de base de données que vous utilisez dépend de l’API Fluent pour l’héritage. Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.
