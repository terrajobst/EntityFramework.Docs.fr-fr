---
title: Clés secondaires (contraintes uniques)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197618"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="09374-102">Autres clés (contraintes uniques)</span><span class="sxs-lookup"><span data-stu-id="09374-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="09374-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="09374-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="09374-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="09374-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="09374-105">Une contrainte unique est introduite pour chaque clé de remplacement dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="09374-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="09374-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="09374-106">Conventions</span></span>

<span data-ttu-id="09374-107">Par Convention, l’index et la contrainte introduits pour une clé secondaire sont nommés `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="09374-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="09374-108">Pour les clés `<property name>` de remplacement composites devient une liste de noms de propriétés séparés par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="09374-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="09374-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="09374-109">Data Annotations</span></span>

<span data-ttu-id="09374-110">Les contraintes unique ne peuvent pas être configurées à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="09374-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="09374-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="09374-111">Fluent API</span></span>

<span data-ttu-id="09374-112">Vous pouvez utiliser l’API Fluent pour configurer l’index et le nom de la contrainte pour une clé secondaire.</span><span class="sxs-lookup"><span data-stu-id="09374-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
