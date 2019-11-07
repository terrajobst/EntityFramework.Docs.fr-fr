---
title: Héritage-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655687"
---
# <a name="inheritance"></a>Héritage

L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.

## <a name="conventions"></a>Conventions

Par Convention, il revient au fournisseur de base de données de déterminer la façon dont l’héritage sera représenté dans la base de données. Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour savoir comment cela est géré avec un fournisseur de base de données relationnelle.

EF ne configure l’héritage que si deux ou plusieurs types hérités sont explicitement inclus dans le modèle. EF ne recherche pas les types de base ou dérivés qui n’étaient pas inclus dans le modèle. Vous pouvez inclure des types dans le modèle en exposant un *DbSet\<tente >* pour chaque type dans la hiérarchie d’héritage.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si vous ne souhaitez pas exposer un *DbSet\<tente >* pour une ou plusieurs entités de la hiérarchie, vous pouvez utiliser l’API Fluent pour vous assurer qu’elles sont incluses dans le modèle.
Et si vous ne vous fiez pas aux conventions, vous pouvez spécifier explicitement le type de base à l’aide de `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Vous pouvez utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité de la hiérarchie.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.

## <a name="fluent-api"></a>API Fluent

L’API Fluent pour l’héritage dépend du fournisseur de base de données que vous utilisez. Consultez [héritage (base de données relationnelle)](relational/inheritance.md) pour la configuration que vous pouvez effectuer pour un fournisseur de base de données relationnelle.
