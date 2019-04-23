---
title: Mappage de colonnes - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929860"
---
# <a name="column-mapping"></a>Mappage de colonnes

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Mappage de colonne identifie les données de la colonne doivent être interrogées à partir d’et enregistrées dans la base de données.

## <a name="conventions"></a>Conventions

Par convention, chaque propriété sera être configurée pour mapper à une colonne portant le même nom que la propriété.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer la colonne à laquelle une propriété est mappée.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
