---
title: Valeurs générées-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197429"
---
# <a name="generated-values"></a>Valeurs générées

## <a name="value-generation-patterns"></a>Modèles de génération de valeur

Il existe trois modèles de génération de valeur qui peuvent être utilisés pour les propriétés :
* Aucune génération de valeur
* Valeur générée lors de l’ajout
* Valeur générée lors de l’ajout ou de la mise à jour

### <a name="no-value-generation"></a>Aucune génération de valeur

Aucune génération de valeur ne signifie que vous fournissez toujours une valeur valide à enregistrer dans la base de données. Cette valeur valide doit être assignée aux nouvelles entités avant d’être ajoutées au contexte.

### <a name="value-generated-on-add"></a>Valeur générée lors de l’ajout

La valeur générée lors de l’ajout signifie qu’une valeur est générée pour les nouvelles entités.

Selon le fournisseur de base de données utilisé, les valeurs peuvent être générées côté client par EF ou dans la base de données. Si la valeur est générée par la base de données, EF peut affecter une valeur temporaire lorsque vous ajoutez l’entité au contexte. Cette valeur temporaire sera ensuite remplacée par la valeur générée par la base `SaveChanges()`de données pendant.

Si vous ajoutez une entité au contexte qui a une valeur affectée à la propriété, EF tente d’insérer cette valeur au lieu d’en générer une nouvelle. Une propriété est considérée comme ayant une valeur affectée si la valeur CLR par défaut n’est pas affectée`null` ( `string` `0` pour, `int`pour `Guid.Empty` , `Guid`pour, etc.). Pour plus d’informations, consultez [valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> La façon dont la valeur est générée pour les entités ajoutées dépend du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, mais d’autres peuvent vous obliger à configurer manuellement la manière dont la valeur est générée.
>
> Par exemple, lors de l’utilisation de SQL Server, les valeurs sont `GUID` générées automatiquement pour les propriétés (à l’aide de l’algorithme de GUID SQL Server séquentiel). Toutefois, si vous spécifiez qu' `DateTime` une propriété est générée lors de l’ajout, vous devez configurer un moyen de générer les valeurs. Pour ce faire, vous pouvez configurer une valeur par défaut de `GETDATE()`, voir [valeurs par défaut](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valeur générée lors de l’ajout ou de la mise à jour

La valeur générée à l’ajout ou à la mise à jour signifie qu’une nouvelle valeur est générée chaque fois que l’enregistrement est enregistré (Insert ou Update).

Comme `value generated on add`, si vous spécifiez une valeur pour la propriété sur une instance nouvellement ajoutée d’une entité, cette valeur est insérée au lieu d’une valeur générée. Il est également possible de définir une valeur explicite lors de la mise à jour. Pour plus d’informations, consultez [valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La façon dont la valeur est générée pour les entités ajoutées et mises à jour dépend du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, tandis que d’autres vous obligent à configurer manuellement la façon dont la valeur est générée.
> 
> Par exemple, lors de l’utilisation `byte[]` de SQL Server, les propriétés définies comme générées lors de l’ajout ou de la mise à jour et marquées comme `rowversion` jetons d’accès concurrentiel sont configurées avec le type de données, de sorte que les valeurs sont générées dans la base de données. Toutefois, si vous spécifiez qu' `DateTime` une propriété est générée lors de l’ajout ou de la mise à jour, vous devez configurer un moyen de générer les valeurs. Pour ce faire, vous pouvez configurer une valeur `GETDATE()` par défaut (voir les [valeurs par défaut](relational/default-values.md)) pour générer des valeurs pour les nouvelles lignes. Vous pouvez ensuite utiliser un déclencheur de base de données pour générer des valeurs lors des mises à jour (par exemple, le déclencheur suivant).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Conventions

Par Convention, les clés primaires non composites de type short, int, long ou GUID seront configurées de façon à ce que les valeurs soient générées lors de l’ajout. Toutes les autres propriétés seront configurées sans génération de valeur.

## <a name="data-annotations"></a>Annotations de données

### <a name="no-value-generation-data-annotations"></a>Aucune génération de valeur (annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valeur générée lors de l’ajout (annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Cela permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées. cela ne garantit pas qu’EF configure le mécanisme réel pour générer des valeurs. Pour plus d’informations, consultez la section [valeur générée sur l’ajout](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valeur générée lors de l’ajout ou de la mise à jour (annotations de données)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées ou mises à jour, mais elle ne garantit pas que EF configure le mécanisme réel pour générer des valeurs. Pour plus d’informations, consultez la section [valeur générée dans la section Ajouter ou mettre à jour](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour modifier le modèle de génération de valeur pour une propriété donnée.

### <a name="no-value-generation-fluent-api"></a>Aucune génération de valeur (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valeur générée lors de l’ajout (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées. cela ne garantit pas qu’EF configure le mécanisme réel pour générer des valeurs.  Pour plus d’informations, consultez la section [valeur générée sur l’ajout](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valeur générée lors de l’ajout ou de la mise à jour (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Cela permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées ou mises à jour, mais elle ne garantit pas que EF configure le mécanisme réel pour générer des valeurs. Pour plus d’informations, consultez la section [valeur générée dans la section Ajouter ou mettre à jour](#value-generated-on-add-or-update) .
