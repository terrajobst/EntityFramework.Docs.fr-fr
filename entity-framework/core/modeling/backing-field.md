---
title: Champs de stockage-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124377"
---
# <a name="backing-fields"></a>Champs de stockage

Les champs de stockage permettent à EF de lire et/ou d’écrire dans un champ plutôt que sur une propriété. Cela peut être utile lorsque l’encapsulation dans la classe est utilisée pour limiter l’utilisation de et/ou améliorer la sémantique de l’accès aux données par code d’application, mais la valeur doit être lue à partir de la base de données et/ou écrite sans utiliser ces restrictions/améliorations.

## <a name="basic-configuration"></a>Configuration de base

Par Convention, les champs suivants sont découverts en tant que champs de stockage pour une propriété donnée (liste dans l’ordre de priorité). 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

Dans l’exemple suivant, la propriété `Url` est configurée pour avoir `_url` comme champ de stockage :

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Notez que les champs de stockage sont uniquement détectés pour les propriétés incluses dans le modèle. Pour plus d’informations sur les propriétés incluses dans le modèle, consultez [inclusion de & exclusion de propriétés](included-properties.md).

Vous pouvez également configurer des champs de stockage de manière explicite, par exemple, si le nom du champ ne correspond pas aux conventions ci-dessus :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a>Accès aux champs et aux propriétés

Par défaut, EF lit et écrit toujours dans le champ de stockage (en supposant que l’un d’eux a été correctement configuré et n’utilise jamais la propriété). Toutefois, EF prend également en charge d’autres modèles d’accès. Par exemple, l’exemple suivant indique à EF d’écrire dans le champ de stockage uniquement lors de la matérialisation, et d’utiliser la propriété dans tous les autres cas :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Consultez l' [énumération PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) pour obtenir l’ensemble complet des options prises en charge.

> [!NOTE]
> Avec EF Core 3,0, le mode d’accès à la propriété par défaut est passé de `PreferFieldDuringConstruction` à `PreferField`.

## <a name="field-only-properties"></a>Propriétés de champ uniquement

Vous pouvez également créer une propriété conceptuelle dans votre modèle qui n’a pas de propriété CLR correspondante dans la classe d’entité, mais utilise à la place un champ pour stocker les données dans l’entité. Cela diffère des [Propriétés Shadow](shadow-properties.md), où les données sont stockées dans le dispositif de suivi des modifications, plutôt que dans le type CLR de l’entité. Les propriétés de champ uniquement sont couramment utilisées lorsque la classe d’entité utilise des méthodes à la place de propriétés pour récupérer/définir des valeurs, ou dans les cas où les champs ne doivent pas être exposés du tout dans le modèle de domaine (par exemple, les clés primaires).

Vous pouvez configurer une propriété de champ uniquement en fournissant un nom dans l’API `Property(...)` :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

EF tente de trouver une propriété CLR portant le nom donné, ou un champ si aucune propriété n’est trouvée. Si aucune propriété ni aucun champ n’est trouvé, une propriété Shadow est configurée à la place.

Vous devrez peut-être faire référence à une propriété de champ uniquement à partir de requêtes LINQ, mais ces champs sont généralement privés. Vous pouvez utiliser la méthode `EF.Property(...)` dans une requête LINQ pour faire référence au champ :

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
