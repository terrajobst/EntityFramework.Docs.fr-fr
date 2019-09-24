---
title: Types de données-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197181"
---
# <a name="data-types"></a>Types de données

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le type de données fait référence au type spécifique à la base de données de la colonne à laquelle une propriété est mappée.

## <a name="conventions"></a>Conventions

Par Convention, le fournisseur de base de données sélectionne un type de données en fonction du type .NET de la propriété. Il prend également en compte d’autres métadonnées, telles que la [longueur maximale](../max-length.md)configurée, si la propriété fait partie d’une clé primaire, etc.

Par exemple, SQL Server utilise `datetime2(7)` pour `DateTime` les propriétés et `nvarchar(max)` pour `string` les propriétés ( `nvarchar(450)` ou `string` pour les propriétés utilisées comme clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour spécifier un type de données exact pour une colonne.

Par exemple, le code suivant configure `Url` en tant que chaîne non-Unicode avec une longueur `200` maximale `Rating` de et en tant que `5` valeur décimale `2`avec la précision et l’échelle de.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez également utiliser l’API Fluent pour spécifier les mêmes types de données pour les colonnes.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
