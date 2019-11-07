---
title: Inclusion de & à l’exception des types-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655737"
---
# <a name="including--excluding-types"></a><span data-ttu-id="f592d-102">Inclusion et exclusion de types</span><span class="sxs-lookup"><span data-stu-id="f592d-102">Including & Excluding Types</span></span>

<span data-ttu-id="f592d-103">L’inclusion d’un type dans le modèle signifie qu’EF a des métadonnées sur ce type et tente de lire et d’écrire des instances à partir de/vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="f592d-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f592d-104">Conventions</span><span class="sxs-lookup"><span data-stu-id="f592d-104">Conventions</span></span>

<span data-ttu-id="f592d-105">Par Convention, les types qui sont exposés dans `DbSet` propriétés de votre contexte sont inclus dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="f592d-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="f592d-106">En outre, les types qui sont mentionnés dans la méthode `OnModelCreating` sont également inclus.</span><span class="sxs-lookup"><span data-stu-id="f592d-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="f592d-107">Enfin, tous les types qui sont trouvés en explorant de manière récursive les propriétés de navigation des types découverts sont également inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="f592d-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="f592d-108">**Par exemple, dans le code suivant, les trois types sont découverts :**</span><span class="sxs-lookup"><span data-stu-id="f592d-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="f592d-109">`Blog`, car il est exposé dans une propriété `DbSet` sur le contexte</span><span class="sxs-lookup"><span data-stu-id="f592d-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="f592d-110">`Post`, car il est découvert via la propriété de navigation `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="f592d-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="f592d-111">`AuditEntry`, car il est mentionné dans `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="f592d-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="f592d-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="f592d-112">Data Annotations</span></span>

<span data-ttu-id="f592d-113">Vous pouvez utiliser des annotations de données pour exclure un type du modèle.</span><span class="sxs-lookup"><span data-stu-id="f592d-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="f592d-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="f592d-114">Fluent API</span></span>

<span data-ttu-id="f592d-115">Vous pouvez utiliser l’API Fluent pour exclure un type du modèle.</span><span class="sxs-lookup"><span data-stu-id="f592d-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
