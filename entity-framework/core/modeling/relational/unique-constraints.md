---
title: Clés secondaires (contraintes uniques) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994189"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="cd5c9-102">Clés secondaires (contraintes uniques)</span><span class="sxs-lookup"><span data-stu-id="cd5c9-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="cd5c9-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cd5c9-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="cd5c9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cd5c9-105">Une contrainte unique est introduite pour chaque clé secondaire dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="cd5c9-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="cd5c9-106">Conventions</span></span>

<span data-ttu-id="cd5c9-107">Par convention, l’index et des contraintes qui sont introduites pour une autre clé seront nommés `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="cd5c9-108">Pour les clés de substitution composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cd5c9-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="cd5c9-109">Data Annotations</span></span>

<span data-ttu-id="cd5c9-110">Contraintes unique ne peuvent pas être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cd5c9-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="cd5c9-111">Fluent API</span></span>

<span data-ttu-id="cd5c9-112">Vous pouvez utiliser l’API Fluent pour configurer le nom de contrainte et d’index pour une autre clé.</span><span class="sxs-lookup"><span data-stu-id="cd5c9-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
