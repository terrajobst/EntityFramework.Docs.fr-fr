---
title: Schéma par défaut-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655966"
---
# <a name="default-schema"></a>Schéma par défaut

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le schéma par défaut est le schéma de base de données dans lequel les objets seront créés si un schéma n’est pas explicitement configuré pour cet objet.

## <a name="conventions"></a>Conventions

Par Convention, le fournisseur de base de données choisit le schéma par défaut le plus approprié. Par exemple, Microsoft SQL Server utilise le schéma `dbo` et SQLite n’utilise pas de schéma (puisque les schémas ne sont pas pris en charge dans SQLite).

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas définir le schéma par défaut à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
