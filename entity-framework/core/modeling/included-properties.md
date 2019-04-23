---
title: Inclusion et exclusion de propriétés - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929821"
---
# <a name="including--excluding-properties"></a>Inclusion et exclusion de propriétés

Y compris une propriété dans le modèle signifie qu’EF comporte des métadonnées sur cette propriété et va tenter de lire et écrire les valeurs à partir / vers la base de données.

## <a name="conventions"></a>Conventions

Par convention, les propriétés publiques avec un accesseur Get et un accesseur Set figureront dans le modèle.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour exclure une propriété à partir du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour exclure une propriété à partir du modèle.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
