---
title: Propriétés Shadow-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655962"
---
# <a name="shadow-properties"></a>Propriétés cachées

Les propriétés Shadow sont des propriétés qui ne sont pas définies dans votre classe d’entité .NET, mais qui sont définies pour ce type d’entité dans le modèle EF Core. La valeur et l’état de ces propriétés sont gérés uniquement dans le dispositif de suivi des modifications.

Les propriétés Shadow sont utiles lorsque la base de données contient des données qui ne doivent pas être exposées sur les types d’entités mappés. Ils sont le plus souvent utilisés pour les propriétés de clé étrangère, où la relation entre deux entités est représentée par une valeur de clé étrangère dans la base de données, mais la relation est gérée sur les types d’entité à l’aide des propriétés de navigation entre les types d’entités.

Les valeurs de propriété Shadow peuvent être obtenues et modifiées par le biais de l’API `ChangeTracker`.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Les propriétés Shadow peuvent être référencées dans des requêtes LINQ via la méthode statique `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Conventions

Les propriétés Shadow peuvent être créées par convention lorsqu’une relation est détectée, mais qu’aucune propriété de clé étrangère n’est trouvée dans la classe d’entité dépendante. Dans ce cas, une propriété de clé étrangère Shadow est introduite. La propriété de clé étrangère Shadow est nommée `<navigation property name><principal key property name>` (la navigation sur l’entité dépendante, qui pointe vers l’entité principale, est utilisée pour le nommage). Si le nom de la propriété de clé principale comprend le nom de la propriété de navigation, le nom sera simplement `<principal key property name>`. S’il n’existe aucune propriété de navigation sur l’entité dépendante, le nom du type de principal est utilisé à la place.

Par exemple, la liste de code suivante entraînera l’introduction d’une propriété Shadow `BlogId` dans l’entité `Post`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Annotations de données

Les propriétés Shadow ne peuvent pas être créées avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer les propriétés Shadow. Une fois que vous avez appelé la surcharge de chaîne de `Property` vous pouvez chaîner n’importe quel appel de configuration que vous feriez pour d’autres propriétés.

Si le nom fourni à la méthode `Property` correspond au nom d’une propriété existante (une propriété Shadow ou une propriété Shadow définie sur la classe d’entité), le code configurera cette propriété existante au lieu d’introduire une nouvelle propriété Shadow.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
