---
title: Clés secondaires (contraintes uniques)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197618"
---
# <a name="alternate-keys-unique-constraints"></a>Autres clés (contraintes uniques)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une contrainte unique est introduite pour chaque clé de remplacement dans le modèle.

## <a name="conventions"></a>Conventions

Par Convention, l’index et la contrainte introduits pour une clé secondaire sont nommés `AK_<type name>_<property name>`. Pour les clés `<property name>` de remplacement composites devient une liste de noms de propriétés séparés par un trait de soulignement.

## <a name="data-annotations"></a>Annotations de données

Les contraintes unique ne peuvent pas être configurées à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer l’index et le nom de la contrainte pour une clé secondaire.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
