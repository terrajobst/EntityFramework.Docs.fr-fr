---
title: Comparer Entity Framework 6 et Entity Framework Core
description: Fournit des conseils sur la façon de choisir entre Entity Framework 6 et Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 3d2f72e64e6846d2d8bb6d4d507e04090287114d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198012"
---
# <a name="compare-ef-core--ef6"></a>Comparer EF Core et EF6

Entity Framework est un mappeur objet-relationnel (ORM) pour .NET. Cet article compare les deux versions : Entity Framework 6 et Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) est une technologie d’accès aux données éprouvée. Il a été publié pour la première fois en 2008, dans le cadre de .NET Framework 3.5 SP1 et de Visual Studio 2008 SP1. À compter de la version 4.1, il est fourni en tant que package NuGet [EntityFramework](https://www.nuget.org/packages/EntityFramework/). EF6 s’exécute sur .NET Framework 4.x et .NET Core à partir de 3.0.

EF6 continue d’être un produit pris en charge et de bénéficier de correctifs de bogues et d’améliorations mineures.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) est une réécriture complète d’EF6 publiée en 2016. Il est fourni dans des packages Nuget, le principal étant [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core est un produit multiplateforme qui s’exécute sur .NET Core.

EF Core a été conçu pour fournir une expérience de développement similaire à EF6. La plupart des API de niveau supérieur restent les mêmes ; EF Core semblera donc familier aux développeurs EF6.

## <a name="feature-comparison"></a>Comparaison des fonctionnalités

EF Core offre de nouvelles fonctionnalités qui ne seront pas implémentées dans EF6 (telles que les [clés secondaires](xref:core/modeling/alternate-keys), les [mises à jour par lot](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements) et l’[évaluation mixte client/base de données dans les requêtes LINQ](xref:core/querying/client-eval). Mais comme il s’agit d’une nouvelle base de code, il manque également certaines fonctionnalités présentes dans EF6.

Les tableaux suivants comparent les fonctionnalités disponibles dans EF Core et EF6. Il s’agit d’une comparaison générale qui ne répertorie pas toutes les fonctionnalités et n’explique pas les différences entre la même fonctionnalité dans les différentes versions d’EF.

La colonne EF Core indique la version du produit dans laquelle la fonctionnalité a été introduite pour la première fois.

### <a name="creating-a-model"></a>Création d’un modèle

| **Fonctionnalité**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mappage des classes de base                                   | Oui      | 1.0                                   |
| Constructeurs avec des paramètres                          |          | 2.1                                   |
| Conversions de valeurs de propriété                            |          | 2.1                                   |
| Types mappés sans clé                             |          | 2.1                                   |
| Conventions                                           | Oui      | 1.0                                   |
| Conventions personnalisées                                    | Oui      | 1.0 (partiel)                         |
| Annotations de données                                      | Oui      | 1.0                                   |
| API Fluent                                            | Oui      | 1.0                                   |
| Héritage : table par hiérarchie (TPH)                | Oui      | 1.0                                   |
| Héritage : table par type (TPT)                     | Oui      |                                       |
| Héritage : table par classe concrète (TPC)           | Oui      |                                       |
| Propriétés d’état de clichés instantanés                               |          | 1.0                                   |
| Clés secondaires                                        |          | 1.0                                   |
| Plusieurs-à-plusieurs sans entité de jonction                      | Oui      |                                       |
| Génération de la clé : Base de données                              | Oui      | 1.0                                   |
| Génération de la clé : Client                                |          | 1.0                                   |
| Types complexes/détenus                                   | Oui      | 2.0                                   |
| Données spatiales                                          | Oui      | 2.2                                   |
| Visualisation graphique de modèle                      | Oui      |                                       |
| Éditeur de modèle graphique                                | Oui      |                                       |
| Format de modèle : Code                                    | Oui      | 1.0                                   |
| Format de modèle : EDMX (XML)                              | Oui      |                                       |
| Créer un modèle à partir d’une base de données : Ligne de commande              | Oui      | 1.0                                   |
| Créer un modèle à partir d’une base de données : Assistant VS                 | Oui      |                                       |
| Mise à jour d’un modèle à partir d’une base de données                            | Partial  |                                       |
| Filtres de requête globale                                  |          | 2.0                                   |
| Fractionnement de table                                       | Oui      | 2.0                                   |
| Fractionnement d'entité                                      | Oui      |                                       |
| Mappage de fonctions scalaires de base de données                      | Médiocre     | 2.0                                   |
| Mappage de champs                                         |          | 1.1                                   |
| Types références Nullables (C# 8.0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Interrogation des données

| **Fonctionnalité**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Requêtes LINQ                                          | Oui      | 1.0 (en cours pour les requêtes complexes) |
| Code SQL généré lisible                                | Médiocre     | 1.0                                   |
| Traduction GroupBy                                   | Oui      | 2.1                                   |
| Chargement des données associées : hâtif                           | Oui      | 1.0                                   |
| Chargement des données associées : Chargement hâtif pour les types dérivés |          | 2.1                                   |
| Chargement des données associées : Différé                            | Oui      | 2.1                                   |
| Chargement des données associées : Explicit                        | Oui      | 1.1                                   |
| Requêtes SQL brutes : Types d'entité                         | Oui      | 1.0                                   |
| Requêtes SQL brutes : Types d’entité sans clé                 | Oui      | 2.1                                   |
| Requêtes SQL brutes : Composition avec LINQ                  |          | 1.0                                   |
| Requêtes compilées explicitement                           | Médiocre     | 2.0                                   |
| Langage de requête textuel (Entity SQL)                | Oui      |                                       |
| await foreach (C# 8.0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Enregistrement de données

| **Fonctionnalité**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Suivi des modifications : Instantané                             | Oui      | 1.0                                   |
| Suivi des modifications : Notification                         | Oui      | 1.0                                   |
| Suivi des modifications : Serveurs proxy                              | Oui      |                                       |
| État du suivi de l’accès                               | Oui      | 1.0                                   |
| Accès concurrentiel optimiste                                | Oui      | 1.0                                   |
| Transactions                                          | Oui      | 1.0                                   |
| Traitement par lot d’instructions                                |          | 1.0                                   |
| Mappage de procédure stockée                              | Oui      |                                       |
| API de bas niveau à graphes déconnectés                     | Médiocre     | 1.0                                   |
| De bout en bout à graphes déconnectés                         |          | 1.0 (partiel)                         |

### <a name="other-features"></a>Autres fonctionnalités

| **Fonctionnalité**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrations                                            | Oui      | 1.0                                   |
| API de création/suppression de base de données                       | Oui      | 1.0                                   |
| Données seed                                             | Oui      | 2.1                                   |
| Résilience de la connexion                                 | Oui      | 1.1                                   |
| Raccordements de cycle de vie (événements, interception)                | Oui      |                                       |
| Journalisation simple (Database.Log)                         | Oui      |                                       |
| Regroupement DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Fournisseurs de bases de données

| **Fonctionnalité**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Oui      | 1.0                                   |
| MySQL                                                 | Oui      | 1.0                                   |
| PostgreSQL                                            | Oui      | 1.0                                   |
| Oracle                                                | Oui      | 1.0                                   |
| SQLite                                                | Oui      | 1.0                                   |
| SQL Server Compact                                    | Oui      | 1.0 <sup>(1)</sup>                    |
| DB2                                                   | Oui      | 1.0                                   |
| Firebird                                              | Oui      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(1)</sup>                    |
| Cosmos DB                                             |          | 3.0                                   |
| In-memory (pour les tests)                               |          | 1.0                                   |

<sup>1</sup> Les fournisseurs SQL Server Compact et Jet fonctionnent uniquement sur le .NET Framework (et non sur .NET Core).

### <a name="net-implementations"></a>Implémentations de .NET

| **Fonctionnalité**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Oui                | 1.0 (supprimé dans 3.0)                  |
| .NET Core                                             | Oui (ajouté dans 6.3) | 1.0                                   |
| Mono & Xamarin                                        |                    | 1.0 (en cours)                     |
| UWP                                                   |                    | 1.0 (en cours)                     |

## <a name="guidance-for-new-applications"></a>Conseils pour les nouvelles applications

Utilisez plutôt EF Core pour une nouvelle application si les deux conditions suivantes sont remplies :
* L’application a besoin des fonctionnalités de .NET Core. Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework pour les applications serveur](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core prend en charge toutes les fonctionnalités requises par l’application. Si une fonctionnalité souhaitée est manquante, consultez la [feuille de route EF Core](xref:core/what-is-new/index) pour savoir si sa prise en charge est prévue à l’avenir. 

Utilisez plutôt EF6 si les deux conditions suivantes sont remplies :
* L’application s’exécutera sur Windows et .NET Framework 4.0 ou version ultérieure.
* EF6 prend en charge toutes les fonctionnalités requises par l’application.

## <a name="guidance-for-existing-ef6-applications"></a>Conseils pour les applications EF6 existantes

En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement. Si vous souhaitez passer à EF Core pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations. Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md). **Le déplacement d’EF6 vers EF Core est plus un portage qu’une mise à niveau.**

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez la documentation :
* <xref:core/index>
* <xref:ef6/index>
