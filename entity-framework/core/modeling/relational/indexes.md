---
title: Index (base de données relationnelle)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824576"
---
# <a name="indexes-relational-database"></a>Index (base de données relationnelle)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Un index dans une base de données relationnelle est mappé au même concept qu’un index dans le cœur de Entity Framework.

## <a name="conventions"></a>Conventions

Par Convention, les index sont nommés `IX_<type name>_<property name>`. Pour les index composites `<property name>` devient une liste de noms de propriétés séparés par un trait de soulignement.

## <a name="data-annotations"></a>Annotations de données

Les index ne peuvent pas être configurés à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Vous pouvez également spécifier un filtre.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Lorsque vous utilisez le SQL Server le fournisseur EF ajoute un filtre `'IS NOT NULL'` pour toutes les colonnes Nullable qui font partie d’un index unique. Pour remplacer cette Convention, vous pouvez fournir une valeur `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Inclure les colonnes dans les index SQL Server

Vous pouvez configurer [des index avec des colonnes incluses](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) pour améliorer considérablement les performances des requêtes lorsque toutes les colonnes de la requête sont incluses dans l’index en tant que colonnes clés ou non-clés.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
