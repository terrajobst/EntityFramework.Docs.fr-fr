---
title: "Types de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a><span data-ttu-id="2e908-102">Types de données</span><span class="sxs-lookup"><span data-stu-id="2e908-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="2e908-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="2e908-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2e908-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="2e908-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2e908-105">Type de données fait référence au type de base de données de la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="2e908-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="2e908-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="2e908-106">Conventions</span></span>

<span data-ttu-id="2e908-107">Par convention, le fournisseur de base de données sélectionne un type de données en fonction du type CLR de la propriété.</span><span class="sxs-lookup"><span data-stu-id="2e908-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="2e908-108">Il prend également en compte d’autres métadonnées, tel que configuré [longueur maximale](../max-length.md), si la propriété fait partie de clé primaire, etc.</span><span class="sxs-lookup"><span data-stu-id="2e908-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="2e908-109">Par exemple, SQL Server utilise `datetime2(7)` pour `DateTime` propriétés, et `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` pour `string` des propriétés qui sont utilisées en tant que clé).</span><span class="sxs-lookup"><span data-stu-id="2e908-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2e908-110">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="2e908-110">Data Annotations</span></span>

<span data-ttu-id="2e908-111">Vous pouvez utiliser des Annotations de données pour spécifier le type de données exact pour une colonne.</span><span class="sxs-lookup"><span data-stu-id="2e908-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="2e908-112">Par exemple, le code suivant configure `Url` sous forme de chaîne non-unicode avec une longueur maximale de `200` et `Rating` comme decimal avec une précision de `5` et mettre à l’échelle de `2`.</span><span class="sxs-lookup"><span data-stu-id="2e908-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="2e908-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="2e908-113">Fluent API</span></span>

<span data-ttu-id="2e908-114">Vous pouvez également utiliser l’API Fluent pour spécifier les mêmes types de données pour les colonnes.</span><span class="sxs-lookup"><span data-stu-id="2e908-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
