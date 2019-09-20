---
title: Types de données-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149166"
---
# <a name="data-types"></a><span data-ttu-id="91362-102">Types de données</span><span class="sxs-lookup"><span data-stu-id="91362-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="91362-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="91362-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="91362-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="91362-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="91362-105">Le type de données fait référence au type spécifique à la base de données de la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="91362-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="91362-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="91362-106">Conventions</span></span>

<span data-ttu-id="91362-107">Par Convention, le fournisseur de base de données sélectionne un type de données en fonction du type .NET de la propriété.</span><span class="sxs-lookup"><span data-stu-id="91362-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="91362-108">Il prend également en compte d’autres métadonnées, telles que la [longueur maximale](../max-length.md)configurée, si la propriété fait partie d’une clé primaire, etc.</span><span class="sxs-lookup"><span data-stu-id="91362-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="91362-109">Par exemple, SQL Server utilise `datetime2(7)` pour `DateTime` les propriétés et `nvarchar(max)` pour `string` les propriétés ( `nvarchar(450)` ou `string` pour les propriétés utilisées comme clé).</span><span class="sxs-lookup"><span data-stu-id="91362-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="91362-110">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="91362-110">Data Annotations</span></span>

<span data-ttu-id="91362-111">Vous pouvez utiliser des annotations de données pour spécifier un type de données exact pour une colonne.</span><span class="sxs-lookup"><span data-stu-id="91362-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="91362-112">Par exemple, le code suivant configure `Url` en tant que chaîne non-Unicode avec une longueur `200` maximale `Rating` de et en tant que `5` valeur décimale `2`avec la précision et l’échelle de.</span><span class="sxs-lookup"><span data-stu-id="91362-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="91362-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="91362-113">Fluent API</span></span>

<span data-ttu-id="91362-114">Vous pouvez également utiliser l’API Fluent pour spécifier les mêmes types de données pour les colonnes.</span><span class="sxs-lookup"><span data-stu-id="91362-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
