---
title: Champs de stockage-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197482"
---
# <a name="backing-fields"></a>Champs de stockage

> [!NOTE]  
> Cette fonctionnalité est une nouveauté de EF Core 1,1.

Les champs de stockage permettent à EF de lire et/ou d’écrire dans un champ plutôt que sur une propriété. Cela peut être utile lorsque l’encapsulation dans la classe est utilisée pour limiter l’utilisation de et/ou améliorer la sémantique de l’accès aux données par le code d’application, mais la valeur doit être lue et/ou écrite dans la base de données sans utiliser ces restrictions/ améliorations.

## <a name="conventions"></a>Conventions

Par Convention, les champs suivants sont découverts en tant que champs de stockage pour une propriété donnée (liste dans l’ordre de priorité). Les champs sont uniquement détectés pour les propriétés incluses dans le modèle. Pour plus d’informations sur les propriétés incluses dans le modèle, consultez [inclusion de & exclusion de propriétés](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Lorsqu’un champ de stockage est configuré, EF écrit directement dans ce champ lors de la matérialisation des instances d’entité de la base de données (au lieu d’utiliser l’accesseur Set de propriété). Si EF doit lire ou écrire la valeur à d’autres moments, il utilisera la propriété si possible. Par exemple, si EF doit mettre à jour la valeur d’une propriété, il utilisera la méthode setter de propriété si celle-ci est définie. Si la propriété est en lecture seule, elle écrit dans le champ.

## <a name="data-annotations"></a>Annotations de données

Les champs de stockage ne peuvent pas être configurés avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer un champ de stockage pour une propriété.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Contrôle de l’utilisation du champ

Vous pouvez configurer le moment où EF utilise le champ ou la propriété. Consultez l' [énumération PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) pour connaître les options prises en charge.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Champs sans propriété

Vous pouvez également créer une propriété conceptuelle dans votre modèle qui n’a pas de propriété CLR correspondante dans la classe d’entité, mais utilise à la place un champ pour stocker les données dans l’entité. Cela diffère des [Propriétés Shadow](shadow-properties.md), où les données sont stockées dans le dispositif de suivi des modifications. Cela est généralement utilisé si la classe d’entité utilise des méthodes pour récupérer/définir des valeurs.

Vous pouvez indiquer à EF le nom du champ dans l' `Property(...)` API. S’il n’existe aucune propriété avec le nom donné, EF recherche un champ.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Vous pouvez également choisir d’attribuer un nom à la propriété, autre que le nom du champ. Ce nom est ensuite utilisé lors de la création du modèle, le plus particulièrement, il sera utilisé pour le nom de colonne qui est mappé à dans la base de données.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Lorsqu’il n’y a aucune propriété dans la classe d’entité, vous `EF.Property(...)` pouvez utiliser la méthode dans une requête LINQ pour faire référence à la propriété qui fait partie conceptuellement du modèle.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
