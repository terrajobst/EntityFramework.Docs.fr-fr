---
title: Clés secondaires (contraintes uniques) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052789"
---
# <a name="alternate-keys-unique-constraints"></a>Clés secondaires (contraintes uniques)

> [!NOTE]  
> La configuration de cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).

Une contrainte unique est introduite pour chaque clé secondaire dans le modèle.

## <a name="conventions"></a>Conventions

Par convention, l’index et des contraintes qui ont été introduites pour une autre clé seront nommés `AK_<type name>_<property name>`. Pour les clés composites autre `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.

## <a name="data-annotations"></a>Annotations de données

Contraintes unique ne peuvent pas être configurés à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom de contrainte et d’index pour une autre clé.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
