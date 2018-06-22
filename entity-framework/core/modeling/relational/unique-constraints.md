---
title: Clés secondaires (contraintes uniques) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052789"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="53bb2-102">Clés secondaires (contraintes uniques)</span><span class="sxs-lookup"><span data-stu-id="53bb2-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="53bb2-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="53bb2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="53bb2-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="53bb2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="53bb2-105">Une contrainte unique est introduite pour chaque clé secondaire dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="53bb2-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="53bb2-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="53bb2-106">Conventions</span></span>

<span data-ttu-id="53bb2-107">Par convention, l’index et des contraintes qui ont été introduites pour une autre clé seront nommés `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="53bb2-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="53bb2-108">Pour les clés composites autre `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.</span><span class="sxs-lookup"><span data-stu-id="53bb2-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="53bb2-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="53bb2-109">Data Annotations</span></span>

<span data-ttu-id="53bb2-110">Contraintes unique ne peuvent pas être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="53bb2-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="53bb2-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="53bb2-111">Fluent API</span></span>

<span data-ttu-id="53bb2-112">Vous pouvez utiliser l’API Fluent pour configurer le nom de contrainte et d’index pour une autre clé.</span><span class="sxs-lookup"><span data-stu-id="53bb2-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
