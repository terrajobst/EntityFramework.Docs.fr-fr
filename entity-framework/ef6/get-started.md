---
title: Bien démarrer avec Entity Framework 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
ms.openlocfilehash: bf54879ea94e597dfeac3e4bd70571dad290dd9e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181398"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="049a5-102">Bien démarrer avec Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="049a5-102">Get started with Entity Framework 6</span></span>

<span data-ttu-id="049a5-103">Ce guide contient un ensemble de liens vers des articles de documentation sélectionnés, des procédures pas à pas et des vidéos qui peuvent vous aider à démarrer rapidement.</span><span class="sxs-lookup"><span data-stu-id="049a5-103">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="049a5-104">Notions de base</span><span class="sxs-lookup"><span data-stu-id="049a5-104">Fundamentals</span></span>

* [<span data-ttu-id="049a5-105">Obtenir Entity Framework</span><span class="sxs-lookup"><span data-stu-id="049a5-105">Get Entity Framework</span></span>](~/ef6/fundamentals/install.md)

  <span data-ttu-id="049a5-106">Vous apprendrez ici à ajouter Entity Framework à vos applications. Par ailleurs, si vous voulez utiliser EF Designer, vérifiez qu’il est installé dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="049a5-106">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* <span data-ttu-id="049a5-107">@no__t 0Creating un modèle : Code First, le concepteur EF et les flux de travail EF @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="049a5-107">[Creating a Model: Code First, the EF Designer, and the EF Workflows](~/ef6/modeling/index.md)</span></span>

  <span data-ttu-id="049a5-108">Préférez-vous spécifier votre modèle EF en écrivant du code ou en traçant des zones et des lignes ?</span><span class="sxs-lookup"><span data-stu-id="049a5-108">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="049a5-109">Allez-vous utiliser EF pour mapper vos objets à une base de données existante ou voulez-vous qu’EF crée une base de données adaptée à vos objets ?</span><span class="sxs-lookup"><span data-stu-id="049a5-109">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="049a5-110">Ici, vous en saurez plus sur deux approches différentes pour utiliser EF6 : Le concepteur EF et Code First.</span><span class="sxs-lookup"><span data-stu-id="049a5-110">Here your learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="049a5-111">Veillez à suivre la discussion et à regarder la vidéo présentant la différence.</span><span class="sxs-lookup"><span data-stu-id="049a5-111">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="049a5-112">Utilisation de DbContext</span><span class="sxs-lookup"><span data-stu-id="049a5-112">Working with DbContext</span></span>](~/ef6/fundamentals/working-with-dbcontext.md)

  <span data-ttu-id="049a5-113">DbContext est le premier et le plus important type EF dont vous avez besoin pour apprendre à utiliser Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="049a5-113">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="049a5-114">Il sert de tremplin pour les requêtes de base de données et effectue le suivi des changements que vous apportez aux objets afin qu’ils puissent être conservés dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="049a5-114">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="049a5-115">Poser une question</span><span class="sxs-lookup"><span data-stu-id="049a5-115">Ask a Question</span></span>](~/ef6/resources/get-help.md)

  <span data-ttu-id="049a5-116">Découvrez comment obtenir de l’aide des experts et fournir vos propres réponses à la Communauté.</span><span class="sxs-lookup"><span data-stu-id="049a5-116">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="049a5-117">Contribuer</span><span class="sxs-lookup"><span data-stu-id="049a5-117">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="049a5-118">Entity Framework 6 utilise un modèle de développement ouvert.</span><span class="sxs-lookup"><span data-stu-id="049a5-118">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="049a5-119">Découvrez comment vous pouvez aider à améliorer EF en consultant notre dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="049a5-119">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="049a5-120">Ressources Code First</span><span class="sxs-lookup"><span data-stu-id="049a5-120">Code First resources</span></span>

  - [<span data-ttu-id="049a5-121">Code First ciblant un flux de travail de base de données existant</span><span class="sxs-lookup"><span data-stu-id="049a5-121">Code First to an Existing Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [<span data-ttu-id="049a5-122">Code First ciblant un nouveau flux de travail de base de données</span><span class="sxs-lookup"><span data-stu-id="049a5-122">Code First to a New Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
  - [<span data-ttu-id="049a5-123">Mappage d’enums avec Code First</span><span class="sxs-lookup"><span data-stu-id="049a5-123">Mapping Enums Using Code First</span></span>](~/ef6/modeling/code-first/data-types/enums.md)
  - [<span data-ttu-id="049a5-124">Mappage de types spatiaux avec Code First</span><span class="sxs-lookup"><span data-stu-id="049a5-124">Mapping Spatial Types Using Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)
  - [<span data-ttu-id="049a5-125">Écriture de conventions Code First personnalisées</span><span class="sxs-lookup"><span data-stu-id="049a5-125">Writing Custom Code First Conventions</span></span>](~/ef6/modeling/code-first/conventions/custom.md)
  - [<span data-ttu-id="049a5-126">Utilisation de la configuration Fluent Code First avec Visual Basic</span><span class="sxs-lookup"><span data-stu-id="049a5-126">Using Code First Fluent Configuration with Visual Basic</span></span>](~/ef6/modeling/code-first/fluent/vb.md)
  - [<span data-ttu-id="049a5-127">Migrations Code First</span><span class="sxs-lookup"><span data-stu-id="049a5-127">Code First Migrations</span></span>](~/ef6/modeling/code-first/migrations/index.md)
  - [<span data-ttu-id="049a5-128">Migrations Code First dans les environnements d’équipe</span><span class="sxs-lookup"><span data-stu-id="049a5-128">Code First Migrations in Team Environments</span></span>](~/ef6/modeling/code-first/migrations/teams.md)
  - <span data-ttu-id="049a5-129">[Migrations Code First automatiques](~/ef6/modeling/code-first/migrations/automatic.md) (désormais déconseillées)</span><span class="sxs-lookup"><span data-stu-id="049a5-129">[Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="049a5-130">Ressources EF Designer</span><span class="sxs-lookup"><span data-stu-id="049a5-130">EF Designer resources</span></span>
  - [<span data-ttu-id="049a5-131">Flux de travail Database First</span><span class="sxs-lookup"><span data-stu-id="049a5-131">Database First Workflow</span></span>](~/ef6/modeling/designer/workflows/database-first.md)
  - [<span data-ttu-id="049a5-132">Flux de travail Model First</span><span class="sxs-lookup"><span data-stu-id="049a5-132">Model First Workflow</span></span>](~/ef6/modeling/designer/workflows/model-first.md)
  - [<span data-ttu-id="049a5-133">Mappage d’enums</span><span class="sxs-lookup"><span data-stu-id="049a5-133">Mapping Enums</span></span>](~/ef6/modeling/designer/data-types/enums.md)
  - [<span data-ttu-id="049a5-134">Mappage de types spatiaux</span><span class="sxs-lookup"><span data-stu-id="049a5-134">Mapping Spatial Types</span></span>](~/ef6/modeling/designer/data-types/spatial.md)
  - [<span data-ttu-id="049a5-135">Mappage d’héritage de table par hiérarchie</span><span class="sxs-lookup"><span data-stu-id="049a5-135">Table-Per Hierarchy Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tph.md)
  - [<span data-ttu-id="049a5-136">Mappage d’héritage de table par type</span><span class="sxs-lookup"><span data-stu-id="049a5-136">Table-Per Type Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tpt.md)
  - [<span data-ttu-id="049a5-137">Mappage de procédures stockées pour les mises à jour</span><span class="sxs-lookup"><span data-stu-id="049a5-137">Stored Procedure Mapping for Updates</span></span>](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [<span data-ttu-id="049a5-138">Mappage de procédures stockées pour la requête</span><span class="sxs-lookup"><span data-stu-id="049a5-138">Stored Procedure Mapping for Query</span></span>](~/ef6/modeling/designer/stored-procedures/query.md)
  - [<span data-ttu-id="049a5-139">Fractionnement d’entité</span><span class="sxs-lookup"><span data-stu-id="049a5-139">Entity Splitting</span></span>](~/ef6/modeling/designer/entity-splitting.md)
  - [<span data-ttu-id="049a5-140">Fractionnement de table</span><span class="sxs-lookup"><span data-stu-id="049a5-140">Table Splitting</span></span>](~/ef6/modeling/designer/table-splitting.md)
  - <span data-ttu-id="049a5-141">[Requête de définition](~/ef6/modeling/designer/advanced/defining-query.md) (Avancé)</span><span class="sxs-lookup"><span data-stu-id="049a5-141">[Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Advanced)</span></span>
  - <span data-ttu-id="049a5-142">[Fonctions table](~/ef6/modeling/designer/advanced/tvfs.md) (Avancé)</span><span class="sxs-lookup"><span data-stu-id="049a5-142">[Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="049a5-143">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="049a5-143">Other resources</span></span>
  - [<span data-ttu-id="049a5-144">Requête et enregistrement asynchrones</span><span class="sxs-lookup"><span data-stu-id="049a5-144">Async Query and Save</span></span>](~/ef6/fundamentals/async.md)
  - [<span data-ttu-id="049a5-145">Liaison de données avec WinForms</span><span class="sxs-lookup"><span data-stu-id="049a5-145">Databinding with WinForms</span></span>](~/ef6/fundamentals/databinding/winforms.md)
  - [<span data-ttu-id="049a5-146">Liaison de données avec WPF</span><span class="sxs-lookup"><span data-stu-id="049a5-146">Databinding with WPF</span></span>](~/ef6/fundamentals/databinding/wpf.md)
  - <span data-ttu-id="049a5-147">[Scénarios déconnectés avec des entités de suivi automatique](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (désormais déconseillés)</span><span class="sxs-lookup"><span data-stu-id="049a5-147">[Disconnected scenarios with Self-Tracking Entities](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (This is no longer recommended)</span></span>
