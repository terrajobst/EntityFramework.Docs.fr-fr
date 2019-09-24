---
title: Inclusion de & exclusion de propriétés-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197413"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="f6159-102">Inclusion & exclusion de propriétés</span><span class="sxs-lookup"><span data-stu-id="f6159-102">Including & Excluding Properties</span></span>

<span data-ttu-id="f6159-103">L’inclusion d’une propriété dans le modèle signifie qu’EF a des métadonnées sur cette propriété et tente de lire et d’écrire des valeurs à partir de/dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="f6159-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f6159-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="f6159-104">Conventions</span></span>

<span data-ttu-id="f6159-105">Par Convention, les propriétés publiques avec un accesseur get et un accesseur Set seront incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="f6159-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f6159-106">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="f6159-106">Data Annotations</span></span>

<span data-ttu-id="f6159-107">Vous pouvez utiliser des annotations de données pour exclure une propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="f6159-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="f6159-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="f6159-108">Fluent API</span></span>

<span data-ttu-id="f6159-109">Vous pouvez utiliser l’API Fluent pour exclure une propriété du modèle.</span><span class="sxs-lookup"><span data-stu-id="f6159-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
