---
title: "Valeurs générées - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 2d79bf1339ebe522c39fe8971d908c30e1f4dca0
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="generated-values"></a>Valeur générée

## <a name="value-generation-patterns"></a>Valeur de génération de modèles

Il existe trois modèles de génération de valeur qui peuvent être utilisés pour les propriétés.

### <a name="no-value-generation"></a>Aucune valeur de génération

Aucune génération de valeur ne signifie que vous devez toujours fournir une valeur valide à enregistrer dans la base de données. Cette valeur valide doit être affectée aux nouvelles entités avant d’être ajoutés au contexte.

### <a name="value-generated-on-add"></a>Valeur générée sur Ajouter

Valeur générée sur Ajouter signifie une valeur générée pour les nouvelles entités.

Selon le fournisseur de base de données utilisé, les valeurs peuvent être générées côté client par EF ou dans la base de données. Si la valeur est générée par la base de données, EF peut affecter une valeur temporaire lorsque vous ajoutez l’entité au contexte. Cette valeur temporaire est ensuite remplacée par la valeur de la base de données générée pendant `SaveChanges()`.

Si vous ajoutez une entité au contexte qui a la valeur affectée à la propriété, puis EF essaiera d’insérer cette valeur au lieu de générer un nouveau. Une propriété est considérée comme ayant une valeur affectée si elle n’est pas attribuée la valeur par défaut CLR (`null` pour `string`, `0` pour `int`, `Guid.Empty` pour `Guid`, etc..). Pour plus d’informations, consultez [explicitement les valeurs des propriétés générées](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Comment la valeur est générée pour les entités ajoutées dépendent du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, mais d’autres peuvent vous amener à configurer manuellement la façon dont la valeur est générée.
>
> Par exemple, lorsque vous utilisez SQL Server, les valeurs seront être générées automatiquement pour `GUID` propriétés (à l’aide de l’algorithme GUID séquentiel de SQL Server). Toutefois, si vous indiquez qu’un `DateTime` propriété est générée lors de l’ajout, puis vous devez configurer un moyen pour les valeurs à générer. Pour faire cela, consiste à configurer une valeur par défaut de `GETDATE()`, consultez [les valeurs par défaut](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valeur générée dans Ajouter ou mettre à jour

Valeur générée lors de l’ajout ou la mise à jour signifie qu’une nouvelle valeur est générée chaque fois que l’enregistrement (insert ou update).

Comme `value generated on add`, si vous spécifiez une valeur pour la propriété sur une instance qui vient d’être ajoutée d’une entité, que la valeur sera insérée au lieu d’une valeur en cours de génération. Il est également possible de définir une valeur explicite lors de la mise à jour. Pour plus d’informations, consultez [explicitement les valeurs des propriétés générées](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Comment la valeur est générée pour les entités ajoutées et mis à jour dépend du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, tandis que d’autres vous obligent à configurer manuellement la façon dont la valeur est générée.
>
> Par exemple, lors de l’utilisation de SQL Server, `byte[]` propriétés qui sont définies comme générées sur Ajouter ou mettre à jour et les jetons d’accès concurrentiel, la mention sera installé avec le `rowversion` de type de données - afin que les valeurs seront générées dans la base de données. Toutefois, si vous indiquez qu’un `DateTime` propriété est générée dans Ajouter ou mettre à jour, puis vous devez configurer un moyen pour les valeurs à générer. Pour faire cela, consiste à configurer une valeur par défaut de `GETDATE()` (consultez [les valeurs par défaut](relational/default-values.md)) pour générer des valeurs pour les nouvelles lignes. Vous pouvez ensuite utiliser un déclencheur de base de données pour générer des valeurs pendant les mises à jour (par exemple, le déclencheur suivant de l’exemple).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Conventions

Par convention, les clés primaires sont d’un entier ou un type de données GUID sera installé pour que les valeurs générées sur Ajouter. Toutes les autres propriétés sera installé avec aucune génération de valeur.

## <a name="data-annotations"></a>Annotations de données

### <a name="no-value-generation-data-annotations"></a>Aucune génération de valeur (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valeur générée sur Ajouter (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées, il ne garantit pas que EF va installer le mécanisme pour générer des valeurs. Consultez [ajouter de la valeur générée sur](#value-generated-on-add) section pour plus d’informations.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valeur générée sur Ajouter ou mettre à jour (Annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées ou mis à jour, il ne garantit pas que EF va installer le mécanisme pour générer des valeurs. Consultez [valeur générée sur Ajouter ou mettre à jour](#value-generated-on-add-or-update) section pour plus d’informations.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent à modifier le modèle de génération de valeur pour une propriété donnée.

### <a name="no-value-generation-fluent-api"></a>Aucune génération de valeur (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valeur générée sur Ajouter (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées, il ne garantit pas que EF va installer le mécanisme pour générer des valeurs.  Consultez [ajouter de la valeur générée sur](#value-generated-on-add) section pour plus d’informations.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valeur générée sur Ajouter ou mettre à jour (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement EF de savoir que les valeurs sont générées pour les entités ajoutées ou mis à jour, il ne garantit pas que EF va installer le mécanisme pour générer des valeurs. Consultez [valeur générée sur Ajouter ou mettre à jour](#value-generated-on-add-or-update) section pour plus d’informations.
