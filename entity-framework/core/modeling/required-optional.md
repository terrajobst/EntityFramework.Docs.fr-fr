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
# <a name="required-and-optional-properties"></a><span data-ttu-id="38817-102">Propriétés obligatoires et facultatives</span><span class="sxs-lookup"><span data-stu-id="38817-102">Required and Optional Properties</span></span>

<span data-ttu-id="38817-103">Une propriété est considérée comme facultative si elle est valide pour contenir `null`.</span><span class="sxs-lookup"><span data-stu-id="38817-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="38817-104">Si `null` n’est pas une valeur valide à assigner à une propriété, elle est considérée comme étant une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="38817-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="38817-105">Conventions</span><span class="sxs-lookup"><span data-stu-id="38817-105">Conventions</span></span>

<span data-ttu-id="38817-106">Par Convention, une propriété dont le type .net peut contenir une valeur null sera configurée`string`comme `int?`étant `byte[]`facultative (,,, etc.).</span><span class="sxs-lookup"><span data-stu-id="38817-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="38817-107">Les propriétés dont le type CLR ne peut pas contenir de valeur null`int`seront `decimal`configurées comme obligatoires (,, `bool`, etc.).</span><span class="sxs-lookup"><span data-stu-id="38817-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="38817-108">Une propriété dont le type .NET ne peut pas contenir de valeur NULL ne peut pas être configurée comme optional.</span><span class="sxs-lookup"><span data-stu-id="38817-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="38817-109">La propriété sera toujours considérée comme requise par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="38817-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="38817-110">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="38817-110">Data Annotations</span></span>

<span data-ttu-id="38817-111">Vous pouvez utiliser des annotations de données pour indiquer qu’une propriété est requise.</span><span class="sxs-lookup"><span data-stu-id="38817-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="38817-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="38817-112">Fluent API</span></span>

<span data-ttu-id="38817-113">Vous pouvez utiliser l’API Fluent pour indiquer qu’une propriété est requise.</span><span class="sxs-lookup"><span data-stu-id="38817-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

