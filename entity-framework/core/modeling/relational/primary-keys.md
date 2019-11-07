---
title: Clés primaires-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656051"
---
# <a name="primary-keys"></a>Clés primaires

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Une contrainte de clé primaire est introduite pour la clé de chaque type d’entité.

## <a name="conventions"></a>Conventions

Par Convention, la clé primaire de la base de données sera nommée `PK_<type name>`.

## <a name="data-annotations"></a>Annotations de données

Aucun aspect spécifique à la base de données relationnelle d’une clé primaire ne peut être configuré à l’aide des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé primaire dans la base de données.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
