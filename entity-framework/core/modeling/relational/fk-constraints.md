---
title: Contraintes de clé étrangère-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655995"
---
# <a name="foreign-key-constraints"></a>Contraintes de clé étrangère

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une contrainte de clé étrangère est introduite pour chaque relation dans le modèle.

## <a name="conventions"></a>Conventions

Par Convention, les contraintes de clé étrangère sont nommées `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Pour les clés étrangères composites `<foreign key property name>` devient une liste de noms de propriétés de clé étrangère séparés par un trait de soulignement.

## <a name="data-annotations"></a>Annotations de données

Les noms de contrainte de clé étrangère ne peuvent pas être configurés à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé étrangère pour une relation.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
