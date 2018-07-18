---
title: Comparaison par fonctionnalité entre EF Core et EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 84b40f03cdab27fd6fc68c5bb65c6e3d238f226a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967136"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Comparaison par fonctionnalité entre EF Core et EF6

Le tableau suivant compare les fonctionnalités disponibles dans EF Core et EF6. Il a pour but d’établir une comparaison de haut niveau, sans pour autant répertorier toutes les fonctionnalités, ou de fournir des informations détaillées sur les éventuelles différences de fonctionnement d’une même fonctionnalité.

La colonne EF Core contient le numéro de la version du produit dans laquelle la fonctionnalité a été introduite pour la première fois.

| **Création d'un modèle**                                  | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mappage des classes de base                                   | Oui      | 1.0                                   |
| Constructeurs avec des paramètres                          |          | 2.1                                   |
| Conversions de valeurs de propriété                            |          | 2.1                                   |
| Types mappés sans clé (types de requêtes)               |          | 2.1                                   |
| Conventions                                           | Oui      | 1.0                                   |
| Conventions personnalisées                                    | Oui      | 1.0 (partiel)                         |
| Annotations de données                                      | Oui      | 1.0                                   |
| API Fluent                                            | Oui      | 1.0                                   |
| Héritage : table par hiérarchie (TPH)                | Oui      | 1.0                                   |
| Héritage : table par type (TPT)                     | Oui      |                                       |
| Héritage : table par classe concrète (TPC)           | Oui      |                                       |
| Propriétés d’état de clichés instantanés                               |          | 1.0                                   |
| Clés secondaires                                        |          | 1.0                                   |
| Plusieurs-à-plusieurs sans entité de jonction                      | Oui      |                                       |
| Génération de clés : base de données                              | Oui      | 1.0                                   |
| Génération de clés : client                                |          | 1.0                                   |
| Types complexes/détenus                                   | Oui      | 2.0                                   |
| Données spatiales                                          | Oui      |                                       |
| Visualisation graphique de modèle                      | Oui      |                                       |
| Éditeur de modèle graphique                                | Oui      |                                       |
| Format de modèle : code                                    | Oui      | 1.0                                   |
| Format de modèle : EDMX (XML)                              | Oui      |                                       |
| Création d’un modèle à partir d’une base de données : ligne de commande              | Oui      | 1.0                                   |
| Création d’un modèle à partir d’une base de données : assistant VS                 | Oui      |                                       |
| Mise à jour d’un modèle à partir d’une base de données                            | Partial  |                                       |
| Filtres de requête globale                                  |          | 2.0                                   |
| Fractionnement de table                                       | Oui      | 2.0                                   |
| Fractionnement d'entité                                      | Oui      |                                       |
| Mappage de fonctions scalaires de base de données                      | Médiocre     | 2.0                                   |
| Mappage de champs                                         |          | 1.1                                   |
|                                                       |          |                                       |
| **Interrogation des données**                                     | **EF6**  | **EF Core**                           |
| Requêtes LINQ                                          | Oui      | 1.0 (en cours pour les requêtes complexes) |
| Code SQL généré lisible                                | Médiocre     | 1.0                                   |
| Évaluation du client/serveur mixte                        |          | 1.0                                   |
| Traduction GroupBy                                   | Oui      | 2.1                                   |
| Chargement des données associées : hâtif                           | Oui      | 1.0                                   |
| Chargement de données associées : chargement hâtif pour les types dérivés |          | 2.1                                   |
| Chargement des données associées : différé                            | Oui      | 2.1                                   |
| Chargement des données associées : explicite                        | Oui      | 1.1                                   |
| Requêtes SQL brutes : types d’entités                         | Oui      | 1.0                                   |
| Requêtes SQL brutes : types autres que des types d’entités (types de requêtes)       | Oui      | 2.1                                   |
| Requêtes SQL brutes : composition avec LINQ                  |          | 1.0                                   |
| Requêtes compilées explicitement                           | Médiocre     | 2.0                                   |
| Langage de requête textuel (Entity SQL)                | Oui      |                                       |
|                                                       |          |                                       |
| **Enregistrer des données**                                       | **EF6**  | **EF Core**                           |
| Suivi des modifications : instantané                             | Oui      | 1.0                                   |
| Suivi des modifications : notification                         | Oui      | 1.0                                   |
| Suivi des modifications : proxys                              | Oui      |                                       |
| État du suivi de l’accès                               | Oui      | 1.0                                   |
| Accès concurrentiel optimiste                                | Oui      | 1.0                                   |
| Transactions                                          | Oui      | 1.0                                   |
| Traitement par lot d’instructions                                |          | 1.0                                   |
| Mappage de procédure stockée                              | Oui      |                                       |
| API de bas niveau à graphes déconnectés                     | Médiocre     | 1.0                                   |
| De bout en bout à graphes déconnectés                         |          | 1.0 (partiel)                         |
|                                                       |          |                                       |
| **Autres fonctionnalités**                                    | **EF6**  | **EF Core**                           |
| Migrations                                            | Oui      | 1.0                                   |
| API de création/suppression de base de données                       | Oui      | 1.0                                   |
| Données seed                                             | Oui      | 2.1                                   |
| Résilience de la connexion                                 | Oui      | 1.1                                   |
| Raccordements de cycle de vie (événements, interception)                | Oui      |                                       |
| Journalisation simple (Database.Log)                         | Oui      |                                       |
| Regroupement DbContext                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Fournisseurs de bases de données**                                | **EF6**  | **EF Core**                           |
| SQL Server                                            | Oui      | 1.0                                   |
| MySQL                                                 | Oui      | 1.0                                   |
| PostgreSQL                                            | Oui      | 1.0                                   |
| Oracle                                                | Oui      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Oui      | 1.0                                   |
| SQL Server Compact                                    | Oui      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Oui      | 1.0                                   |
| Firebird                                              | Oui      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| In-memory (pour les tests)                               |          | 1.0                                   |
|                                                       |          |                                       |
| **Plateformes**                                         | **EF6**  | **EF Core**                           |
| .NET Framework (console, WinForms, WPF, ASP.NET)      | Oui      | 1.0                                   |
| .NET Core (console, ASP.NET Core)                     |          | 1.0                                   |
| Mono & Xamarin                                        |          | 1.0 (en cours)                     |
| UWP                                                   |          | 1.0 (en cours)                     |

<sup>1</sup> Un fournisseur payant est actuellement disponible. Un fournisseur officiel gratuit pour Oracle est en cours de préparation.
<sup>2</sup> Ce fournisseur fonctionne uniquement sur le .NET Framework (et non sur .NET Core).
