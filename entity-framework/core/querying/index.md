---
title: Interrogation des données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="f0a6a-102">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="f0a6a-102">Querying Data</span></span>

<span data-ttu-id="f0a6a-103">Entity Framework Core utilise LINQ (Language Integrate Query) pour interroger les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="f0a6a-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="f0a6a-104">LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="f0a6a-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="f0a6a-105">Une représentation de la requête LINQ est passée au fournisseur de base de données, en vue de sa conversion dans un langage de requête propre à la base de données (par exemple, SQL pour une base de données relationnelle).</span><span class="sxs-lookup"><span data-stu-id="f0a6a-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="f0a6a-106">Pour plus d’informations sur le traitement d’une requête, consultez [Fonctionnement d’une requête](overview.md).</span><span class="sxs-lookup"><span data-stu-id="f0a6a-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
