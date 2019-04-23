---
title: Inclusion et exclusion de propriétés - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929821"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="76865-102">Inclusion et exclusion de propriétés</span><span class="sxs-lookup"><span data-stu-id="76865-102">Including & Excluding Properties</span></span>

<span data-ttu-id="76865-103">Y compris une propriété dans le modèle signifie qu’EF comporte des métadonnées sur cette propriété et va tenter de lire et écrire les valeurs à partir / vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="76865-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="76865-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="76865-104">Conventions</span></span>

<span data-ttu-id="76865-105">Par convention, les propriétés publiques avec un accesseur Get et un accesseur Set figureront dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="76865-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="76865-106">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="76865-106">Data Annotations</span></span>

<span data-ttu-id="76865-107">Vous pouvez utiliser des Annotations de données pour exclure une propriété à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="76865-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="76865-108">API Fluent</span><span class="sxs-lookup"><span data-stu-id="76865-108">Fluent API</span></span>

<span data-ttu-id="76865-109">Vous pouvez utiliser l’API Fluent pour exclure une propriété à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="76865-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
