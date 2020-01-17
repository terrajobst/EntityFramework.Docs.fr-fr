---
title: Planifier Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125381"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="a4deb-102">Planifier Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="a4deb-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="a4deb-103">Comme décrit dans le [processus de planification](../release-planning.md), nous avons rassemblé les entrées des parties prenantes dans un plan provisoire pour la version EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="a4deb-104">Ce plan est toujours un travail en cours.</span><span class="sxs-lookup"><span data-stu-id="a4deb-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="a4deb-105">Rien ici n’est un engagement.</span><span class="sxs-lookup"><span data-stu-id="a4deb-105">Nothing here is a commitment.</span></span> <span data-ttu-id="a4deb-106">Ce plan est un point de départ qui évoluera à mesure que nous en apprendrons davantage.</span><span class="sxs-lookup"><span data-stu-id="a4deb-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="a4deb-107">Certains éléments qui ne sont pas actuellement planifiés pour 5,0 peuvent être extraits.</span><span class="sxs-lookup"><span data-stu-id="a4deb-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="a4deb-108">Certains éléments actuellement planifiés pour 5,0 peuvent devenir punted.</span><span class="sxs-lookup"><span data-stu-id="a4deb-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="a4deb-109">Numéro de version et date de publication.</span><span class="sxs-lookup"><span data-stu-id="a4deb-109">Version number and release date.</span></span>

<span data-ttu-id="a4deb-110">La version de EF Core 5,0 est actuellement planifiée en [même temps que .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="a4deb-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="a4deb-111">La version « 5,0 » a été choisie pour s’aligner sur .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="a4deb-112">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="a4deb-112">Supported platforms</span></span>

<span data-ttu-id="a4deb-113">EF Core 5,0 est planifiée pour s’exécuter sur toute plateforme .NET 5,0 basée sur la [convergence de ces plateformes sur .net Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="a4deb-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="a4deb-114">Ce que cela signifie en termes de .NET Standard et le TFM réel utilisé est toujours TBD.</span><span class="sxs-lookup"><span data-stu-id="a4deb-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="a4deb-115">EF Core 5,0 ne s’exécutera pas sur .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a4deb-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="a4deb-116">Modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="a4deb-116">Breaking changes</span></span>

<span data-ttu-id="a4deb-117">EF Core 5,0 contiendra des modifications avec rupture, mais celles-ci seront bien moins sévères que dans le cas de EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="a4deb-118">Notre objectif est de permettre la mise à jour sans interruption de la grande majorité des applications.</span><span class="sxs-lookup"><span data-stu-id="a4deb-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="a4deb-119">Il est prévu qu’il y ait des modifications avec rupture pour les fournisseurs de base de données, en particulier pour la prise en charge de TPT.</span><span class="sxs-lookup"><span data-stu-id="a4deb-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="a4deb-120">Toutefois, nous pensons que le travail de mise à jour d’un fournisseur pour 5,0 sera inférieur à la nécessité de mettre à jour 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="a4deb-121">Thèmes</span><span class="sxs-lookup"><span data-stu-id="a4deb-121">Themes</span></span>

<span data-ttu-id="a4deb-122">Nous avons extrait quelques principaux domaines ou thèmes qui constitueront la base des investissements importants en EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="a4deb-123">Propriétés de navigation plusieurs-à-plusieurs (a. k. a "ignorer les navigations")</span><span class="sxs-lookup"><span data-stu-id="a4deb-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="a4deb-124">Développeurs en chef : @smitpatel et @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="a4deb-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="a4deb-125">Suivi par [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="a4deb-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="a4deb-126">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-126">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-127">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-127">Status: In-progress</span></span>

<span data-ttu-id="a4deb-128">Plusieurs-à-plusieurs est la fonctionnalité la plus demandée (environ 407 votes) sur le backlog GitHub.</span><span class="sxs-lookup"><span data-stu-id="a4deb-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="a4deb-129">La prise en charge des relations plusieurs-à-plusieurs peut être divisée en trois domaines principaux :</span><span class="sxs-lookup"><span data-stu-id="a4deb-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="a4deb-130">Ignore les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="a4deb-130">Skip navigation properties.</span></span> <span data-ttu-id="a4deb-131">Celles-ci permettent d’utiliser le modèle pour les requêtes, etc. sans référence à l’entité de table de jointure sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="a4deb-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="a4deb-132">Types d’entité de conteneur de propriétés.</span><span class="sxs-lookup"><span data-stu-id="a4deb-132">Property-bag entity types.</span></span> <span data-ttu-id="a4deb-133">Celles-ci permettent l’utilisation d’un type CLR standard (par exemple, `Dictionary`) pour les instances d’entité, de sorte qu’un type CLR explicite n’est pas nécessaire pour chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="a4deb-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="a4deb-134">Sucre pour une configuration facile des relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a4deb-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="a4deb-135">Nous pensons que le bloqueur le plus significatif pour ceux qui souhaitent une prise en charge de type plusieurs-à-plusieurs ne peut pas utiliser les relations « naturelles », sans faire référence à la table de jointure, dans une logique métier telle que des requêtes.</span><span class="sxs-lookup"><span data-stu-id="a4deb-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="a4deb-136">Le type d’entité de la table de jointure peut encore exister, mais il ne doit pas être dans la logique métier.</span><span class="sxs-lookup"><span data-stu-id="a4deb-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="a4deb-137">C’est pourquoi nous avons choisi de traiter les propriétés de navigation ignorées pour 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="a4deb-138">À ce stade, les autres parties de plusieurs-à-plusieurs sont poursuivies comme un objectif d’étirement pour EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="a4deb-139">Cela signifie qu’ils ne sont pas actuellement dans le plan pour 5,0, mais si nous espérons bien les extraire.</span><span class="sxs-lookup"><span data-stu-id="a4deb-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="a4deb-140">Mappage de l’héritage TPT (table par type) (TPT)</span><span class="sxs-lookup"><span data-stu-id="a4deb-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="a4deb-141">Développeur principal : @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="a4deb-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="a4deb-142">Suivi par [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="a4deb-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="a4deb-143">Taille de T-shirt : XL</span><span class="sxs-lookup"><span data-stu-id="a4deb-143">T-shirt size: XL</span></span>

<span data-ttu-id="a4deb-144">État : non démarré</span><span class="sxs-lookup"><span data-stu-id="a4deb-144">Status: Not started</span></span>

<span data-ttu-id="a4deb-145">Nous faisons TPT parce qu’il s’agit d’une fonctionnalité hautement demandée (environ 254 votes ; 3e au total) et qu’elle nécessite des modifications de bas niveau que nous pensons à la nature fondamentale du plan .NET 5 global.</span><span class="sxs-lookup"><span data-stu-id="a4deb-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="a4deb-146">Nous pensons que cela entraînera des modifications avec rupture pour les fournisseurs de base de données, bien que ceux-ci soient nettement moins sévères que les modifications requises pour 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="a4deb-147">Include filtré</span><span class="sxs-lookup"><span data-stu-id="a4deb-147">Filtered Include</span></span>

<span data-ttu-id="a4deb-148">Développeur principal : @maumar</span><span class="sxs-lookup"><span data-stu-id="a4deb-148">Lead developer: @maumar</span></span>

<span data-ttu-id="a4deb-149">Suivi par [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="a4deb-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="a4deb-150">Taille de T-shirt : M</span><span class="sxs-lookup"><span data-stu-id="a4deb-150">T-shirt size: M</span></span>

<span data-ttu-id="a4deb-151">État : non démarré</span><span class="sxs-lookup"><span data-stu-id="a4deb-151">Status: Not started</span></span>

<span data-ttu-id="a4deb-152">Le modèle include filtré est une fonctionnalité très demandée (environ 317 votes ; 2e au total) qui n’est pas une énorme quantité de travail, et que nous pensons autoriser à débloquer ou à faciliter de nombreux scénarios qui nécessitent actuellement des filtres au niveau du modèle ou des requêtes plus complexes.</span><span class="sxs-lookup"><span data-stu-id="a4deb-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="a4deb-153">Rationaliser ToTable, ToQuery, ToView, FromSql, etc.</span><span class="sxs-lookup"><span data-stu-id="a4deb-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="a4deb-154">Développeurs en chef : @maumar et @smitpatel</span><span class="sxs-lookup"><span data-stu-id="a4deb-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="a4deb-155">Suivi par [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="a4deb-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="a4deb-156">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-156">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-157">État : non démarré</span><span class="sxs-lookup"><span data-stu-id="a4deb-157">Status: Not started</span></span>

<span data-ttu-id="a4deb-158">Nous avons progressé dans les versions précédentes jusqu’à la prise en charge de SQL brut, de types sans clé et de zones associées.</span><span class="sxs-lookup"><span data-stu-id="a4deb-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="a4deb-159">Toutefois, il existe des lacunes et des incohérences dans la façon dont tout fonctionne ensemble dans son ensemble.</span><span class="sxs-lookup"><span data-stu-id="a4deb-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="a4deb-160">L’objectif de 5,0 est de les corriger et de créer une bonne expérience pour la définition, la migration et l’utilisation de différents types d’entités et de leurs requêtes et artefacts de base de données associés.</span><span class="sxs-lookup"><span data-stu-id="a4deb-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="a4deb-161">Cela peut également impliquer des mises à jour de l’API de requête compilée.</span><span class="sxs-lookup"><span data-stu-id="a4deb-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="a4deb-162">Notez que cet élément peut entraîner des modifications avec rupture au niveau de l’application, car certaines des fonctionnalités dont nous disposons actuellement sont trop permissive, ce qui permet à l’utilisateur de mener rapidement des gens sur des problèmes.</span><span class="sxs-lookup"><span data-stu-id="a4deb-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="a4deb-163">Nous nous contenterons de bloquer certaines de ces fonctionnalités, ainsi que des conseils sur la marche à suivre.</span><span class="sxs-lookup"><span data-stu-id="a4deb-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="a4deb-164">Améliorations générales relatives aux requêtes</span><span class="sxs-lookup"><span data-stu-id="a4deb-164">General query enhancements</span></span>

<span data-ttu-id="a4deb-165">Développeurs en chef : @smitpatel et @maumar</span><span class="sxs-lookup"><span data-stu-id="a4deb-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="a4deb-166">Suivi par des [problèmes étiquetés avec `area-query` dans le jalon 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="a4deb-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="a4deb-167">Taille de T-shirt : XL</span><span class="sxs-lookup"><span data-stu-id="a4deb-167">T-shirt size: XL</span></span>

<span data-ttu-id="a4deb-168">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-168">Status: In-progress</span></span>

<span data-ttu-id="a4deb-169">Le code de traduction de la requête a été largement réécrit pour EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="a4deb-170">Le code de requête est généralement dans un état bien plus robuste.</span><span class="sxs-lookup"><span data-stu-id="a4deb-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="a4deb-171">Pour 5,0, nous ne prévoyons pas de modifier les requêtes majeures, en dehors de celles qui sont nécessaires pour prendre en charge les propriétés de navigation TPT et Skip.</span><span class="sxs-lookup"><span data-stu-id="a4deb-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="a4deb-172">Toutefois, il y a toujours un travail important nécessaire pour résoudre une dette technique à partir de la révision 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="a4deb-173">Nous prévoyons également de résoudre de nombreux bogues et d’implémenter de petites améliorations pour améliorer l’expérience globale des requêtes.</span><span class="sxs-lookup"><span data-stu-id="a4deb-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="a4deb-174">Migrations et expérience de déploiement</span><span class="sxs-lookup"><span data-stu-id="a4deb-174">Migrations and deployment experience</span></span>

<span data-ttu-id="a4deb-175">Développeurs de leads : @bricelam</span><span class="sxs-lookup"><span data-stu-id="a4deb-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="a4deb-176">Suivi par [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="a4deb-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="a4deb-177">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-177">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-178">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-178">Status: In-progress</span></span>

<span data-ttu-id="a4deb-179">Actuellement, de nombreux développeurs migrent leurs bases de données au moment du démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a4deb-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="a4deb-180">C’est facile, mais cela n’est pas recommandé car :</span><span class="sxs-lookup"><span data-stu-id="a4deb-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="a4deb-181">Plusieurs threads/processus/serveurs peuvent tenter de migrer la base de données simultanément</span><span class="sxs-lookup"><span data-stu-id="a4deb-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="a4deb-182">Les applications peuvent tenter d’accéder à un état incohérent alors que cela se produit</span><span class="sxs-lookup"><span data-stu-id="a4deb-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="a4deb-183">En général, les autorisations de base de données pour modifier le schéma ne doivent pas être accordées pour l’exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="a4deb-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="a4deb-184">Il est difficile de revenir à un état propre si un problème se pose</span><span class="sxs-lookup"><span data-stu-id="a4deb-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="a4deb-185">Nous souhaitons offrir une meilleure expérience ici, qui permet de migrer facilement la base de données au moment du déploiement.</span><span class="sxs-lookup"><span data-stu-id="a4deb-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="a4deb-186">Cela doit :</span><span class="sxs-lookup"><span data-stu-id="a4deb-186">This should:</span></span>

* <span data-ttu-id="a4deb-187">Travailler sur Linux, Mac et Windows</span><span class="sxs-lookup"><span data-stu-id="a4deb-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="a4deb-188">Soyez une bonne expérience sur la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="a4deb-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="a4deb-189">Scénarios de prise en charge avec conteneurs</span><span class="sxs-lookup"><span data-stu-id="a4deb-189">Support scenarios with containers</span></span>
* <span data-ttu-id="a4deb-190">Travailler avec des outils/flux de déploiement réels couramment utilisés</span><span class="sxs-lookup"><span data-stu-id="a4deb-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="a4deb-191">Intégrer dans au moins Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4deb-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="a4deb-192">Le résultat est probablement de nombreuses améliorations de EF Core (par exemple, de meilleures migrations sur SQLite), ainsi que des conseils et des collaborations à long terme avec d’autres équipes pour améliorer les expériences de bout en bout qui vont au-delà d’EF.</span><span class="sxs-lookup"><span data-stu-id="a4deb-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="a4deb-193">Expérience des plates-formes EF Core</span><span class="sxs-lookup"><span data-stu-id="a4deb-193">EF Core platforms experience</span></span> 

<span data-ttu-id="a4deb-194">Développeurs en chef : @roji et @bricelam</span><span class="sxs-lookup"><span data-stu-id="a4deb-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="a4deb-195">Suivi par [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="a4deb-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="a4deb-196">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-196">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-197">État : non démarré</span><span class="sxs-lookup"><span data-stu-id="a4deb-197">Status: Not started</span></span>

<span data-ttu-id="a4deb-198">Nous avons de bonnes recommandations pour l’utilisation de EF Core dans des applications Web classiques basées sur MVC.</span><span class="sxs-lookup"><span data-stu-id="a4deb-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="a4deb-199">Des conseils pour d’autres plateformes et modèles d’application sont manquants ou obsolètes.</span><span class="sxs-lookup"><span data-stu-id="a4deb-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="a4deb-200">Pour EF Core 5,0, nous prévoyons d’examiner, d’améliorer et de documenter l’expérience d’utilisation de EF Core avec :</span><span class="sxs-lookup"><span data-stu-id="a4deb-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="a4deb-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="a4deb-201">Blazor</span></span>
* <span data-ttu-id="a4deb-202">Xamarin, y compris l’utilisation de l’AOA/histoire de l’éditeur de liens</span><span class="sxs-lookup"><span data-stu-id="a4deb-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="a4deb-203">WinForms/WPF/WinUI et éventuellement d’autres U.I.</span><span class="sxs-lookup"><span data-stu-id="a4deb-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="a4deb-204">frameworks</span><span class="sxs-lookup"><span data-stu-id="a4deb-204">frameworks</span></span>

<span data-ttu-id="a4deb-205">Il s’agit probablement de nombreuses améliorations de EF Core, ainsi que de conseils et de collaborations à long terme avec d’autres équipes pour améliorer les expériences de bout en bout qui vont au-delà d’EF.</span><span class="sxs-lookup"><span data-stu-id="a4deb-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="a4deb-206">Voici quelques-uns des éléments que nous prévoyons de consulter :</span><span class="sxs-lookup"><span data-stu-id="a4deb-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="a4deb-207">Déploiement, y compris l’expérience d’utilisation des outils EF comme pour les migrations</span><span class="sxs-lookup"><span data-stu-id="a4deb-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="a4deb-208">Modèles d’application, notamment Xamarin et éblouissant, et probablement d’autres</span><span class="sxs-lookup"><span data-stu-id="a4deb-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="a4deb-209">Expériences SQLite, y compris l’expérience spatiale et les reconstructions de tables</span><span class="sxs-lookup"><span data-stu-id="a4deb-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="a4deb-210">AOA et liaison d’expériences</span><span class="sxs-lookup"><span data-stu-id="a4deb-210">AOT and linking experiences</span></span>
* <span data-ttu-id="a4deb-211">Intégration des diagnostics, y compris les compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="a4deb-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="a4deb-212">Performances</span><span class="sxs-lookup"><span data-stu-id="a4deb-212">Performance</span></span>

<span data-ttu-id="a4deb-213">Développeur principal : @roji</span><span class="sxs-lookup"><span data-stu-id="a4deb-213">Lead developer: @roji</span></span>

<span data-ttu-id="a4deb-214">Suivi par des [problèmes étiquetés avec `area-perf` dans le jalon 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="a4deb-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="a4deb-215">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-215">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-216">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-216">Status: In-progress</span></span>

<span data-ttu-id="a4deb-217">Par EF Core, nous prévoyons d’améliorer notre suite de tests de performances et d’améliorer les performances de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a4deb-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="a4deb-218">En outre, nous prévoyons de terminer la nouvelle API de traitement par lot ADO.NET qui a été prototypée durant le cycle de publication de 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="a4deb-219">En outre, au niveau de la couche ADO.NET, nous prévoyons des améliorations de performances supplémentaires pour le fournisseur npgsql.</span><span class="sxs-lookup"><span data-stu-id="a4deb-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="a4deb-220">Dans le cadre de ce travail, nous prévoyons également d’ajouter des compteurs de performances ADO.NET/EF Core et d’autres diagnostics appropriés.</span><span class="sxs-lookup"><span data-stu-id="a4deb-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="a4deb-221">Documentation architecturale/contributeur</span><span class="sxs-lookup"><span data-stu-id="a4deb-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="a4deb-222">Document en chef : @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="a4deb-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="a4deb-223">Suivi par [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="a4deb-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="a4deb-224">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-224">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-225">État : non démarré</span><span class="sxs-lookup"><span data-stu-id="a4deb-225">Status: Not started</span></span>

<span data-ttu-id="a4deb-226">L’idée ici est de faciliter la compréhension de ce qui se passe dans les éléments internes de EF Core.</span><span class="sxs-lookup"><span data-stu-id="a4deb-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="a4deb-227">Cela peut être utile pour toute personne utilisant EF Core, mais la motivation principale est de faciliter la tâche pour les utilisateurs externes :</span><span class="sxs-lookup"><span data-stu-id="a4deb-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="a4deb-228">Contribuer au code EF Core</span><span class="sxs-lookup"><span data-stu-id="a4deb-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="a4deb-229">Créer des fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="a4deb-229">Create database providers</span></span>
* <span data-ttu-id="a4deb-230">Créer d’autres extensions</span><span class="sxs-lookup"><span data-stu-id="a4deb-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="a4deb-231">Documentation de Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="a4deb-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="a4deb-232">Document en chef : @bricelam</span><span class="sxs-lookup"><span data-stu-id="a4deb-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="a4deb-233">Suivi par [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="a4deb-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="a4deb-234">Taille de T-shirt : M</span><span class="sxs-lookup"><span data-stu-id="a4deb-234">T-shirt size: M</span></span>

<span data-ttu-id="a4deb-235">État : terminé.</span><span class="sxs-lookup"><span data-stu-id="a4deb-235">Status: Completed.</span></span> <span data-ttu-id="a4deb-236">La nouvelle documentation est [en ligne sur le site Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="a4deb-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="a4deb-237">L’équipe EF possède également le fournisseur ADO.NET de Microsoft. Data. sqlite.</span><span class="sxs-lookup"><span data-stu-id="a4deb-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="a4deb-238">Nous prévoyons de documenter entièrement ce fournisseur dans le cadre de la version 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="a4deb-239">Documentation générale</span><span class="sxs-lookup"><span data-stu-id="a4deb-239">General documentation</span></span>

<span data-ttu-id="a4deb-240">Document en chef : @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="a4deb-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="a4deb-241">Suivi par des [problèmes dans la documentation référentiel dans le jalon 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="a4deb-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="a4deb-242">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-242">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-243">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-243">Status: In-progress</span></span>

<span data-ttu-id="a4deb-244">Nous sommes déjà en train de mettre à jour la documentation pour les versions 3,0 et 3,1.</span><span class="sxs-lookup"><span data-stu-id="a4deb-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="a4deb-245">Nous travaillons également sur :</span><span class="sxs-lookup"><span data-stu-id="a4deb-245">We are also working on:</span></span>
  * <span data-ttu-id="a4deb-246">Une révision des documents de prise en main pour les rendre plus simples et plus faciles à suivre</span><span class="sxs-lookup"><span data-stu-id="a4deb-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="a4deb-247">Réorganisation de docs pour faciliter la recherche et ajouter des références croisées</span><span class="sxs-lookup"><span data-stu-id="a4deb-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="a4deb-248">Ajout de détails et de clarifications à des documents existants</span><span class="sxs-lookup"><span data-stu-id="a4deb-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="a4deb-249">Mise à jour des exemples et ajout d’autres exemples</span><span class="sxs-lookup"><span data-stu-id="a4deb-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="a4deb-250">Correction des bogues</span><span class="sxs-lookup"><span data-stu-id="a4deb-250">Fixing bugs</span></span>

<span data-ttu-id="a4deb-251">Suivi par des [problèmes étiquetés avec `type-bug` dans le jalon 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="a4deb-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="a4deb-252">Développeurs : @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="a4deb-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="a4deb-253">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-253">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-254">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-254">Status: In-progress</span></span>

<span data-ttu-id="a4deb-255">Au moment de la rédaction de ce document, nous avons 135 bogues en triage pour qu’ils soient corrigés dans la version 5,0 (avec 62 déjà corrigé), mais il y a un chevauchement significatif avec la section sur les _améliorations générales des requêtes_ ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="a4deb-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="a4deb-256">Le taux entrant (les problèmes qui se terminent par un travail dans une étape majeure) était d’environ 23 problèmes par mois au cours de la version 3,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="a4deb-257">Ces derniers ne doivent pas nécessairement être corrigés dans 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="a4deb-258">En guise d’estimation approximative, nous prévoyons de résoudre les problèmes 150 supplémentaires dans le délai de 5,0.</span><span class="sxs-lookup"><span data-stu-id="a4deb-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="a4deb-259">Petites améliorations</span><span class="sxs-lookup"><span data-stu-id="a4deb-259">Small enhancements</span></span>

<span data-ttu-id="a4deb-260">Suivi par des [problèmes étiquetés avec `type-enhancement` dans le jalon 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="a4deb-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="a4deb-261">Développeurs : @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="a4deb-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="a4deb-262">Taille de T-shirt : L</span><span class="sxs-lookup"><span data-stu-id="a4deb-262">T-shirt size: L</span></span>

<span data-ttu-id="a4deb-263">État : en cours</span><span class="sxs-lookup"><span data-stu-id="a4deb-263">Status: In-progress</span></span>

<span data-ttu-id="a4deb-264">En plus des fonctionnalités plus grandes décrites ci-dessus, nous avons également de nombreuses améliorations plus petites, planifiées pour 5,0, afin de corriger les « coupes papier ».</span><span class="sxs-lookup"><span data-stu-id="a4deb-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="a4deb-265">Notez que la plupart de ces améliorations sont également couvertes par les thèmes plus généraux décrits ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="a4deb-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="a4deb-266">En dessous de la ligne</span><span class="sxs-lookup"><span data-stu-id="a4deb-266">Below-the-line</span></span>

<span data-ttu-id="a4deb-267">Suivi par des [problèmes étiquetés avec `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="a4deb-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="a4deb-268">Ce sont des correctifs de bogues et des améliorations qui **ne sont pas** actuellement planifiés pour la version 5,0, mais nous examinerons les objectifs d’étirement en fonction de la progression de la tâche ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="a4deb-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="a4deb-269">En outre, nous considérons toujours les [problèmes les plus votés](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) lors de la planification.</span><span class="sxs-lookup"><span data-stu-id="a4deb-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="a4deb-270">La réduction de l’un de ces problèmes à partir d’une version est toujours pénible, mais nous avons besoin d’un plan réaliste pour les ressources dont nous disposons.</span><span class="sxs-lookup"><span data-stu-id="a4deb-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="a4deb-271">Commentaires</span><span class="sxs-lookup"><span data-stu-id="a4deb-271">Feedback</span></span>

<span data-ttu-id="a4deb-272">Vos commentaires sur la planification sont importants.</span><span class="sxs-lookup"><span data-stu-id="a4deb-272">Your feedback on planning is important.</span></span> <span data-ttu-id="a4deb-273">La meilleure façon d’indiquer l’importance d’un problème est de voter (thumbs-up) pour ce problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="a4deb-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="a4deb-274">Ces données sont ensuite chargées dans le [processus de planification](../release-planning.md) de la prochaine version.</span><span class="sxs-lookup"><span data-stu-id="a4deb-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
