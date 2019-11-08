---
title: Gestion des schémas de base de données - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655644"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="1b2c4-102">Gestion des schémas de base de données</span><span class="sxs-lookup"><span data-stu-id="1b2c4-102">Managing Database Schemas</span></span>

<span data-ttu-id="1b2c4-103">EF Core propose deux méthodes pour que votre modèle EF Core et le schéma de base de données restent synchronisés. Pour choisir entre les deux, décidez si votre modèle EF Core ou le schéma de base de données est la source de vérité.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="1b2c4-104">Si vous souhaitez que votre modèle EF Core soit la source de vérité, utilisez [Migrations][1].</span><span class="sxs-lookup"><span data-stu-id="1b2c4-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="1b2c4-105">Quand vous apportez des modifications à votre modèle EF Core, cette approche applique progressivement les modifications de schéma correspondantes à votre base de données afin qu’elle reste compatible avec votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="1b2c4-106">Utilisez [l’ingénierie à rebours][2] si vous souhaitez que votre schéma de base de données soit la source de vérité.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="1b2c4-107">Cette approche vous permet de structurer un DbContext et les classes de type d’entité en reconstituant la logique de votre schéma de base de données dans un modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="1b2c4-108">Les [API de création et de suppression][3] peuvent également créer le schéma de base de données à partir de votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="1b2c4-109">Toutefois, elles servent principalement pour des tâches de test, de prototypage et d’autres scénarios où la suppression de la base de données est acceptable.</span><span class="sxs-lookup"><span data-stu-id="1b2c4-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
