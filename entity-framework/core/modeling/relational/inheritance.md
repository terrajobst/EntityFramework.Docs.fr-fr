---
title: Héritage (base de données relationnelle)-EF Core
description: Comment configurer l’héritage de type d’entité dans une base de données relationnelle à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824747"
---
# <a name="inheritance-relational-database"></a>Héritage (base de données relationnelle)

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.

> [!NOTE]  
> Actuellement, seul le modèle TPH (table par hiérarchie) est implémenté dans EF Core. D’autres modèles courants tels que table par type (TPT) et table par type (TPC) ne sont pas encore disponibles.

## <a name="conventions"></a>Conventions

Par défaut, l’héritage est mappé à l’aide du modèle TPH (table par hiérarchie). TPH utilise une table unique pour stocker les données de tous les types dans la hiérarchie. Une colonne de discriminateur est utilisée pour identifier le type représenté par chaque ligne.

EF Core configure l’héritage uniquement si au moins deux types hérités sont explicitement inclus dans le modèle (pour plus d’informations, consultez [héritage](../inheritance.md) ).

Voici un exemple qui illustre un scénario d’héritage simple et les données stockées dans une table de base de données relationnelle à l’aide du modèle TPH. La colonne de *discriminateur* identifie le type de *blog* qui est stocké dans chaque ligne.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Les colonnes de base de données deviennent automatiquement Nullable si nécessaire lors de l’utilisation du mappage TPH.

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs utilisées pour identifier chaque type dans la hiérarchie.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configuration de la propriété de discriminateur

Dans les exemples ci-dessus, le discriminateur est créé en tant que [propriété Shadow](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie. Étant donné qu’il s’agit d’une propriété dans le modèle, elle peut être configurée comme d’autres propriétés. Par exemple, pour définir la longueur maximale lorsque le discriminateur par défaut par convention est utilisé :

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

Le discriminateur peut également être mappé à une propriété .NET dans votre entité et le configurer. Par exemple :

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Colonnes partagées

Lorsque deux types d’entités frères ont une propriété portant le même nom, ils sont mappés à deux colonnes distinctes par défaut. Mais s’ils sont compatibles, ils peuvent être mappés à la même colonne :

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]