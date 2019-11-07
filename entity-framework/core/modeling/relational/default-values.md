---
title: Valeurs par défaut-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655903"
---
# <a name="default-values"></a>Valeurs par défaut

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

La valeur par défaut d’une colonne est la valeur qui sera insérée si une nouvelle ligne est insérée, mais qu’aucune valeur n’est spécifiée pour la colonne.

## <a name="conventions"></a>Conventions

Par Convention, une valeur par défaut n’est pas configurée.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas définir une valeur par défaut à l’aide des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier la valeur par défaut d’une propriété.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Vous pouvez également spécifier un fragment SQL utilisé pour calculer la valeur par défaut.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
