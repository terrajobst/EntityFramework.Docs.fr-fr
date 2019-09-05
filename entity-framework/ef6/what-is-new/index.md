---
title: Nouveautés - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271443"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="c3597-102">Nouveautés dans EF6</span><span class="sxs-lookup"><span data-stu-id="c3597-102">What's New in EF6</span></span>

<span data-ttu-id="c3597-103">Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.</span><span class="sxs-lookup"><span data-stu-id="c3597-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="c3597-104">Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.</span><span class="sxs-lookup"><span data-stu-id="c3597-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="c3597-105">Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="c3597-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="c3597-106">Cette page décrit les fonctionnalités incluses dans chaque nouvelle version.</span><span class="sxs-lookup"><span data-stu-id="c3597-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="c3597-107">Versions récentes</span><span class="sxs-lookup"><span data-stu-id="c3597-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="c3597-108">Mise à jour d’EF Tools dans Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="c3597-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="c3597-109">En mai 2018, nous avons publié une version mise à jour d’EF Tools dans Visual Studio 2017 15.7.</span><span class="sxs-lookup"><span data-stu-id="c3597-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="c3597-110">Cette version contient des améliorations sur certains points de problème courants :</span><span class="sxs-lookup"><span data-stu-id="c3597-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="c3597-111">Correctifs pour plusieurs bogues d’accessibilité de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="c3597-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="c3597-112">Solution de contournement pour la régression des performances de SQL Server pendant la génération de modèles à partir de bases de données existantes [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="c3597-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="c3597-113">Prise en charge de la mise à jour des gros modèles sur SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="c3597-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="c3597-114">Une autre amélioration de cette nouvelle version d’EF Tools est l’installation du runtime EF 6.2 quand vous créez un modèle dans un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="c3597-114">Another improvement in this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="c3597-115">Avec les versions précédentes de Visual Studio, vous pouvez utiliser le runtime EF 6.2 (ainsi que n’importe quelle version précédente d’EF) en installant la version correspondante du package NuGet.</span><span class="sxs-lookup"><span data-stu-id="c3597-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="c3597-116">Runtime EF 6.2</span><span class="sxs-lookup"><span data-stu-id="c3597-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="c3597-117">Le runtime EF 6.2 a été publié sur NuGet en octobre 2017.</span><span class="sxs-lookup"><span data-stu-id="c3597-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="c3597-118">Grâce en majeure partie aux efforts de notre communauté de contributeurs open source, EF 6.2 comprend de nombreux [correctifs de bogues](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) et [améliorations de produit](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="c3597-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="c3597-119">Voici une courte liste des changements les plus importants du runtime EF 6.2 :</span><span class="sxs-lookup"><span data-stu-id="c3597-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="c3597-120">Réduction du temps de démarrage en chargeant les modèles Code First terminés à partir d’un cache persistant [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="c3597-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="c3597-121">API Fluent pour définir des index [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="c3597-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="c3597-122">DbFunctions.Like() pour activer l’écriture de requêtes LINQ qui se traduisent en LIKE dans SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="c3597-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="c3597-123">Migrate.exe prend désormais en charge l’option -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="c3597-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="c3597-124">EF6 peut maintenant utiliser des valeurs de clé générées par une séquence dans SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="c3597-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="c3597-125">Liste de mise à jour des erreurs temporaires pour la stratégie d’exécution de SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="c3597-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="c3597-126">Bogue : La réexécution des requêtes ou des commandes SQL échoue avec le message « SqlParameter est déjà contenu par un autre SqlParameterCollection » [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="c3597-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="c3597-127">Bogue : L’évaluation de DbQuery.ToString() expire dans le débogueur [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="c3597-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="c3597-128">Versions futures</span><span class="sxs-lookup"><span data-stu-id="c3597-128">Future Releases</span></span>

<span data-ttu-id="c3597-129">Pour plus d’informations sur la prochaine version d’EF6, regardez notre [Feuille de route](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="c3597-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="c3597-130">Versions précédentes</span><span class="sxs-lookup"><span data-stu-id="c3597-130">Past Releases</span></span>

<span data-ttu-id="c3597-131">La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.</span><span class="sxs-lookup"><span data-stu-id="c3597-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
