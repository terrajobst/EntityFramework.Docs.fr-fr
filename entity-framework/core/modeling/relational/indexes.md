---
title: Index (base de données relationnelle)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813629"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="991d3-102">Index (base de données relationnelle)</span><span class="sxs-lookup"><span data-stu-id="991d3-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="991d3-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="991d3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="991d3-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="991d3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="991d3-105">Un index dans une base de données relationnelle est mappé au même concept qu’un index dans le cœur de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="991d3-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="991d3-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="991d3-106">Conventions</span></span>

<span data-ttu-id="991d3-107">Par Convention, les index sont nommés `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="991d3-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="991d3-108">Pour les index `<property name>` composites devient une liste de noms de propriétés séparés par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="991d3-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="991d3-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="991d3-109">Data Annotations</span></span>

<span data-ttu-id="991d3-110">Les index ne peuvent pas être configurés à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="991d3-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="991d3-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="991d3-111">Fluent API</span></span>

<span data-ttu-id="991d3-112">Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.</span><span class="sxs-lookup"><span data-stu-id="991d3-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="991d3-113">Vous pouvez également spécifier un filtre.</span><span class="sxs-lookup"><span data-stu-id="991d3-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="991d3-114">Lorsque vous utilisez le SQL Server le fournisseur EF ajoute un filtre’IS NOT NULL’pour toutes les colonnes Nullable qui font partie d’un index unique.</span><span class="sxs-lookup"><span data-stu-id="991d3-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="991d3-115">Pour remplacer cette Convention, vous pouvez fournir une `null` valeur.</span><span class="sxs-lookup"><span data-stu-id="991d3-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="991d3-116">Inclure les colonnes dans les index SQL Server</span><span class="sxs-lookup"><span data-stu-id="991d3-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="991d3-117">Vous pouvez configurer [des index avec des colonnes incluses](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) pour améliorer considérablement les performances des requêtes lorsque toutes les colonnes de la requête sont incluses dans l’index en tant que colonnes clés ou non-clés.</span><span class="sxs-lookup"><span data-stu-id="991d3-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
