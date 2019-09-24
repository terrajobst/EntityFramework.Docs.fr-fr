---
title: Longueur maximale-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197226"
---
# <a name="maximum-length"></a><span data-ttu-id="c4089-102">Longueur maximale</span><span class="sxs-lookup"><span data-stu-id="c4089-102">Maximum Length</span></span>

<span data-ttu-id="c4089-103">La configuration d’une longueur maximale fournit une indication au magasin de données sur le type de données approprié à utiliser pour une propriété donnée.</span><span class="sxs-lookup"><span data-stu-id="c4089-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="c4089-104">La longueur maximale s’applique uniquement aux types de données de `string` tableau `byte[]`, tels que et.</span><span class="sxs-lookup"><span data-stu-id="c4089-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="c4089-105">Entity Framework n’effectue aucune validation de longueur maximale avant de transmettre des données au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="c4089-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="c4089-106">Il revient au fournisseur ou au magasin de données de valider le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="c4089-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="c4089-107">Par exemple, lorsque vous ciblez SQL Server, le dépassement de la longueur maximale entraîne une exception, car le type de données de la colonne sous-jacente n’autorise pas le stockage des données excédentaires.</span><span class="sxs-lookup"><span data-stu-id="c4089-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="c4089-108">Conventions</span><span class="sxs-lookup"><span data-stu-id="c4089-108">Conventions</span></span>

<span data-ttu-id="c4089-109">Par Convention, il est laissé au fournisseur de base de données de choisir un type de données approprié pour les propriétés.</span><span class="sxs-lookup"><span data-stu-id="c4089-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="c4089-110">Pour les propriétés qui ont une longueur, le fournisseur de base de données choisit généralement un type de données qui autorise la longueur de données la plus longue.</span><span class="sxs-lookup"><span data-stu-id="c4089-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="c4089-111">Par exemple, Microsoft SQL Server utilise `nvarchar(max)` pour `string` les propriétés (ou `nvarchar(450)` si la colonne est utilisée comme clé).</span><span class="sxs-lookup"><span data-stu-id="c4089-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c4089-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="c4089-112">Data Annotations</span></span>

<span data-ttu-id="c4089-113">Vous pouvez utiliser les annotations de données pour configurer une longueur maximale pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="c4089-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="c4089-114">Dans cet exemple, le ciblage SQL Server cela entraînerait l' `nvarchar(500)` utilisation du type de données.</span><span class="sxs-lookup"><span data-stu-id="c4089-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="c4089-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c4089-115">Fluent API</span></span>

<span data-ttu-id="c4089-116">Vous pouvez utiliser l’API Fluent pour configurer une longueur maximale pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="c4089-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="c4089-117">Dans cet exemple, le ciblage SQL Server cela entraînerait l' `nvarchar(500)` utilisation du type de données.</span><span class="sxs-lookup"><span data-stu-id="c4089-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
