---
title: Nouveautés d’EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656012"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="d2ccd-102">Nouvelles fonctionnalités d’EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="d2ccd-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="d2ccd-103">Modélisation</span><span class="sxs-lookup"><span data-stu-id="d2ccd-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="d2ccd-104">Mappage de champs</span><span class="sxs-lookup"><span data-stu-id="d2ccd-104">Field mapping</span></span>

<span data-ttu-id="d2ccd-105">Permet de configurer un champ de stockage pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="d2ccd-106">Cela peut être utile pour les propriétés en lecture seule ou les données utilisant les méthodes Get/Set au lieu d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="d2ccd-107">Mappage de tables à mémoire optimisée dans SQL Server</span><span class="sxs-lookup"><span data-stu-id="d2ccd-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="d2ccd-108">Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="d2ccd-109">Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="d2ccd-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="d2ccd-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="d2ccd-111">API de suivi des modifications supplémentaires dans EF6</span><span class="sxs-lookup"><span data-stu-id="d2ccd-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="d2ccd-112">Par exemple : `Reload`, `GetModifiedProperties`, `GetDatabaseValues`, etc.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="d2ccd-113">Query</span><span class="sxs-lookup"><span data-stu-id="d2ccd-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="d2ccd-114">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="d2ccd-114">Explicit Loading</span></span>

<span data-ttu-id="d2ccd-115">Permet de déclencher le remplissage d’une propriété de navigation sur une entité qui a été précédemment chargée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="d2ccd-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="d2ccd-116">DbSet.Find</span></span>

<span data-ttu-id="d2ccd-117">Fournit un moyen simple de récupérer une entité en fonction de sa valeur de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="d2ccd-118">Autre</span><span class="sxs-lookup"><span data-stu-id="d2ccd-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="d2ccd-119">Résilience de la connexion</span><span class="sxs-lookup"><span data-stu-id="d2ccd-119">Connection resiliency</span></span>

<span data-ttu-id="d2ccd-120">Effectue automatiquement de nouvelles tentatives de commandes de base de données ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="d2ccd-121">Cela est particulièrement utile durant la connexion à SQL Azure, où les défaillances passagères sont courantes.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="d2ccd-122">Remplacement de services simplifié</span><span class="sxs-lookup"><span data-stu-id="d2ccd-122">Simplified service replacement</span></span>

<span data-ttu-id="d2ccd-123">Facilite le remplacement de services internes utilisés par EF.</span><span class="sxs-lookup"><span data-stu-id="d2ccd-123">Makes it easier to replace internal services that EF uses.</span></span>
