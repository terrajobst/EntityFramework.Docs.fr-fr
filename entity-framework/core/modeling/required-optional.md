---
title: Propriétés obligatoires/facultatives-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149154"
---
# <a name="required-and-optional-properties"></a>Propriétés obligatoires et facultatives

Une propriété est considérée comme facultative si elle est valide pour contenir `null`. Si `null` n’est pas une valeur valide à assigner à une propriété, elle est considérée comme étant une propriété obligatoire.

## <a name="conventions"></a>Conventions

Par Convention, une propriété dont le type .net peut contenir une valeur null sera configurée`string`comme `int?`étant `byte[]`facultative (,,, etc.). Les propriétés dont le type CLR ne peut pas contenir de valeur null`int`seront `decimal`configurées comme obligatoires (,, `bool`, etc.).

> [!NOTE]  
> Une propriété dont le type .NET ne peut pas contenir de valeur NULL ne peut pas être configurée comme optional. La propriété sera toujours considérée comme requise par Entity Framework.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour indiquer qu’une propriété est requise.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour indiquer qu’une propriété est requise.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

