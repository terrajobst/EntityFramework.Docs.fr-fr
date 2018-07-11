---
title: Présentation rapide d’Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913497"
---
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="7acdb-102">Présentation rapide d’Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7acdb-102">Entity Framework 6 Quick Overview</span></span>

<span data-ttu-id="7acdb-103">Entity Framework 6 (EF6) est un mappeur Objet Relationnel (O/RM) éprouvé pour .NET qui bénéficie de nombreuses années de développement de fonctionnalités et de stabilisation.</span><span class="sxs-lookup"><span data-stu-id="7acdb-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="7acdb-104">EF6 implémente de nombreuses fonctionnalités O/RM populaires :</span><span class="sxs-lookup"><span data-stu-id="7acdb-104">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="7acdb-105">Mappage de classes d’entité « ignorant la persistance » (ou « OCT », objet CLR traditionnel) qui ne dépendent pas d’un type EF</span><span class="sxs-lookup"><span data-stu-id="7acdb-105">Mapping of "persistence ignorant" (also known as "POCO", for Plan-Old CLR Object) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="7acdb-106">Détection automatique des changements</span><span class="sxs-lookup"><span data-stu-id="7acdb-106">Automatic change tracking</span></span>
- <span data-ttu-id="7acdb-107">Résolution d’identité et unité de travail</span><span class="sxs-lookup"><span data-stu-id="7acdb-107">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="7acdb-108">Chargement hâtif, différé et explicite</span><span class="sxs-lookup"><span data-stu-id="7acdb-108">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="7acdb-109">Traduction des requêtes fortement typées à l’aide de LINQ (Language Integrated Query)</span><span class="sxs-lookup"><span data-stu-id="7acdb-109">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span> 
- <span data-ttu-id="7acdb-110">Fonctionnalités de mappage complètes, notamment la prise en charge des points suivants :</span><span class="sxs-lookup"><span data-stu-id="7acdb-110">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="7acdb-111">Héritage (table par hiérarchie, table par type et table par classe concrète)</span><span class="sxs-lookup"><span data-stu-id="7acdb-111">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="7acdb-112">Types complexes</span><span class="sxs-lookup"><span data-stu-id="7acdb-112">Complex types</span></span>
  - <span data-ttu-id="7acdb-113">Procédures stockées</span><span class="sxs-lookup"><span data-stu-id="7acdb-113">Stored procedures</span></span>
- <span data-ttu-id="7acdb-114">Un concepteur visuel pour créer des modèles d’entité.</span><span class="sxs-lookup"><span data-stu-id="7acdb-114">A visual designer to create entity models.</span></span>
- <span data-ttu-id="7acdb-115">Une expérience « Code First » qui prend en charge la création de modèles d’entité en écrivant du code.</span><span class="sxs-lookup"><span data-stu-id="7acdb-115">A "Code First" experience that supports creating entity models by writing code.</span></span>
- <span data-ttu-id="7acdb-116">Les modèles peuvent être générés à partir de bases de données existantes et modifiés à la main, ou créés à partir de zéro et utilisés pour générer de nouvelles bases de données.</span><span class="sxs-lookup"><span data-stu-id="7acdb-116">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="7acdb-117">Intégration aux modèles d’application .NET Framework, y compris ASP.NET, et au moyen de la liaison de données avec WPF et WinForms.</span><span class="sxs-lookup"><span data-stu-id="7acdb-117">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="7acdb-118">Connectivité de base de données basée sur ADO.NET et nombreux fournisseurs disponibles pour établir la connexion à SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span><span class="sxs-lookup"><span data-stu-id="7acdb-118">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

<span data-ttu-id="7acdb-119">Tout comme un O/RM, EF6 réduit la différence d’impédance entre les mondes Relationnel et Objet. Cela permet aux développeurs d’écrire des applications qui interagissent avec des données stockées dans des bases de données relationnelles à l’aide d’objets .NET fortement typés représentant le domaine de l’application. De cette façon, ils n’ont plus besoin d’écrire une grande partie du code « de raccordement » qui sert à accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="7acdb-119">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="7acdb-120">Dois-je utiliser EF6 ou EF Core ?</span><span class="sxs-lookup"><span data-stu-id="7acdb-120">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="7acdb-121">EF Core est une version plus moderne, légère et extensible d’Entity Framework, qui a des avantages et des fonctionnalités très similaires à EF6.</span><span class="sxs-lookup"><span data-stu-id="7acdb-121">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="7acdb-122">EF Core est le résultat d’une réécriture complète et contient de nombreuses nouvelles fonctionnalités non disponibles dans EF6, même s’il lui manque encore certaines des fonctionnalités de mappage les plus avancées d’EF6.</span><span class="sxs-lookup"><span data-stu-id="7acdb-122">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="7acdb-123">Nous vous recommandons d’utiliser EF Core dans les nouvelles applications si l’ensemble des fonctionnalités correspond à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="7acdb-123">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="7acdb-124">La section [Comparer EF Core et EF6](xref:efcore-and-ef6/index) examine ce choix plus en détail.</span><span class="sxs-lookup"><span data-stu-id="7acdb-124">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="7acdb-125">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="7acdb-125">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="7acdb-126">Ajoutez le package NuGet EntityFramework à votre projet ou installez Entity Framework Tools pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7acdb-126">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="7acdb-127">Ensuite, regardez les vidéos, lisez les tutoriels et la documentation avancée pour vous aider à tirer le meilleur parti d’Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7acdb-127">Then watch videos, read tutorials, and advanced documentation to help you make the most of Entity Framework 6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="7acdb-128">Versions précédentes d’Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7acdb-128">Past Entity Framework Versions</span></span>

<span data-ttu-id="7acdb-129">Il s’agit de la documentation de la dernière version d’Entity Framework 6, bien qu’une grande partie s’applique aussi aux versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="7acdb-129">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="7acdb-130">Consultez les rubriques [Nouveautés](~/ef6/what-is-new/index.md) et [Versions précédentes](~/ef6/what-is-new/past-releases.md) pour obtenir la liste complète des versions EF et des fonctionnalités introduites par chacune.</span><span class="sxs-lookup"><span data-stu-id="7acdb-130">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
