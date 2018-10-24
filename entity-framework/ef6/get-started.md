---
title: Bien démarrer avec Entity Framework 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
ms.openlocfilehash: c25bf16bd2c39530d54b286b7743ceb83c941e4d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489284"
---
# <a name="get-started-with-entity-framework-6"></a>Bien démarrer avec Entity Framework 6

Ce guide contient un ensemble de liens vers des articles de documentation sélectionnés, des procédures pas à pas et des vidéos qui peuvent vous aider à démarrer rapidement.

## <a name="fundamentals"></a>Notions de base

* [Obtenir Entity Framework](~/ef6/fundamentals/install.md)

  Vous apprendrez ici à ajouter Entity Framework à vos applications. Par ailleurs, si vous voulez utiliser EF Designer, vérifiez qu’il est installé dans Visual Studio.

* [Création d’un modèle : Code First, EF Designer et les flux de travail EF](~/ef6/modeling/index.md)

  Préférez-vous spécifier votre modèle EF en écrivant du code ou en traçant des zones et des lignes ?
Allez-vous utiliser EF pour mapper vos objets à une base de données existante ou voulez-vous qu’EF crée une base de données adaptée à vos objets ?
Vous découvrez ici deux approches différentes pour utiliser EF6 : EF Designer et Code First.
Veillez à suivre la discussion et à regarder la vidéo présentant la différence.

* [Utilisation de DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext est le premier et le plus important type EF dont vous avez besoin pour apprendre à utiliser Entity Framework. Il sert de tremplin pour les requêtes de base de données et effectue le suivi des changements que vous apportez aux objets afin qu’ils puissent être conservés dans la base de données.

* [Poser une question](~/ef6/resources/get-help.md)

  Découvrez comment obtenir de l’aide des experts et fournir vos propres réponses à la Communauté.

* [Contribuer](http://github.com/aspnet/EntityFramework6/)

  Entity Framework 6 utilise un modèle de développement ouvert. Découvrez comment vous pouvez aider à améliorer EF en consultant notre dépôt GitHub.

## <a name="code-first-resources"></a>Ressources Code First

  - [Code First ciblant un flux de travail de base de données existant](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First ciblant un nouveau flux de travail de base de données](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mappage d’enums avec Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mappage de types spatiaux avec Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Écriture de conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md)
  - [Utilisation de la configuration Fluent Code First avec Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md)
  - [Migrations Code First dans les environnements d’équipe](~/ef6/modeling/code-first/migrations/teams.md)
  - [Migrations Code First automatiques](~/ef6/modeling/code-first/migrations/automatic.md) (désormais déconseillées)

## <a name="ef-designer-resources"></a>Ressources EF Designer
  - [Flux de travail Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Flux de travail Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Mappage d’enums](~/ef6/modeling/designer/data-types/enums.md)
  - [Mappage de types spatiaux](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mappage d’héritage de table par hiérarchie](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mappage d’héritage de table par type](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mappage de procédures stockées pour les mises à jour](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mappage de procédures stockées pour la requête](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Fractionnement d’entité](~/ef6/modeling/designer/entity-splitting.md)
  - [Fractionnement de table](~/ef6/modeling/designer/table-splitting.md)
  - [Requête de définition](~/ef6/modeling/designer/advanced/defining-query.md) (Avancé)
  - [Fonctions table](~/ef6/modeling/designer/advanced/tvfs.md) (Avancé)

## <a name="other-resources"></a>Autres ressources
  - [Requête et enregistrement asynchrones](~/ef6/fundamentals/async.md)
  - [Liaison de données avec WinForms](~/ef6/fundamentals/databinding/winforms.md)
  - [Liaison de données avec WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Scénarios déconnectés avec des entités de suivi automatique](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (désormais déconseillés)
