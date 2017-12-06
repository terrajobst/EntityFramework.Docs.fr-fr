---
title: "Types de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a>Types de données

> [!NOTE]  
> La configuration de cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).

Type de données fait référence au type de base de données de la colonne à laquelle une propriété est mappée.

## <a name="conventions"></a>Conventions

Par convention, le fournisseur de base de données sélectionne un type de données en fonction du type CLR de la propriété. Il prend également en compte d’autres métadonnées, tel que configuré [longueur maximale](../max-length.md), si la propriété fait partie de clé primaire, etc.

Par exemple, SQL Server utilise `datetime2(7)` pour `DateTime` propriétés, et `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` pour `string` des propriétés qui sont utilisées en tant que clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour spécifier le type de données exact pour une colonne.

Par exemple, le code suivant configure `Url` sous forme de chaîne non-unicode avec une longueur maximale de `200` et `Rating` comme decimal avec une précision de `5` et mettre à l’échelle de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez également utiliser l’API Fluent pour spécifier les mêmes types de données pour les colonnes.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
