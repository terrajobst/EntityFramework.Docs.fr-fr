---
title: Index - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993215"
---
# <a name="indexes"></a><span data-ttu-id="353c2-102">Index</span><span class="sxs-lookup"><span data-stu-id="353c2-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="353c2-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="353c2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="353c2-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="353c2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="353c2-105">Un index dans une base de données relationnelle est mappé au même concept en tant qu’index dans le cœur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="353c2-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="353c2-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="353c2-106">Conventions</span></span>

<span data-ttu-id="353c2-107">Par convention, les index sont nommés `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="353c2-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="353c2-108">Pour les index composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.</span><span class="sxs-lookup"><span data-stu-id="353c2-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="353c2-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="353c2-109">Data Annotations</span></span>

<span data-ttu-id="353c2-110">Index ne peuvent pas être configurés à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="353c2-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="353c2-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="353c2-111">Fluent API</span></span>

<span data-ttu-id="353c2-112">Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.</span><span class="sxs-lookup"><span data-stu-id="353c2-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="353c2-113">Vous pouvez également spécifier un filtre.</span><span class="sxs-lookup"><span data-stu-id="353c2-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="353c2-114">Lors de l’utilisation du fournisseur SQL Server EF ajoute un 'IS NOT NULL' filtrer pour toutes les colonnes nullables qui font partie d’un index unique.</span><span class="sxs-lookup"><span data-stu-id="353c2-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="353c2-115">Pour remplacer cette convention, vous pouvez fournir un `null` valeur.</span><span class="sxs-lookup"><span data-stu-id="353c2-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
