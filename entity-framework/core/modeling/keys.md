---
title: Clés (principale)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655950"
---
# <a name="keys-primary"></a>Clés (primaires)

Une clé sert d’identificateur unique principal pour chaque instance d’entité. Lorsque vous utilisez une base de données relationnelle, cela correspond au concept d’une *clé primaire*. Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (pour plus d’informations, consultez [autres clés](alternate-keys.md) ).

L’une des méthodes suivantes peut être utilisée pour configurer/créer une clé primaire.

## <a name="conventions"></a>Conventions

Par Convention, une propriété nommée `Id` ou `<type name>Id` sera configurée comme clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour configurer une seule propriété en tant que clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété unique en tant que clé d’une entité.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé d’une entité (appelée clé composite). Les clés composites ne peuvent être configurées qu’à l’aide des conventions de l’API Fluent qui ne configureront jamais de clé composite, et vous ne pourrez pas utiliser d’annotations de données pour en configurer une.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
