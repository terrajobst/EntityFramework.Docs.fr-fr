---
title: Clés (principale)-EF Core
description: Comment configurer des clés pour des types d’entités lors de l’utilisation de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824623"
---
# <a name="keys-primary"></a>Clés (primaires)

Une clé sert d’identificateur unique principal pour chaque instance d’entité. Lorsque vous utilisez une base de données relationnelle, cela correspond au concept d’une *clé primaire*. Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (pour plus d’informations, consultez [autres clés](alternate-keys.md) ).

L’une des méthodes suivantes peut être utilisée pour configurer/créer une clé primaire.

## <a name="conventions"></a>Conventions

Par défaut, une propriété nommée `Id` ou `<type name>Id` sera configurée comme clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> Les [types d’entités détenues](xref:core/modeling/owned-entities) utilisent des règles différentes pour définir des clés.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour configurer une seule propriété en tant que clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété unique en tant que clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé d’une entité (appelée clé composite). Les clés composites ne peuvent être configurées qu’à l’aide des conventions de l’API Fluent qui ne configureront jamais de clé composite, et vous ne pourrez pas utiliser d’annotations de données pour en configurer une.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Types et valeurs de clé

EF Core prend en charge l’utilisation de propriétés de n’importe quel type primitif comme clé primaire, y compris `string`, `Guid`, `byte[]` et autres. Mais toutes les bases de données ne les prennent pas en charge. Dans certains cas, les valeurs de clé peuvent être converties automatiquement en un type pris en charge ; sinon, la conversion doit être [spécifiée manuellement](xref:core/modeling/value-conversions).

Les propriétés de clé doivent toujours avoir une valeur non définie par défaut lors de l’ajout d’une nouvelle entité au contexte, mais certains types sont [générés par la base de données](xref:core/modeling/generated-properties). Dans ce cas, EF tente de générer une valeur temporaire lorsque l’entité est ajoutée à des fins de suivi. Une fois [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) appelé, la valeur temporaire est remplacée par la valeur générée par la base de données.

> [!Important]
> Si une propriété de clé a une valeur générée par la base de données et qu’une valeur non définie par défaut est spécifiée lors de l’ajout d’une entité, EF suppose que l’entité existe déjà dans la base de données et essaiera de la mettre à jour au lieu d’en insérer une nouvelle. Pour éviter cela, désactivez la génération de valeur ou consultez [comment spécifier des valeurs explicites pour les propriétés générées](../saving/explicit-values-generated-properties.md).