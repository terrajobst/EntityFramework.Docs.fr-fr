---
title: Propriétés obligatoires et facultatives-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197841"
---
# <a name="required-and-optional-properties"></a>Propriétés obligatoires et facultatives

Une propriété est considérée comme facultative si elle est valide pour contenir `null`. Si `null` n’est pas une valeur valide à assigner à une propriété, elle est considérée comme étant une propriété obligatoire.

Lors du mappage à un schéma de base de données relationnelle, les propriétés requises sont créées en tant que colonnes n’acceptant pas les valeurs NULL, et les propriétés facultatives sont créées en tant que colonnes Nullable.

## <a name="conventions"></a>Conventions

Par Convention, une propriété dont le type .NET peut contenir une valeur null sera configurée comme étant facultative, alors que les propriétés dont le type .NET ne peut pas contenir de valeur null seront configurées selon les besoins. Par exemple, toutes les propriétés avec des types valeur`int`.NET `decimal`( `bool`,,, etc.) sont configurées en fonction des besoins, et toutes les propriétés avec `decimal?`des `bool?`types valeur .net Nullable (`int?`,,, etc.) sont configuré comme option facultative.

C#8 a introduit une nouvelle fonctionnalité appelée [types de référence Nullable](/dotnet/csharp/tutorials/nullable-reference-types), qui permet d’annoter des types de référence, ce qui indique s’il est valide qu’ils contiennent ou non des valeurs NULL. Cette fonctionnalité est désactivée par défaut et, si elle est activée, elle modifie le comportement de EF Core de la façon suivante :

* Si les types de référence Nullable sont désactivés (valeur par défaut), toutes les propriétés avec des types de référence .NET sont `string`configurées comme étant facultatives par convention (par exemple,).
* Si les types de référence Nullable sont activés, les propriétés seront configurées en fonction de la C# possibilité de `string?` valeur null de leur type .net : `string` sera configuré comme étant facultatif, tandis que sera configuré comme requis.

L’exemple suivant montre un type d’entité avec des propriétés obligatoires et facultatives, avec la fonctionnalité de référence Nullable désactivée (valeur par défaut) et activée :

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Sans types référence Nullable (valeur par défaut)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[Avec les types de référence Nullable](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

L’utilisation de types de référence Nullable est recommandée, car elle transmet la C# possibilité de valeur null exprimée dans le code à EF Core modèle et à la base de données, et évite l’utilisation de l’API Fluent ou des annotations de données pour exprimer deux fois le même concept.

> [!NOTE]
> Soyez prudent lorsque vous activez les types de référence Nullable sur un projet existant : les propriétés de type de référence qui ont été précédemment configurées comme étant facultatives sont maintenant configurées comme obligatoires, sauf si elles sont explicitement annotées comme nullables. Lors de la gestion d’un schéma de base de données relationnelle, des migrations peuvent être générées, ce qui modifie la possibilité de valeur null de la colonne de base de données.

Pour plus d’informations sur les types de référence Nullable et leur utilisation avec EF Core, [consultez la page de documentation dédiée pour cette fonctionnalité](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Configuration

Une propriété qui serait facultative par convention peut être configurée comme suit :

# <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***