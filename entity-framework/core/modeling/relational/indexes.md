---
title: Indexes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678997"
---
# <a name="indexes"></a><span data-ttu-id="354bb-102">Index</span><span class="sxs-lookup"><span data-stu-id="354bb-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="354bb-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="354bb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="354bb-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="354bb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="354bb-105">Un index dans une base de données relationnelle est mappé au même concept en tant qu’index dans le cœur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="354bb-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="354bb-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="354bb-106">Conventions</span></span>

<span data-ttu-id="354bb-107">Par convention, les index sont nommés `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="354bb-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="354bb-108">Index composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.</span><span class="sxs-lookup"><span data-stu-id="354bb-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="354bb-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="354bb-109">Data Annotations</span></span>

<span data-ttu-id="354bb-110">Les index ne peuvent pas être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="354bb-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="354bb-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="354bb-111">Fluent API</span></span>

<span data-ttu-id="354bb-112">Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.</span><span class="sxs-lookup"><span data-stu-id="354bb-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="354bb-113">Vous pouvez également spécifier un filtre.</span><span class="sxs-lookup"><span data-stu-id="354bb-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="354bb-114">Lors de l’utilisation du fournisseur SQL Server EF ajoute un 'IS NOT NULL' filtrer pour toutes les colonnes qui font partie d’un index unique.</span><span class="sxs-lookup"><span data-stu-id="354bb-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="354bb-115">Pour remplacer cette convention, vous pouvez fournir un `null` valeur.</span><span class="sxs-lookup"><span data-stu-id="354bb-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
