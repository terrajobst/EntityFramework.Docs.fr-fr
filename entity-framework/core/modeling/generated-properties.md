---
title: Valeurs générées - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250697"
---
# <a name="generated-values"></a>Valeurs générées

## <a name="value-generation-patterns"></a>Modèles de génération de valeur

Il existe trois modèles de génération de valeur qui peuvent être utilisées pour les propriétés :
* Aucune génération de valeur
* Valeur générée sur Ajouter
* Valeur générée dans Ajouter ou mettre à jour

### <a name="no-value-generation"></a>Aucune génération de valeur

Aucune génération de valeur ne signifie que vous fournirez toujours une valeur valide pour être enregistrés dans la base de données. Cette valeur valide doit être affectée aux nouvelles entités avant d’être ajoutés au contexte.

### <a name="value-generated-on-add"></a>Valeur générée sur Ajouter

Valeur générée sur Ajouter signifie une valeur générée pour les nouvelles entités.

Selon le fournisseur de base de données utilisé, les valeurs peuvent être générées côté client par Entity Framework ou dans la base de données. Si la valeur est générée par la base de données, EF peut affecter une valeur temporaire lorsque vous ajoutez l’entité au contexte. Cette valeur temporaire est alors remplacée par la valeur de la base de données générée au cours de `SaveChanges()`.

Si vous ajoutez une entité au contexte qui a la valeur affectée à la propriété, puis EF tente insérer cette valeur au lieu de générer un nouveau. Une propriété est considérée comme ayant une valeur affectée s’il n’est pas affecté la valeur par défaut CLR (`null` pour `string`, `0` pour `int`, `Guid.Empty` pour `Guid`, etc..). Pour plus d’informations, consultez [explicitement les valeurs des propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Comment la valeur est générée pour les entités ajoutées dépendent du fournisseur de base de données utilisé. Fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, mais d’autres peuvent nécessiter vous permettent de configurer manuellement la façon dont la valeur est générée.
>
> Par exemple, lorsque vous utilisez SQL Server, les valeurs seront être générées automatiquement pour `GUID` propriétés (à l’aide de l’algorithme GUID séquentiel de SQL Server). Toutefois, si vous indiquez qu’un `DateTime` propriété est générée lors de l’ajout, puis vous devez configurer un moyen pour les valeurs à être généré. Pour faire cela, consiste à configurer une valeur par défaut de `GETDATE()`, consultez [valeurs par défaut](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valeur générée dans Ajouter ou mettre à jour

Valeur générée lors de l’ajout ou la mise à jour signifie qu’une nouvelle valeur est générée chaque fois que l’enregistrement (insert ou update).

Comme `value generated on add`, si vous spécifiez une valeur pour la propriété sur une instance nouvellement ajoutée d’une entité, que la valeur sera insérée plutôt qu’une valeur en cours de génération. Il est également possible de définir une valeur explicite lors de la mise à jour. Pour plus d’informations, consultez [explicitement les valeurs des propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Comment la valeur est générée pour les entités ajoutées et mis à jour dépend du fournisseur de base de données utilisé. Fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, tandis que d’autres nécessiteront vous permettent de configurer manuellement la façon dont la valeur est générée.
> 
> Par exemple, lors de l’utilisation de SQL Server, `byte[]` propriétés qui sont définies comme générées sur Ajouter ou mettre à jour et les jetons d’accès concurrentiel, la mention sera configuré avec le `rowversion` de type de données - afin que les valeurs dans la base de données va être générés. Toutefois, si vous indiquez qu’un `DateTime` propriété est générée sous Ajouter ou mettre à jour, puis vous devez configurer un moyen pour les valeurs à être généré. Pour faire cela, consiste à configurer une valeur par défaut de `GETDATE()` (consultez [valeurs par défaut](relational/default-values.md)) pour générer des valeurs pour les nouvelles lignes. Vous pouvez ensuite utiliser un déclencheur de base de données pour générer des valeurs pendant les mises à jour (par exemple, le déclencheur suivant de l’exemple).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Conventions

Par convention, non composites des clés primaires de type short, int, long ou Guid sera configuré pour avoir des valeurs générées sur Ajouter. Toutes les autres propriétés sera configuré avec aucune génération de valeur.

## <a name="data-annotations"></a>Annotations de données

### <a name="no-value-generation-data-annotations"></a>Aucune génération de valeur (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valeur générée sur Ajouter (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées, cela ne garantit pas que EF vous permettront de configurer le mécanisme pour générer des valeurs. Consultez [ajouter de la valeur générée sur](#value-generated-on-add) section pour plus d’informations.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valeur générée sur Ajouter ou mettre à jour (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées ou mises à jour, il ne garantit pas que EF vous permettront de configurer le mécanisme pour générer des valeurs. Consultez [valeur générée sur Ajouter ou mettre à jour](#value-generated-on-add-or-update) section pour plus d’informations.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent à modifier le modèle de génération de valeur pour une propriété donnée.

### <a name="no-value-generation-fluent-api"></a>Aucune génération de valeur (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valeur générée sur Ajouter (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` Venez informe Entity Framework que les valeurs sont générées pour les entités ajoutées, cela ne garantit pas que EF vous permettront de configurer le mécanisme pour générer des valeurs.  Consultez [ajouter de la valeur générée sur](#value-generated-on-add) section pour plus d’informations.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valeur générée sur Ajouter ou mettre à jour (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées ou mises à jour, il ne garantit pas que EF vous permettront de configurer le mécanisme pour générer des valeurs. Consultez [valeur générée sur Ajouter ou mettre à jour](#value-generated-on-add-or-update) section pour plus d’informations.
