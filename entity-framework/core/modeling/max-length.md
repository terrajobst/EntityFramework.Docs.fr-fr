---
title: Longueur maximale - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929847"
---
# <a name="maximum-length"></a><span data-ttu-id="3a88e-102">Longueur maximale</span><span class="sxs-lookup"><span data-stu-id="3a88e-102">Maximum Length</span></span>

<span data-ttu-id="3a88e-103">Configuration d’une longueur maximale de fournit une indication au magasin de données sur le type de données approprié à utiliser pour une propriété donnée.</span><span class="sxs-lookup"><span data-stu-id="3a88e-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="3a88e-104">Longueur maximale s’applique uniquement aux types de données de tableau, tel que `string` et `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="3a88e-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="3a88e-105">Entity Framework n’effectue aucune validation de longueur maximale avant de transmettre des données au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="3a88e-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="3a88e-106">C’est à la banque de fournisseur ou de données pour valider le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="3a88e-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="3a88e-107">Par exemple, lorsque SQL Server, qui dépasse la longueur maximale de ciblage entraîne une exception comme type de données de la colonne sous-jacente n’autorise pas les données excédentaires à stocker.</span><span class="sxs-lookup"><span data-stu-id="3a88e-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="3a88e-108">Conventions</span><span class="sxs-lookup"><span data-stu-id="3a88e-108">Conventions</span></span>

<span data-ttu-id="3a88e-109">Par convention, il est de laisser le fournisseur de base de données peut choisir un type de données approprié pour les propriétés.</span><span class="sxs-lookup"><span data-stu-id="3a88e-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="3a88e-110">Pour les propriétés qui ont une longueur, le fournisseur de base de données choisit généralement un type de données qui autorise la longueur la plus longue des données.</span><span class="sxs-lookup"><span data-stu-id="3a88e-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="3a88e-111">Par exemple, Microsoft SQL Server utilisera `nvarchar(max)` pour `string` propriétés (ou `nvarchar(450)` si la colonne est utilisée en tant que clé).</span><span class="sxs-lookup"><span data-stu-id="3a88e-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3a88e-112">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="3a88e-112">Data Annotations</span></span>

<span data-ttu-id="3a88e-113">Vous pouvez utiliser les Annotations de données pour configurer une longueur maximale pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="3a88e-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="3a88e-114">Dans cet exemple, ciblant SQL Server cela entraînerait la `nvarchar(500)` type de données utilisé.</span><span class="sxs-lookup"><span data-stu-id="3a88e-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="3a88e-115">API Fluent</span><span class="sxs-lookup"><span data-stu-id="3a88e-115">Fluent API</span></span>

<span data-ttu-id="3a88e-116">Vous pouvez utiliser l’API Fluent pour configurer une longueur maximale pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="3a88e-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="3a88e-117">Dans cet exemple, ciblant SQL Server cela entraînerait la `nvarchar(500)` type de données utilisé.</span><span class="sxs-lookup"><span data-stu-id="3a88e-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
