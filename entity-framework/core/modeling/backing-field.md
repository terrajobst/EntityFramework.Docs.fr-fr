---
title: Sauvegarde des champs - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994091"
---
# <a name="backing-fields"></a>Champs de stockage

> [!NOTE]  
> Cette fonctionnalité est une nouveauté dans EF Core 1.1.

Champs de stockage permettent d’EF lire et/ou écrire sur un champ au lieu d’une propriété. Cela peut être utile lors de l’encapsulation de la classe est utilisée pour restreindre l’utilisation d’et/ou améliorer la sémantique concernant l’accès aux données par code d’application, mais la valeur doit être lus ou écrite à la base de données sans utiliser de ces restrictions / améliorations.

## <a name="conventions"></a>Conventions

Par convention, les champs suivants seront détectés en tant que champs pour une propriété donnée (répertoriés par ordre de priorité) de stockage. Les champs sont détectés uniquement pour les propriétés qui sont incluses dans le modèle. Pour plus d’informations sur lequel les propriétés sont incluses dans le modèle, consultez [inclusion et exclusion de propriétés](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Lorsqu’un champ de stockage est configuré, EF écrira directement à ce champ lors de la matérialisation des instances d’entité à partir de la base de données (plutôt qu’à l’aide de l’accesseur Set de propriété). Si Entity Framework a besoin lire ou écrire la valeur à d’autres moments, il utilisera la propriété si possible. Par exemple, si EF doit mettre à jour la valeur d’une propriété, il utilisera la méthode setter de propriété si celle-ci est définie. Si la propriété est en lecture seule, il écrira dans le champ.

## <a name="data-annotations"></a>Annotations de données

Champs de stockage ne peut pas être configuré avec des annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer un champ de stockage pour une propriété.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Contrôler quand le champ est utilisé.

Vous pouvez configurer quand EF utilise le champ ou la propriété. Consultez le [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) pour les options prises en charge.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Champs sans une propriété

Vous pouvez également créer une propriété conceptuelle dans votre modèle qui n’a pas d’une propriété CLR correspondante dans la classe d’entité, mais utilise à la place d’un champ pour stocker les données dans l’entité. Cela est différent de [occulter les propriétés](shadow-properties.md), où les données sont stockées dans le traceur de modifications. Cela doit généralement être utilisé si la classe d’entité utilise des méthodes pour obtenir/définir des valeurs.

Vous pouvez donner à EF le nom du champ dans le `Property(...)` API. S’il n’existe aucune propriété portant le nom spécifié, puis EF recherchera un champ.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Vous pouvez également choisir d’attribuer la propriété un nom, autre que le nom du champ. Ce nom est ensuite utilisé lors de la création du modèle, plus particulièrement il sera utilisé pour le nom de colonne est mappé à la base de données.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Quand il n’existe aucune propriété dans la classe d’entité, vous pouvez utiliser la `EF.Property(...)` méthode dans une requête LINQ pour faire référence à la propriété qui fait partie du modèle.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
