---
title: Interrogation des données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949273"
---
# <a name="querying-data"></a><span data-ttu-id="ab700-102">Interrogation des données</span><span class="sxs-lookup"><span data-stu-id="ab700-102">Querying Data</span></span>

<span data-ttu-id="ab700-103">Entity Framework Core utilise LINQ (Language Integrated Query) pour interroger les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="ab700-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="ab700-104">LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité.</span><span class="sxs-lookup"><span data-stu-id="ab700-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="ab700-105">Une représentation de la requête LINQ est passée au fournisseur de base de données, en vue de sa conversion dans un langage de requête propre à la base de données (par exemple SQL pour une base de données relationnelle).</span><span class="sxs-lookup"><span data-stu-id="ab700-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="ab700-106">Pour plus d’informations sur le traitement d’une requête, consultez [Fonctionnement d’une requête](overview.md).</span><span class="sxs-lookup"><span data-stu-id="ab700-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
