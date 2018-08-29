---
title: Types de données - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993519"
---
# <a name="data-types"></a>Types de données

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Type de données fait référence au type spécifique de base de données de la colonne à laquelle une propriété est mappée.

## <a name="conventions"></a>Conventions

Par convention, le fournisseur de base de données sélectionne un type de données en fonction du type CLR de la propriété. Il prend également en compte d’autres métadonnées, telles que la [longueur maximale](../max-length.md), si la propriété fait partie d’une clé primaire, un etc.

Par exemple, SQL Server utilise `datetime2(7)` pour `DateTime` propriétés, et `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` pour `string` propriétés qui sont utilisées en tant que clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour spécifier un type de données exactes pour une colonne.

Par exemple, le code suivant configure `Url` sous forme de chaîne non-unicode avec une longueur maximale de `200` et `Rating` comme decimal avec une précision de `5` et mettre à l’échelle de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez également utiliser l’API Fluent pour spécifier les mêmes types de données pour les colonnes.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
