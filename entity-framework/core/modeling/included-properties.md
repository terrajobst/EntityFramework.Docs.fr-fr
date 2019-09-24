---
title: Inclusion de & exclusion de propriétés-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197413"
---
# <a name="including--excluding-properties"></a>Inclusion & exclusion de propriétés

L’inclusion d’une propriété dans le modèle signifie qu’EF a des métadonnées sur cette propriété et tente de lire et d’écrire des valeurs à partir de/dans la base de données.

## <a name="conventions"></a>Conventions

Par Convention, les propriétés publiques avec un accesseur get et un accesseur Set seront incluses dans le modèle.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour exclure une propriété du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour exclure une propriété du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
