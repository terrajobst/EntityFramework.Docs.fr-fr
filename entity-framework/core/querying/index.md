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
# <a name="querying-data"></a>Interrogation des données

Entity Framework Core utilise LINQ (Language Integrated Query) pour interroger les données de la base de données. LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité. Une représentation de la requête LINQ est passée au fournisseur de base de données, en vue de sa conversion dans un langage de requête propre à la base de données (par exemple SQL pour une base de données relationnelle). Pour plus d’informations sur le traitement d’une requête, consultez [Fonctionnement d’une requête](overview.md).
