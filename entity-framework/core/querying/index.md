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
ms.locfileid: "26048881"
---
# <a name="querying-data"></a>Interrogation des données

Entity Framework Core utilise LINQ (Language Integrate Query) pour interroger les données de la base de données. LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité. Une représentation de la requête LINQ est passée au fournisseur de base de données, en vue de sa conversion dans un langage de requête propre à la base de données (par exemple, SQL pour une base de données relationnelle). Pour plus d’informations sur le traitement d’une requête, consultez [Fonctionnement d’une requête](overview.md).
