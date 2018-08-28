---
title: Clés secondaires (contraintes uniques) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994189"
---
# <a name="alternate-keys-unique-constraints"></a>Clés secondaires (contraintes uniques)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une contrainte unique est introduite pour chaque clé secondaire dans le modèle.

## <a name="conventions"></a>Conventions

Par convention, l’index et des contraintes qui sont introduites pour une autre clé seront nommés `AK_<type name>_<property name>`. Pour les clés de substitution composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.

## <a name="data-annotations"></a>Annotations de données

Contraintes unique ne peuvent pas être configurés à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom de contrainte et d’index pour une autre clé.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
