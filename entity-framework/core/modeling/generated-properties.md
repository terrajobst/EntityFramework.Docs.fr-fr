---
title: Valeurs générées-EF Core
description: Comment configurer la génération de valeurs pour les propriétés lors de l’utilisation de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502030"
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

Selon le fournisseur de base de données utilisé, les valeurs peuvent être générées côté client par EF ou dans la base de données. Si la valeur est générée par la base de données, EF peut affecter une valeur temporaire lorsque vous ajoutez l’entité au contexte. Cette valeur temporaire sera ensuite remplacée par la valeur générée par la base de données pendant `SaveChanges()`.

Si vous ajoutez une entité au contexte qui a une valeur affectée à la propriété, EF tente d’insérer cette valeur au lieu d’en générer une nouvelle. Une propriété est considérée comme ayant une valeur affectée si elle n’est pas affectée à la valeur CLR par défaut (`null` pour `string`, `0` pour `int`, `Guid.Empty` pour `Guid`, etc.). Pour plus d’informations, consultez [valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La façon dont la valeur est générée pour les entités ajoutées dépend du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, mais d’autres peuvent vous obliger à configurer manuellement la manière dont la valeur est générée.
>
> Par exemple, lors de l’utilisation de SQL Server, les valeurs sont générées automatiquement pour les propriétés de `GUID` (à l’aide de l’algorithme de GUID séquentiel SQL Server). Toutefois, si vous spécifiez qu’une propriété `DateTime` est générée lors de l’ajout, vous devez configurer un moyen de générer les valeurs. Pour ce faire, vous pouvez configurer une valeur par défaut de `GETDATE()`, voir [valeurs par défaut](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valeur générée lors de l’ajout ou de la mise à jour

La valeur générée à l’ajout ou à la mise à jour signifie qu’une nouvelle valeur est générée chaque fois que l’enregistrement est enregistré (Insert ou Update).

Comme `value generated on add`, si vous spécifiez une valeur pour la propriété sur une instance nouvellement ajoutée d’une entité, cette valeur est insérée au lieu d’une valeur générée. Il est également possible de définir une valeur explicite lors de la mise à jour. Pour plus d’informations, consultez [valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La façon dont la valeur est générée pour les entités ajoutées et mises à jour dépend du fournisseur de base de données utilisé. Les fournisseurs de base de données peuvent configurer automatiquement la génération de valeur pour certains types de propriété, tandis que d’autres vous obligent à configurer manuellement la façon dont la valeur est générée.
>
> Par exemple, lors de l’utilisation de SQL Server, `byte[]` propriétés définies comme générées lors de l’ajout ou de la mise à jour et marquées comme jetons d’accès concurrentiel, seront configurées avec le type de données `rowversion`-afin que les valeurs soient générées dans la base de données. Toutefois, si vous spécifiez qu’une propriété `DateTime` est générée lors de l’ajout ou de la mise à jour, vous devez configurer un moyen de générer les valeurs. Une façon de procéder consiste à configurer une valeur par défaut de `GETDATE()` (voir les [valeurs par défaut](relational/default-values.md)) pour générer des valeurs pour les nouvelles lignes. Vous pouvez ensuite utiliser un déclencheur de base de données pour générer des valeurs lors des mises à jour (par exemple, le déclencheur suivant).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="value-generated-on-add"></a>Valeur générée lors de l’ajout

Par Convention, les clés primaires non composites de type short, int, long ou GUID sont configurées pour avoir des valeurs générées pour les entités insérées, si une valeur n’est pas fournie par l’application. Votre fournisseur de base de données prend généralement en charge la configuration requise ; par exemple, une clé primaire numérique dans SQL Server est automatiquement configurée pour être une colonne d’identité.

Vous pouvez configurer n’importe quelle propriété pour que sa valeur soit générée pour les entités insérées comme suit :

### <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> Cela permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées. cela ne garantit pas qu’EF configure le mécanisme réel pour générer des valeurs. Pour plus d’informations, consultez la section [valeur générée sur l’ajout](#value-generated-on-add) .

### <a name="default-values"></a>Valeurs par défaut

Sur les bases de données relationnelles, une colonne peut être configurée avec une valeur par défaut ; Si une ligne est insérée sans valeur pour cette colonne, la valeur par défaut est utilisée.

Vous pouvez configurer une valeur par défaut sur une propriété :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

Vous pouvez également spécifier un fragment SQL utilisé pour calculer la valeur par défaut :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

La spécification d’une valeur par défaut configure implicitement la propriété en tant que valeur générée lors de l’ajout.

## <a name="value-generated-on-add-or-update"></a>Valeur générée lors de l’ajout ou de la mise à jour

### <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> Cela permet simplement à EF de savoir que les valeurs sont générées pour les entités ajoutées ou mises à jour, mais elle ne garantit pas que EF configure le mécanisme réel pour générer des valeurs. Pour plus d’informations, consultez la section [valeur générée dans la section Ajouter ou mettre à jour](#value-generated-on-add-or-update) .

### <a name="computed-columns"></a>Colonnes calculées

Sur certaines bases de données relationnelles, une colonne peut être configurée de manière à ce que sa valeur soit calculée dans la base de données, généralement avec une expression faisant référence à d’autres colonnes :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> Dans certains cas, la valeur de la colonne est calculée chaque fois qu’elle est extraite (parfois appelée colonnes *virtuelles* ), et dans d’autres, elle est calculée à chaque mise à jour de la ligne et stockée (parfois appelée colonnes *stockées* ou *conservées* ). Cela varie d’un fournisseur de base de données à l’autre.

## <a name="no-value-generation"></a>Aucune génération de valeur

La désactivation de la génération de valeur sur une propriété est généralement nécessaire si une convention la configure pour la génération de valeur. Par exemple, si vous avez une clé primaire de type int, elle sera définie implicitement comme valeur générée lors de l’ajout. vous pouvez désactiver ce code à l’aide des éléments suivants :

### <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
