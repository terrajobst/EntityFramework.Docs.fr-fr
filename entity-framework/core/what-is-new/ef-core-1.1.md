---
title: Nouveautés d’EF Core 1.1 - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26049672"
---
# <a name="new-features-in-ef-core-11"></a>Nouvelles fonctionnalités d’EF Core 1.1

## <a name="modelling"></a>Modélisation
### <a name="field-mapping"></a>Mappage de champs
Permet de configurer un champ de stockage pour une propriété. Cela peut être utile pour les propriétés en lecture seule ou les données utilisant les méthodes Get/Set au lieu d’une propriété.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mappage de tables à mémoire optimisée dans SQL Server
Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée. Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.

## <a name="change-tracking"></a>Change tracking
### <a name="additional-change-tracking-apis-from-ef6"></a>API de suivi des modifications supplémentaires dans EF6
Par exemple : `Reload`, `GetModifiedProperties`, `GetDatabaseValues`, etc.

## <a name="query"></a>Query
### <a name="explicit-loading"></a>Chargement explicite
Permet de déclencher le remplissage d’une propriété de navigation sur une entité qui a été précédemment chargée à partir de la base de données.
### <a name="dbsetfind"></a>DbSet.Find
Fournit un moyen simple de récupérer une entité en fonction de sa valeur de clé primaire.

## <a name="other"></a>Autre
### <a name="connection-resiliency"></a>Résilience de la connexion
Effectue automatiquement de nouvelles tentatives de commandes de base de données ayant échoué. Cela est particulièrement utile durant la connexion à SQL Azure, où les défaillances passagères sont courantes.
### <a name="simplified-service-replacement"></a>Remplacement de services simplifié
Facilite le remplacement de services internes utilisés par EF.
