---
title: Héritage-EF Core
description: Comment configurer l’héritage de type d’entité à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502164"
---
# <a name="inheritance"></a>Héritage

EF peut mapper une hiérarchie de types .NET à une base de données. Cela vous permet d’écrire vos entités .NET dans le code comme d’habitude, à l’aide de types de base et dérivés, et de faire en sorte que EF crée en toute transparence le schéma de base de données approprié, les requêtes d’émission, etc. Les détails réels de la façon dont une hiérarchie de types est mappée dépendent du fournisseur ; Cette page décrit la prise en charge de l’héritage dans le contexte d’une base de données relationnelle.

Pour le moment, EF Core ne prend en charge que le modèle TPH (table par hiérarchie). TPH utilise une table unique pour stocker les données de tous les types dans la hiérarchie, et une colonne de discriminateur est utilisée pour identifier le type représenté par chaque ligne.

> [!NOTE]
> La table par type (TPT) et la table par type (TPC), qui sont prises en charge par EF6, ne sont pas encore prises en charge par EF Core. TPT est une fonctionnalité majeure prévue pour EF Core 5,0.

## <a name="entity-type-hierarchy-mapping"></a>Mappage de la hiérarchie des types d’entités

Par Convention, EF ne configure que l’héritage si au moins deux types hérités sont explicitement inclus dans le modèle. EF ne recherche pas automatiquement les types de base ou dérivés qui ne sont pas inclus dans le modèle.

Vous pouvez inclure des types dans le modèle en exposant un DbSet pour chaque type dans la hiérarchie d’héritage :

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Ce modèle est mappé au schéma de base de données suivant (Notez la colonne de *discriminateur* créée implicitement, qui identifie le type de *blog* stocké dans chaque ligne) :

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Les colonnes de base de données deviennent automatiquement Nullable si nécessaire lors de l’utilisation du mappage TPH. Par exemple, la colonne *RssUrl* accepte les valeurs NULL, car les instances de *blog* ordinaires n’ont pas cette propriété.

Si vous ne souhaitez pas exposer un DbSet pour une ou plusieurs entités dans la hiérarchie, vous pouvez également utiliser l’API Fluent pour vous assurer qu’elles sont incluses dans le modèle.

> [!TIP]
> Si vous ne vous fiez pas aux conventions, vous pouvez spécifier explicitement le type de base à l’aide de `HasBaseType`. Vous pouvez également utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité de la hiérarchie.

## <a name="discriminator-configuration"></a>Configuration de discriminateur

Vous pouvez configurer le nom et le type de la colonne de discriminateur et les valeurs utilisées pour identifier chaque type dans la hiérarchie :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

Dans les exemples ci-dessus, EF a ajouté le discriminateur implicitement en tant que [propriété Shadow](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie. Cette propriété peut être configurée comme n’importe quelle autre :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Enfin, le discriminateur peut également être mappé à une propriété .NET normale dans votre entité :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Colonnes partagées

Par défaut, lorsque deux types d’entités frères dans la hiérarchie ont une propriété portant le même nom, ils sont mappés à deux colonnes distinctes. Toutefois, si leur type est identique, ils peuvent être mappés à la même colonne de base de données :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
