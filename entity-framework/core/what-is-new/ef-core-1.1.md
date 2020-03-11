---
title: Nouveautés d’EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417516"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="be37c-102">Nouvelles fonctionnalités d’EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="be37c-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="be37c-103">Modélisation</span><span class="sxs-lookup"><span data-stu-id="be37c-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="be37c-104">Mappages de champs</span><span class="sxs-lookup"><span data-stu-id="be37c-104">Field mapping</span></span>

<span data-ttu-id="be37c-105">Permet de configurer un champ de stockage pour une propriété.</span><span class="sxs-lookup"><span data-stu-id="be37c-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="be37c-106">Cela peut être utile pour les propriétés en lecture seule ou les données utilisant les méthodes Get/Set au lieu d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="be37c-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="be37c-107">Mappage de tables à mémoire optimisée dans SQL Server</span><span class="sxs-lookup"><span data-stu-id="be37c-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="be37c-108">Vous pouvez spécifier que la table à laquelle est mappée une entité a une mémoire optimisée.</span><span class="sxs-lookup"><span data-stu-id="be37c-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="be37c-109">Quand EF Core est utilisé pour créer et gérer une base de données basée sur votre modèle (avec des migrations ou `Database.EnsureCreated()`), une table à mémoire optimisée est créée pour ces entités.</span><span class="sxs-lookup"><span data-stu-id="be37c-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="be37c-110">Suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="be37c-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="be37c-111">API de suivi des modifications supplémentaires dans EF6</span><span class="sxs-lookup"><span data-stu-id="be37c-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="be37c-112">Par exemple : `Reload`, `GetModifiedProperties`, `GetDatabaseValues`, etc.</span><span class="sxs-lookup"><span data-stu-id="be37c-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="be37c-113">Requête</span><span class="sxs-lookup"><span data-stu-id="be37c-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="be37c-114">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="be37c-114">Explicit Loading</span></span>

<span data-ttu-id="be37c-115">Permet de déclencher le remplissage d’une propriété de navigation sur une entité qui a été précédemment chargée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="be37c-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="be37c-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="be37c-116">DbSet.Find</span></span>

<span data-ttu-id="be37c-117">Fournit un moyen simple de récupérer une entité en fonction de sa valeur de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="be37c-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="be37c-118">Autres</span><span class="sxs-lookup"><span data-stu-id="be37c-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="be37c-119">Résilience de connexion</span><span class="sxs-lookup"><span data-stu-id="be37c-119">Connection resiliency</span></span>

<span data-ttu-id="be37c-120">Effectue automatiquement de nouvelles tentatives de commandes de base de données ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="be37c-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="be37c-121">Cela est particulièrement utile durant la connexion à SQL Azure, où les défaillances passagères sont courantes.</span><span class="sxs-lookup"><span data-stu-id="be37c-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="be37c-122">Remplacement de services simplifié</span><span class="sxs-lookup"><span data-stu-id="be37c-122">Simplified service replacement</span></span>

<span data-ttu-id="be37c-123">Facilite le remplacement de services internes utilisés par EF.</span><span class="sxs-lookup"><span data-stu-id="be37c-123">Makes it easier to replace internal services that EF uses.</span></span>
