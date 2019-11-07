---
title: Séquences-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656112"
---
# <a name="sequences"></a>Séquences

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une séquence génère des valeurs numériques séquentielles dans la base de données. Les séquences ne sont pas associées à une table spécifique.

## <a name="conventions"></a>Conventions

Par Convention, les séquences ne sont pas introduites dans le modèle.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas configurer une séquence à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour créer une séquence dans le modèle.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Vous pouvez également configurer un aspect supplémentaire de la séquence, par exemple son schéma, sa valeur de départ et son incrément.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Une fois qu’une séquence est introduite, vous pouvez l’utiliser pour générer des valeurs pour les propriétés de votre modèle. Par exemple, vous pouvez utiliser les [valeurs par défaut](default-values.md) pour insérer la valeur suivante à partir de la séquence.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
