---
title: Interrogation des données - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181914"
---
# <a name="querying-data"></a><span data-ttu-id="416e0-102">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="416e0-102">Querying Data</span></span>

<span data-ttu-id="416e0-103">Entity Framework Core utilise LINQ (Language Integrated Query) pour interroger les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="416e0-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="416e0-104">LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées.</span><span class="sxs-lookup"><span data-stu-id="416e0-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="416e0-105">Il utilise vos classes de contexte et d’entité dérivées pour référencer les objet de base de données.</span><span class="sxs-lookup"><span data-stu-id="416e0-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="416e0-106">EF Core passe une représentation de la requête LINQ au fournisseur de bases de données.</span><span class="sxs-lookup"><span data-stu-id="416e0-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="416e0-107">Les fournisseurs de bases de données la traduisent à leur tour en langage de requête spécifique aux bases de données (par exemple, SQL pour une base de données relationnelle).</span><span class="sxs-lookup"><span data-stu-id="416e0-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="416e0-108">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="416e0-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="416e0-109">Les extraits de code montrent quelques exemples illustrant comment accomplir des tâches courantes avec Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="416e0-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="416e0-110">Chargement de toutes les données</span><span class="sxs-lookup"><span data-stu-id="416e0-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="416e0-111">Chargement d’une seule entité</span><span class="sxs-lookup"><span data-stu-id="416e0-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="416e0-112">Filtrage</span><span class="sxs-lookup"><span data-stu-id="416e0-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="416e0-113">Pour aller plus loin</span><span class="sxs-lookup"><span data-stu-id="416e0-113">Further readings</span></span>

- <span data-ttu-id="416e0-114">Découvrir les [expressions de requête LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="416e0-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="416e0-115">Pour plus d’informations sur le traitement d’une requête dans EF Core, consultez [Fonctionnement d’une requête](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="416e0-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
