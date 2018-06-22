---
title: Sauvegarde des champs - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053459"
---
# <a name="backing-fields"></a>Champs de stockage

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 1.1.

Champs de stockage permettent EF lire et/ou écrire à un champ au lieu d’une propriété. Cela peut être utile lors de l’encapsulation de la classe est utilisée pour limiter l’utilisation d’et/ou d’améliorer la sémantique autour de l’accès aux données par code d’application, mais la valeur doit être lus ou écrite à la base de données sans l’aide de ces restrictions / améliorations.

## <a name="conventions"></a>Conventions

Par convention, les champs suivants seront détectés en tant que la sauvegarde des champs pour une propriété donnée (répertoriées dans l’ordre de priorité). Les champs sont détectés uniquement pour les propriétés qui sont incluses dans le modèle. Pour plus d’informations sur lequel les propriétés sont incluses dans le modèle, consultez [notamment & exclusion des propriétés](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Lorsqu’un champ de stockage est configuré, EF écrit directement dans ce champ lors de la matérialisation des instances d’entité à partir de la base de données (plutôt qu’à l’aide de l’accesseur Set de propriété). Si EF doit lire ou écrire la valeur à d’autres moments, il utilise la propriété si possible. Par exemple, si EF doit mettre à jour la valeur d’une propriété, il utilise la méthode setter de propriété si celle-ci est définie. Si la propriété est en lecture seule, puis elle écrit dans le champ.

## <a name="data-annotations"></a>Annotations de données

Champs de stockage ne peut pas être configuré avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer un champ de stockage pour une propriété.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Contrôle lorsque le champ est utilisé.

Vous pouvez configurer quand EF utilise le champ ou la propriété. Consultez le [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) pour les options prises en charge.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Champs sans une propriété

Vous pouvez également créer une propriété conceptuelle dans votre modèle qui n’a pas d’une propriété CLR correspondante dans la classe d’entité, mais utilise à la place d’un champ pour stocker les données dans l’entité. Cela est différent de [occulter les propriétés](shadow-properties.md), où les données sont stockées dans le traceur de modifications. Cela doit généralement être utilisé si la classe d’entité utilise des méthodes pour obtenir/définir des valeurs.

Vous pouvez donner à EF le nom du champ dans le `Property(...)` API. S’il n’existe aucune propriété portant le nom donné, puis EF recherchera un champ.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Vous pouvez également attribuer la propriété à un nom, autre que le nom du champ. Ce nom est ensuite utilisé lors de la création du modèle, notamment il sera utilisé pour le nom de colonne qui est mappé à la base de données.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Lorsqu’il n’y a aucune propriété dans la classe d’entité, vous pouvez utiliser la `EF.Property(...)` méthode dans une requête LINQ pour faire référence à la propriété qui fait partie du modèle.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
