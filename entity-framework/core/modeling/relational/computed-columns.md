---
title: Colonnes calculées-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655913"
---
# <a name="computed-columns"></a>Colonnes calculées

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une colonne calculée est une colonne dont la valeur est calculée dans la base de données. Une colonne calculée peut utiliser d’autres colonnes de la table pour calculer sa valeur.

## <a name="conventions"></a>Conventions

Par Convention, les colonnes calculées ne sont pas créées dans le modèle.

## <a name="data-annotations"></a>Annotations de données

Les colonnes calculées ne peuvent pas être configurées avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier qu’une propriété doit être mappée à une colonne calculée.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
