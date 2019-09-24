---
title: Mappage de colonnes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197213"
---
# <a name="column-mapping"></a>Mappage de colonnes

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le mappage de colonnes identifie les données de colonne à partir desquelles les données doivent être interrogées et enregistrées dans la base de données.

## <a name="conventions"></a>Conventions

Par Convention, chaque propriété est configurée pour être mappée à une colonne portant le même nom que la propriété.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour configurer la colonne à laquelle une propriété est mappée.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
