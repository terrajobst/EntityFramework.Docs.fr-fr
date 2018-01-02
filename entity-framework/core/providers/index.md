---
title: "Fournisseurs de base de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Fournisseurs de bases de données

Entity Framework Core utilise un modèle de fournisseur qui permet d’utiliser EF pour accéder à de nombreuses bases de données différentes. Certains concepts, communs à la plupart des bases de données, sont inclus dans les principaux composants d’EF Core. Ces concepts comprennent l’expression des requêtes dans LINQ, les transactions et le suivi des modifications apportées aux objets une fois qu’ils sont chargés à partir de la base de données. Certains concepts sont propres à un fournisseur. Par exemple, le fournisseur SQL Server vous permet de configurer des tables à mémoire optimisée (fonctionnalité spécifique de SQL Server). D’autres concepts sont propres à une classe de fournisseurs. Par exemple, les fournisseurs EF Core pour les bases de données relationnelles reposent sur la bibliothèque `Microsoft.EntityFrameworkCore.Relational` commune, qui fournit des API pour configurer les mappages de tables et de colonnes, les contraintes de clé étrangère, etc.

Les fournisseurs EF Core sont créés à partir de différentes sources. Les fournisseurs ne sont pas tous gérés dans le cadre du projet Entity Framework Core. Quand vous envisagez un fournisseur tiers, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vous assurer qu’il répond à vos besoins.
