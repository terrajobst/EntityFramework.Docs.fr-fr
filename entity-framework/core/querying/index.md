---
title: Interrogation des données - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993532"
---
# <a name="querying-data"></a><span data-ttu-id="b58ba-102">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="b58ba-102">Querying Data</span></span>

<span data-ttu-id="b58ba-103">Entity Framework Core utilise LINQ (Language Integrated Query) pour interroger les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b58ba-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="b58ba-104">LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="b58ba-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="b58ba-105">Une représentation de la requête LINQ est passée au fournisseur de base de données, en vue de sa conversion dans un langage de requête propre à la base de données (par exemple SQL pour une base de données relationnelle).</span><span class="sxs-lookup"><span data-stu-id="b58ba-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="b58ba-106">Pour plus d’informations sur le traitement d’une requête, consultez [Fonctionnement d’une requête](overview.md).</span><span class="sxs-lookup"><span data-stu-id="b58ba-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
