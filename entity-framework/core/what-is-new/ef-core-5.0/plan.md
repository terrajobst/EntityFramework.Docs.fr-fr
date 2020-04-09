---
title: Plan pour Le noyau cadre d’entité 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136209"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="b3b97-102">Plan pour Le noyau cadre d’entité 5.0</span><span class="sxs-lookup"><span data-stu-id="b3b97-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="b3b97-103">Tel que décrit dans le processus de [planification,](../release-planning.md)nous avons recueilli les commentaires des parties prenantes dans un plan provisoire pour le rejet du noyau 5.0 de l’EF.</span><span class="sxs-lookup"><span data-stu-id="b3b97-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="b3b97-104">Ce plan est encore un travail en cours.</span><span class="sxs-lookup"><span data-stu-id="b3b97-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="b3b97-105">Rien ici n’est un engagement.</span><span class="sxs-lookup"><span data-stu-id="b3b97-105">Nothing here is a commitment.</span></span> <span data-ttu-id="b3b97-106">Ce plan est un point de départ qui évoluera au fur et à mesure que nous en apprendrons davantage.</span><span class="sxs-lookup"><span data-stu-id="b3b97-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="b3b97-107">Certaines choses qui ne sont pas actuellement prévues pour 5.0 peuvent être tirés dans.</span><span class="sxs-lookup"><span data-stu-id="b3b97-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="b3b97-108">Certaines choses actuellement prévues pour 5.0 peuvent obtenir botté.</span><span class="sxs-lookup"><span data-stu-id="b3b97-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="b3b97-109">Numéro de version et date de sortie.</span><span class="sxs-lookup"><span data-stu-id="b3b97-109">Version number and release date.</span></span>

<span data-ttu-id="b3b97-110">EF Core 5.0 est actuellement prévu pour la sortie [en même temps que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="b3b97-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="b3b97-111">La version "5.0" a été choisie pour s’aligner sur .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="b3b97-112">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="b3b97-112">Supported platforms</span></span>

<span data-ttu-id="b3b97-113">EF Core 5.0 est prévu de fonctionner sur n’importe quelle plate-forme .NET 5.0 basée sur la [convergence de ces plates-formes à .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="b3b97-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="b3b97-114">Ce que cela signifie en termes de .NET Standard et le TFM réel utilisé est toujours TBD.</span><span class="sxs-lookup"><span data-stu-id="b3b97-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="b3b97-115">EF Core 5.0 ne fonctionnera pas sur .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b3b97-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="b3b97-116">Changements cassants</span><span class="sxs-lookup"><span data-stu-id="b3b97-116">Breaking changes</span></span>

<span data-ttu-id="b3b97-117">EF Core 5.0 contiendra certains changements de rupture, mais ceux-ci seront beaucoup moins graves que ce n’était le cas pour EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="b3b97-118">Notre objectif est de permettre à la grande majorité des applications de mettre à jour sans se casser.</span><span class="sxs-lookup"><span data-stu-id="b3b97-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="b3b97-119">On s’attend à ce qu’il y ait des changements de rupture pour les fournisseurs de bases de données, en particulier autour du support TPT.</span><span class="sxs-lookup"><span data-stu-id="b3b97-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="b3b97-120">Cependant, nous nous attendons à ce que les travaux de mise à jour d’un fournisseur pour 5.0 sera inférieur à ce qui était nécessaire pour mettre à jour pour 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="b3b97-121">Thèmes</span><span class="sxs-lookup"><span data-stu-id="b3b97-121">Themes</span></span>

<span data-ttu-id="b3b97-122">Nous avons extrait quelques grands domaines ou thèmes qui constitueront la base des investissements importants dans EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="b3b97-123">Propriétés de navigation de plusieurs à plusieurs (alias « navigations de saut »)</span><span class="sxs-lookup"><span data-stu-id="b3b97-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="b3b97-124">Développeurs principaux @smitpatel : et@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="b3b97-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="b3b97-125">Suivi par [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="b3b97-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="b3b97-126">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-126">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-127">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-127">Status: In-progress</span></span>

<span data-ttu-id="b3b97-128">De nombreux sont la fonctionnalité la [plus demandée](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (407 votes) sur l’arriéré GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3b97-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="b3b97-129">Le soutien pour de nombreuses relations dans leur intégralité est suivi comme [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="b3b97-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="b3b97-130">Cela peut être divisé en trois grands domaines :</span><span class="sxs-lookup"><span data-stu-id="b3b97-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="b3b97-131">Passer les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b3b97-131">Skip navigation properties.</span></span> <span data-ttu-id="b3b97-132">Ceux-ci permettent d’utiliser le modèle pour les requêtes, etc. sans référence à l’entité sous-jacente de la table de jointure.</span><span class="sxs-lookup"><span data-stu-id="b3b97-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="b3b97-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="b3b97-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="b3b97-134">Types d’entités de sac de propriété.</span><span class="sxs-lookup"><span data-stu-id="b3b97-134">Property-bag entity types.</span></span> <span data-ttu-id="b3b97-135">Ceux-ci permettent d’utiliser un `Dictionary`type CLR standard (p. ex. ) pour les cas d’entités de telle sorte qu’un type de CLR explicite n’est pas nécessaire pour chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="b3b97-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="b3b97-136">(Étirement pour 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="b3b97-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="b3b97-137">Sucre pour une configuration facile de plusieurs à plusieurs relations.</span><span class="sxs-lookup"><span data-stu-id="b3b97-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="b3b97-138">(Étirement pour 5.0.)</span><span class="sxs-lookup"><span data-stu-id="b3b97-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="b3b97-139">Nous croyons que le bloqueur le plus important pour ceux qui veulent beaucoup à beaucoup de soutien est de ne pas être en mesure d’utiliser les relations «naturelles», sans se référer à la table de jointure, dans la logique d’affaires comme les requêtes.</span><span class="sxs-lookup"><span data-stu-id="b3b97-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="b3b97-140">Le type d’entité de table de jointure peut encore exister, mais il ne devrait pas obtenir de la manière de la logique d’affaires.</span><span class="sxs-lookup"><span data-stu-id="b3b97-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="b3b97-141">C’est pourquoi nous avons choisi de nous attaquer aux propriétés de navigation skip pour 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="b3b97-142">En ce moment, les autres parties de plusieurs à beaucoup sont poursuivies comme un objectif extensible pour EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="b3b97-143">Cela signifie qu’ils ne sont pas actuellement dans le plan pour 5.0, mais si les choses vont bien, nous espérons les tirer dans.</span><span class="sxs-lookup"><span data-stu-id="b3b97-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="b3b97-144">Cartographie de l’héritage de type par type (TPT)</span><span class="sxs-lookup"><span data-stu-id="b3b97-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="b3b97-145">Développeur principal :@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="b3b97-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="b3b97-146">Suivi par [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="b3b97-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="b3b97-147">Taille du T-shirt: XL</span><span class="sxs-lookup"><span data-stu-id="b3b97-147">T-shirt size: XL</span></span>

<span data-ttu-id="b3b97-148">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-148">Status: In-progress</span></span>

<span data-ttu-id="b3b97-149">Nous faisons TPT parce qu’il est à la fois une caractéristique très demandée (254 votes; 3e au total) et parce qu’il nécessite quelques changements de bas niveau que nous jugeons appropriés pour la nature fondamentale du plan global .NET 5.</span><span class="sxs-lookup"><span data-stu-id="b3b97-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="b3b97-150">Nous nous attendons à ce que cela entraîne des changements de rupture pour les fournisseurs de bases de données, bien que ceux-ci devraient être beaucoup moins graves que les changements requis pour 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="b3b97-151">Filtré Inclure</span><span class="sxs-lookup"><span data-stu-id="b3b97-151">Filtered Include</span></span>

<span data-ttu-id="b3b97-152">Développeur principal :@maumar</span><span class="sxs-lookup"><span data-stu-id="b3b97-152">Lead developer: @maumar</span></span>

<span data-ttu-id="b3b97-153">Suivi par [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="b3b97-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="b3b97-154">Taille du T-shirt: M</span><span class="sxs-lookup"><span data-stu-id="b3b97-154">T-shirt size: M</span></span>

<span data-ttu-id="b3b97-155">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-155">Status: In-progress</span></span>

<span data-ttu-id="b3b97-156">Filtered Include est une fonctionnalité très demandée (317 votes; 2e au total) qui n’est pas une énorme quantité de travail, et que nous croyons débloquer ou rendre plus facile de nombreux scénarios qui nécessitent actuellement des filtres de niveau modèle ou des requêtes plus complexes.</span><span class="sxs-lookup"><span data-stu-id="b3b97-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="b3b97-157">Rationaliser ToTable, ToQuery, ToView, FromSql, etc.</span><span class="sxs-lookup"><span data-stu-id="b3b97-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="b3b97-158">Développeurs principaux @maumar : et@smitpatel</span><span class="sxs-lookup"><span data-stu-id="b3b97-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="b3b97-159">Suivi par [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="b3b97-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="b3b97-160">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-160">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-161">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-161">Status: In-progress</span></span>

<span data-ttu-id="b3b97-162">Nous avons fait des progrès dans les rejets précédents vers le soutien SQL brut, types sans clé, et les domaines connexes.</span><span class="sxs-lookup"><span data-stu-id="b3b97-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="b3b97-163">Cependant, il y a à la fois des lacunes et des incohérences dans la façon dont tout fonctionne ensemble dans son ensemble.</span><span class="sxs-lookup"><span data-stu-id="b3b97-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="b3b97-164">L’objectif pour 5.0 est de les corriger et de créer une bonne expérience pour définir, migrer et utiliser différents types d’entités et leurs requêtes et artefacts de base de données associés.</span><span class="sxs-lookup"><span data-stu-id="b3b97-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="b3b97-165">Cela peut également impliquer des mises à jour de l’API de requête compilée.</span><span class="sxs-lookup"><span data-stu-id="b3b97-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="b3b97-166">Notez que cet élément peut entraîner des changements de rupture au niveau de l’application puisque certaines des fonctionnalités que nous avons actuellement est trop permissive de sorte qu’il peut rapidement conduire les gens dans des fosses de défaillance.</span><span class="sxs-lookup"><span data-stu-id="b3b97-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="b3b97-167">Nous finirons probablement par bloquer une partie de cette fonctionnalité ainsi que des conseils sur ce qu’il faut faire à la place.</span><span class="sxs-lookup"><span data-stu-id="b3b97-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="b3b97-168">Améliorations générales des requêtes</span><span class="sxs-lookup"><span data-stu-id="b3b97-168">General query enhancements</span></span>

<span data-ttu-id="b3b97-169">Développeurs principaux @smitpatel : et@maumar</span><span class="sxs-lookup"><span data-stu-id="b3b97-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="b3b97-170">Suivi par [des `area-query` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="b3b97-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="b3b97-171">Taille du T-shirt: XL</span><span class="sxs-lookup"><span data-stu-id="b3b97-171">T-shirt size: XL</span></span>

<span data-ttu-id="b3b97-172">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-172">Status: In-progress</span></span>

<span data-ttu-id="b3b97-173">Le code de traduction de requête a été largement réécrit pour EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="b3b97-174">Le code de requête est généralement dans un état beaucoup plus robuste à cause de cela.</span><span class="sxs-lookup"><span data-stu-id="b3b97-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="b3b97-175">Pour 5.0, nous n’avons pas l’intention d’apporter des changements majeurs aux requêtes, en dehors de ceux nécessaires pour prendre en charge le TPT et sauter les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b3b97-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="b3b97-176">Cependant, il reste encore beaucoup à faire pour régler une partie de la dette technique restante à la révision de 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="b3b97-177">Nous prévoyons également de corriger de nombreux bogues et de mettre en œuvre de petites améliorations pour améliorer davantage l’expérience globale de la requête.</span><span class="sxs-lookup"><span data-stu-id="b3b97-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="b3b97-178">Migrations et expérience de déploiement</span><span class="sxs-lookup"><span data-stu-id="b3b97-178">Migrations and deployment experience</span></span>

<span data-ttu-id="b3b97-179">Développeurs principaux :@bricelam</span><span class="sxs-lookup"><span data-stu-id="b3b97-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="b3b97-180">Suivi par [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="b3b97-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="b3b97-181">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-181">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-182">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-182">Status: In-progress</span></span>

<span data-ttu-id="b3b97-183">Actuellement, de nombreux développeurs migrent leurs bases de données au moment du démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3b97-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="b3b97-184">C’est facile, mais n’est pas recommandé parce que:</span><span class="sxs-lookup"><span data-stu-id="b3b97-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="b3b97-185">Plusieurs threads/processus/serveurs peuvent tenter de migrer la base de données en même temps</span><span class="sxs-lookup"><span data-stu-id="b3b97-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="b3b97-186">Les demandes peuvent essayer d’accéder à un état incohérent pendant que cela se produit</span><span class="sxs-lookup"><span data-stu-id="b3b97-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="b3b97-187">Habituellement, les autorisations de base de données pour modifier le schéma ne doivent pas être accordées pour l’exécution de la demande</span><span class="sxs-lookup"><span data-stu-id="b3b97-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="b3b97-188">Il est difficile de revenir à un état propre si quelque chose va mal</span><span class="sxs-lookup"><span data-stu-id="b3b97-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="b3b97-189">Nous voulons offrir ici une meilleure expérience qui permette de migrer facilement la base de données au moment du déploiement.</span><span class="sxs-lookup"><span data-stu-id="b3b97-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="b3b97-190">Cela devrait:</span><span class="sxs-lookup"><span data-stu-id="b3b97-190">This should:</span></span>

* <span data-ttu-id="b3b97-191">Travaillez sur Linux, Mac et Windows</span><span class="sxs-lookup"><span data-stu-id="b3b97-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="b3b97-192">Soyez une bonne expérience sur la ligne de commandement</span><span class="sxs-lookup"><span data-stu-id="b3b97-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="b3b97-193">Scénarios de soutien avec conteneurs</span><span class="sxs-lookup"><span data-stu-id="b3b97-193">Support scenarios with containers</span></span>
* <span data-ttu-id="b3b97-194">Travailler avec des outils/flux de déploiement réels couramment utilisés</span><span class="sxs-lookup"><span data-stu-id="b3b97-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="b3b97-195">Intégrer dans au moins Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3b97-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="b3b97-196">Il en résultera probablement de nombreuses petites améliorations dans EF Core (par exemple, de meilleures migrations sur SQLite), ainsi que des conseils et des collaborations à long terme avec d’autres équipes afin d’améliorer les expériences de bout en bout qui vont au-delà de l’EF.</span><span class="sxs-lookup"><span data-stu-id="b3b97-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="b3b97-197">Expérience des plates-formes EF Core</span><span class="sxs-lookup"><span data-stu-id="b3b97-197">EF Core platforms experience</span></span> 

<span data-ttu-id="b3b97-198">Développeurs principaux @roji : et@bricelam</span><span class="sxs-lookup"><span data-stu-id="b3b97-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="b3b97-199">Suivi par [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="b3b97-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="b3b97-200">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-200">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-201">Statut: Pas commencé</span><span class="sxs-lookup"><span data-stu-id="b3b97-201">Status: Not started</span></span>

<span data-ttu-id="b3b97-202">Nous avons de bons conseils pour l’utilisation de EF Core dans les applications Web traditionnelles de MVC.</span><span class="sxs-lookup"><span data-stu-id="b3b97-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="b3b97-203">Les conseils pour d’autres plates-formes et modèles d’application sont manquants ou obsolètes.</span><span class="sxs-lookup"><span data-stu-id="b3b97-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="b3b97-204">Pour EF Core 5.0, nous prévoyons d’étudier, d’améliorer et de documenter l’expérience de l’utilisation de EF Core avec :</span><span class="sxs-lookup"><span data-stu-id="b3b97-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="b3b97-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="b3b97-205">Blazor</span></span>
* <span data-ttu-id="b3b97-206">Xamarin, y compris l’utilisation de l’histoire AOT/linker</span><span class="sxs-lookup"><span data-stu-id="b3b97-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="b3b97-207">WinForms/WPF/WinUI et peut-être d’autres U.I.</span><span class="sxs-lookup"><span data-stu-id="b3b97-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="b3b97-208">frameworks</span><span class="sxs-lookup"><span data-stu-id="b3b97-208">frameworks</span></span>

<span data-ttu-id="b3b97-209">Il s’agira probablement de nombreuses petites améliorations dans EF Core, ainsi que des conseils et des collaborations à long terme avec d’autres équipes pour améliorer les expériences de bout en bout qui vont au-delà de l’EF.</span><span class="sxs-lookup"><span data-stu-id="b3b97-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="b3b97-210">Les domaines spécifiques que nous prévoyons examiner sont les :</span><span class="sxs-lookup"><span data-stu-id="b3b97-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="b3b97-211">Déploiement, y compris l’expérience de l’utilisation de l’outillage EF comme pour les migrations</span><span class="sxs-lookup"><span data-stu-id="b3b97-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="b3b97-212">Modèles d’application, y compris Xamarin et Blazor, et probablement d’autres</span><span class="sxs-lookup"><span data-stu-id="b3b97-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="b3b97-213">Expériences SQLite, y compris l’expérience spatiale et la reconstruction de la table</span><span class="sxs-lookup"><span data-stu-id="b3b97-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="b3b97-214">AOT et lier les expériences</span><span class="sxs-lookup"><span data-stu-id="b3b97-214">AOT and linking experiences</span></span>
* <span data-ttu-id="b3b97-215">Intégration diagnostique, y compris les compteurs perf</span><span class="sxs-lookup"><span data-stu-id="b3b97-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="b3b97-216">Performances</span><span class="sxs-lookup"><span data-stu-id="b3b97-216">Performance</span></span>

<span data-ttu-id="b3b97-217">Développeur principal :@roji</span><span class="sxs-lookup"><span data-stu-id="b3b97-217">Lead developer: @roji</span></span>

<span data-ttu-id="b3b97-218">Suivi par [des `area-perf` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="b3b97-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="b3b97-219">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-219">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-220">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-220">Status: In-progress</span></span>

<span data-ttu-id="b3b97-221">Pour EF Core, nous prévoyons d’améliorer notre gamme de repères de performance et d’améliorer les performances dirigées au temps d’exécution.</span><span class="sxs-lookup"><span data-stu-id="b3b97-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="b3b97-222">En outre, nous prévoyons de compléter la nouvelle ADO.NET aPI de lotage qui a été prototype au cours du cycle de libération 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="b3b97-223">Également à la couche ADO.NET, nous prévoyons des améliorations de performance supplémentaires au fournisseur Npgsql.</span><span class="sxs-lookup"><span data-stu-id="b3b97-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="b3b97-224">Dans le cadre de ce travail, nous prévoyons également d’ajouter ADO.NET/EF compteurs de performance de base et d’autres diagnostics, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="b3b97-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="b3b97-225">Documentation architecturale/contributive</span><span class="sxs-lookup"><span data-stu-id="b3b97-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="b3b97-226">Documenteur principal :@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="b3b97-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="b3b97-227">Suivi par [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="b3b97-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="b3b97-228">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-228">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-229">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-229">Status: In-progress</span></span>

<span data-ttu-id="b3b97-230">L’idée ici est de faciliter la compréhension de ce qui se passe dans les internes d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3b97-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="b3b97-231">Cela peut être utile à toute personne utilisant EF Core, mais la motivation principale est de le rendre plus facile pour les personnes externes à:</span><span class="sxs-lookup"><span data-stu-id="b3b97-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="b3b97-232">Contribuer au code EF Core</span><span class="sxs-lookup"><span data-stu-id="b3b97-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="b3b97-233">Créer des fournisseurs de bases de données</span><span class="sxs-lookup"><span data-stu-id="b3b97-233">Create database providers</span></span>
* <span data-ttu-id="b3b97-234">Construire d’autres extensions</span><span class="sxs-lookup"><span data-stu-id="b3b97-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="b3b97-235">Documentation Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="b3b97-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="b3b97-236">Documenteur principal :@bricelam</span><span class="sxs-lookup"><span data-stu-id="b3b97-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="b3b97-237">Suivi par [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="b3b97-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="b3b97-238">Taille du T-shirt: M</span><span class="sxs-lookup"><span data-stu-id="b3b97-238">T-shirt size: M</span></span>

<span data-ttu-id="b3b97-239">Statut: Terminé.</span><span class="sxs-lookup"><span data-stu-id="b3b97-239">Status: Completed.</span></span> <span data-ttu-id="b3b97-240">La nouvelle documentation est [en direct sur le site de docs Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="b3b97-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="b3b97-241">L’équipe EF est également propriétaire du fournisseur de ADO.NET Microsoft.Data.Sqlite.</span><span class="sxs-lookup"><span data-stu-id="b3b97-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="b3b97-242">Nous prévoyons de documenter entièrement ce fournisseur dans le cadre de la version 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="b3b97-243">Documentation générale</span><span class="sxs-lookup"><span data-stu-id="b3b97-243">General documentation</span></span>

<span data-ttu-id="b3b97-244">Documenteur principal :@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="b3b97-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="b3b97-245">Suivi par [des questions dans le repo docs dans le jalon 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="b3b97-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="b3b97-246">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-246">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-247">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-247">Status: In-progress</span></span>

<span data-ttu-id="b3b97-248">Nous sommes déjà en train de mettre à jour la documentation pour les versions 3.0 et 3.1.</span><span class="sxs-lookup"><span data-stu-id="b3b97-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="b3b97-249">Nous travaillons également sur :</span><span class="sxs-lookup"><span data-stu-id="b3b97-249">We are also working on:</span></span>
  * <span data-ttu-id="b3b97-250">Une refonte des documents qui commencent à les rendre plus accessibles/plus faciles à suivre</span><span class="sxs-lookup"><span data-stu-id="b3b97-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="b3b97-251">Réorganisation des docs pour faciliter la recherche et ajouter des références croisées</span><span class="sxs-lookup"><span data-stu-id="b3b97-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="b3b97-252">Ajout de plus de détails et de clarifications aux documents existants</span><span class="sxs-lookup"><span data-stu-id="b3b97-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="b3b97-253">Mise à jour des échantillons et ajout d’autres exemples</span><span class="sxs-lookup"><span data-stu-id="b3b97-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="b3b97-254">Correction des bogues</span><span class="sxs-lookup"><span data-stu-id="b3b97-254">Fixing bugs</span></span>

<span data-ttu-id="b3b97-255">Suivi par [des `type-bug` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="b3b97-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="b3b97-256">Développeurs: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="b3b97-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="b3b97-257">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-257">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-258">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-258">Status: In-progress</span></span>

<span data-ttu-id="b3b97-259">Au moment d’écrire ces lignes, nous avons 135 bogues triés pour être corrigés dans la version 5.0 (avec 62 déjà fixés), mais il ya un chevauchement significatif avec la section _d’amélioration des requêtes générales_ ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b3b97-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="b3b97-260">Le taux entrant (les questions qui finissent comme le travail dans une étape importante) était d’environ 23 questions par mois au cours de la version 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="b3b97-261">Tous ces éléments n’auront pas besoin d’être fixés en 5.0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="b3b97-262">Comme estimation approximative, nous prévoyons fixer 150 problèmes supplémentaires dans le délai de 5,0.</span><span class="sxs-lookup"><span data-stu-id="b3b97-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="b3b97-263">Petites améliorations</span><span class="sxs-lookup"><span data-stu-id="b3b97-263">Small enhancements</span></span>

<span data-ttu-id="b3b97-264">Suivi par [des `type-enhancement` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="b3b97-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="b3b97-265">Développeurs: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="b3b97-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="b3b97-266">Taille T-shirt: L</span><span class="sxs-lookup"><span data-stu-id="b3b97-266">T-shirt size: L</span></span>

<span data-ttu-id="b3b97-267">Statut: En cours</span><span class="sxs-lookup"><span data-stu-id="b3b97-267">Status: In-progress</span></span>

<span data-ttu-id="b3b97-268">En plus des plus grandes caractéristiques décrites ci-dessus, nous avons également de nombreuses améliorations plus petites prévues pour 5.0 pour fixer "paper-cuts".</span><span class="sxs-lookup"><span data-stu-id="b3b97-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="b3b97-269">Notez que bon nombre de ces améliorations sont également couvertes par les thèmes plus généraux décrits ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b3b97-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="b3b97-270">En dessous de la ligne</span><span class="sxs-lookup"><span data-stu-id="b3b97-270">Below-the-line</span></span>

<span data-ttu-id="b3b97-271">Suivi par [des `consider-for-next-release` questions étiquetées avec](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="b3b97-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="b3b97-272">Ce sont des corrections de bogues et des améliorations qui ne sont **pas** actuellement prévues pour la version 5.0, mais nous allons regarder comme des objectifs extensibles en fonction des progrès réalisés sur le travail ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b3b97-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="b3b97-273">En outre, nous considérons toujours les [questions les plus votées](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) lors de la planification.</span><span class="sxs-lookup"><span data-stu-id="b3b97-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="b3b97-274">Couper l’une ou l’autre de ces questions d’une version est toujours douloureux, mais nous avons besoin d’un plan réaliste pour les ressources dont nous disposons.</span><span class="sxs-lookup"><span data-stu-id="b3b97-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="b3b97-275">Commentaires</span><span class="sxs-lookup"><span data-stu-id="b3b97-275">Feedback</span></span>

<span data-ttu-id="b3b97-276">Vos commentaires sur la planification sont importants.</span><span class="sxs-lookup"><span data-stu-id="b3b97-276">Your feedback on planning is important.</span></span> <span data-ttu-id="b3b97-277">La meilleure façon d’indiquer l’importance d’un problème est de voter (pouce vers le haut) pour ce problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3b97-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="b3b97-278">Ces données alimenteront ensuite le [processus de planification](../release-planning.md) de la prochaine version.</span><span class="sxs-lookup"><span data-stu-id="b3b97-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
