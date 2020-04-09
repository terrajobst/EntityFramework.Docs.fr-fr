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
# <a name="plan-for-entity-framework-core-50"></a>Plan pour Le noyau cadre d’entité 5.0

Tel que décrit dans le processus de [planification,](../release-planning.md)nous avons recueilli les commentaires des parties prenantes dans un plan provisoire pour le rejet du noyau 5.0 de l’EF.

> [!IMPORTANT] 
> Ce plan est encore un travail en cours. Rien ici n’est un engagement. Ce plan est un point de départ qui évoluera au fur et à mesure que nous en apprendrons davantage. Certaines choses qui ne sont pas actuellement prévues pour 5.0 peuvent être tirés dans. Certaines choses actuellement prévues pour 5.0 peuvent obtenir botté.

### <a name="version-number-and-release-date"></a>Numéro de version et date de sortie.

EF Core 5.0 est actuellement prévu pour la sortie [en même temps que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). La version "5.0" a été choisie pour s’aligner sur .NET 5.0.

### <a name="supported-platforms"></a>Plateformes prises en charge

EF Core 5.0 est prévu de fonctionner sur n’importe quelle plate-forme .NET 5.0 basée sur la [convergence de ces plates-formes à .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Ce que cela signifie en termes de .NET Standard et le TFM réel utilisé est toujours TBD.

EF Core 5.0 ne fonctionnera pas sur .NET Framework.

### <a name="breaking-changes"></a>Changements cassants

EF Core 5.0 contiendra certains changements de rupture, mais ceux-ci seront beaucoup moins graves que ce n’était le cas pour EF Core 3.0. Notre objectif est de permettre à la grande majorité des applications de mettre à jour sans se casser.

On s’attend à ce qu’il y ait des changements de rupture pour les fournisseurs de bases de données, en particulier autour du support TPT. Cependant, nous nous attendons à ce que les travaux de mise à jour d’un fournisseur pour 5.0 sera inférieur à ce qui était nécessaire pour mettre à jour pour 3.0.

## <a name="themes"></a>Thèmes

Nous avons extrait quelques grands domaines ou thèmes qui constitueront la base des investissements importants dans EF Core 5.0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Propriétés de navigation de plusieurs à plusieurs (alias « navigations de saut »)

Développeurs principaux @smitpatel : et@AndriySvyryd

Suivi par [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Taille T-shirt: L

Statut: En cours

De nombreux sont la fonctionnalité la [plus demandée](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (407 votes) sur l’arriéré GitHub.

Le soutien pour de nombreuses relations dans leur intégralité est suivi comme [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508). Cela peut être divisé en trois grands domaines :

* Passer les propriétés de navigation. Ceux-ci permettent d’utiliser le modèle pour les requêtes, etc. sans référence à l’entité sous-jacente de la table de jointure. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Types d’entités de sac de propriété. Ceux-ci permettent d’utiliser un `Dictionary`type CLR standard (p. ex. ) pour les cas d’entités de telle sorte qu’un type de CLR explicite n’est pas nécessaire pour chaque type d’entité. (Étirement pour 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Sucre pour une configuration facile de plusieurs à plusieurs relations. (Étirement pour 5.0.)

Nous croyons que le bloqueur le plus important pour ceux qui veulent beaucoup à beaucoup de soutien est de ne pas être en mesure d’utiliser les relations «naturelles», sans se référer à la table de jointure, dans la logique d’affaires comme les requêtes. Le type d’entité de table de jointure peut encore exister, mais il ne devrait pas obtenir de la manière de la logique d’affaires. C’est pourquoi nous avons choisi de nous attaquer aux propriétés de navigation skip pour 5.0.

En ce moment, les autres parties de plusieurs à beaucoup sont poursuivies comme un objectif extensible pour EF Core 5.0. Cela signifie qu’ils ne sont pas actuellement dans le plan pour 5.0, mais si les choses vont bien, nous espérons les tirer dans.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Cartographie de l’héritage de type par type (TPT)

Développeur principal :@AndriySvyryd

Suivi par [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Taille du T-shirt: XL

Statut: En cours

Nous faisons TPT parce qu’il est à la fois une caractéristique très demandée (254 votes; 3e au total) et parce qu’il nécessite quelques changements de bas niveau que nous jugeons appropriés pour la nature fondamentale du plan global .NET 5. Nous nous attendons à ce que cela entraîne des changements de rupture pour les fournisseurs de bases de données, bien que ceux-ci devraient être beaucoup moins graves que les changements requis pour 3.0.

## <a name="filtered-include"></a>Filtré Inclure

Développeur principal :@maumar

Suivi par [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Taille du T-shirt: M

Statut: En cours

Filtered Include est une fonctionnalité très demandée (317 votes; 2e au total) qui n’est pas une énorme quantité de travail, et que nous croyons débloquer ou rendre plus facile de nombreux scénarios qui nécessitent actuellement des filtres de niveau modèle ou des requêtes plus complexes.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Rationaliser ToTable, ToQuery, ToView, FromSql, etc.

Développeurs principaux @maumar : et@smitpatel

Suivi par [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Taille T-shirt: L

Statut: En cours

Nous avons fait des progrès dans les rejets précédents vers le soutien SQL brut, types sans clé, et les domaines connexes. Cependant, il y a à la fois des lacunes et des incohérences dans la façon dont tout fonctionne ensemble dans son ensemble. L’objectif pour 5.0 est de les corriger et de créer une bonne expérience pour définir, migrer et utiliser différents types d’entités et leurs requêtes et artefacts de base de données associés. Cela peut également impliquer des mises à jour de l’API de requête compilée.

Notez que cet élément peut entraîner des changements de rupture au niveau de l’application puisque certaines des fonctionnalités que nous avons actuellement est trop permissive de sorte qu’il peut rapidement conduire les gens dans des fosses de défaillance. Nous finirons probablement par bloquer une partie de cette fonctionnalité ainsi que des conseils sur ce qu’il faut faire à la place.

## <a name="general-query-enhancements"></a>Améliorations générales des requêtes

Développeurs principaux @smitpatel : et@maumar

Suivi par [des `area-query` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Taille du T-shirt: XL

Statut: En cours

Le code de traduction de requête a été largement réécrit pour EF Core 3.0. Le code de requête est généralement dans un état beaucoup plus robuste à cause de cela. Pour 5.0, nous n’avons pas l’intention d’apporter des changements majeurs aux requêtes, en dehors de ceux nécessaires pour prendre en charge le TPT et sauter les propriétés de navigation. Cependant, il reste encore beaucoup à faire pour régler une partie de la dette technique restante à la révision de 3.0. Nous prévoyons également de corriger de nombreux bogues et de mettre en œuvre de petites améliorations pour améliorer davantage l’expérience globale de la requête.

## <a name="migrations-and-deployment-experience"></a>Migrations et expérience de déploiement

Développeurs principaux :@bricelam

Suivi par [#19587](https://github.com/dotnet/efcore/issues/19587)

Taille T-shirt: L

Statut: En cours

Actuellement, de nombreux développeurs migrent leurs bases de données au moment du démarrage de l’application. C’est facile, mais n’est pas recommandé parce que:

* Plusieurs threads/processus/serveurs peuvent tenter de migrer la base de données en même temps
* Les demandes peuvent essayer d’accéder à un état incohérent pendant que cela se produit
* Habituellement, les autorisations de base de données pour modifier le schéma ne doivent pas être accordées pour l’exécution de la demande
* Il est difficile de revenir à un état propre si quelque chose va mal

Nous voulons offrir ici une meilleure expérience qui permette de migrer facilement la base de données au moment du déploiement. Cela devrait:

* Travaillez sur Linux, Mac et Windows
* Soyez une bonne expérience sur la ligne de commandement
* Scénarios de soutien avec conteneurs
* Travailler avec des outils/flux de déploiement réels couramment utilisés
* Intégrer dans au moins Visual Studio

Il en résultera probablement de nombreuses petites améliorations dans EF Core (par exemple, de meilleures migrations sur SQLite), ainsi que des conseils et des collaborations à long terme avec d’autres équipes afin d’améliorer les expériences de bout en bout qui vont au-delà de l’EF.

## <a name="ef-core-platforms-experience"></a>Expérience des plates-formes EF Core 

Développeurs principaux @roji : et@bricelam

Suivi par [#19588](https://github.com/dotnet/efcore/issues/19588)

Taille T-shirt: L

Statut: Pas commencé

Nous avons de bons conseils pour l’utilisation de EF Core dans les applications Web traditionnelles de MVC. Les conseils pour d’autres plates-formes et modèles d’application sont manquants ou obsolètes. Pour EF Core 5.0, nous prévoyons d’étudier, d’améliorer et de documenter l’expérience de l’utilisation de EF Core avec :

* Blazor
* Xamarin, y compris l’utilisation de l’histoire AOT/linker
* WinForms/WPF/WinUI et peut-être d’autres U.I. frameworks

Il s’agira probablement de nombreuses petites améliorations dans EF Core, ainsi que des conseils et des collaborations à long terme avec d’autres équipes pour améliorer les expériences de bout en bout qui vont au-delà de l’EF.

Les domaines spécifiques que nous prévoyons examiner sont les :

* Déploiement, y compris l’expérience de l’utilisation de l’outillage EF comme pour les migrations
* Modèles d’application, y compris Xamarin et Blazor, et probablement d’autres
* Expériences SQLite, y compris l’expérience spatiale et la reconstruction de la table
* AOT et lier les expériences
* Intégration diagnostique, y compris les compteurs perf

## <a name="performance"></a>Performances

Développeur principal :@roji

Suivi par [des `area-perf` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Taille T-shirt: L

Statut: En cours

Pour EF Core, nous prévoyons d’améliorer notre gamme de repères de performance et d’améliorer les performances dirigées au temps d’exécution. En outre, nous prévoyons de compléter la nouvelle ADO.NET aPI de lotage qui a été prototype au cours du cycle de libération 3.0. Également à la couche ADO.NET, nous prévoyons des améliorations de performance supplémentaires au fournisseur Npgsql.

Dans le cadre de ce travail, nous prévoyons également d’ajouter ADO.NET/EF compteurs de performance de base et d’autres diagnostics, le cas échéant.

## <a name="architecturalcontributor-documentation"></a>Documentation architecturale/contributive

Documenteur principal :@ajcvickers

Suivi par [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Taille T-shirt: L

Statut: En cours

L’idée ici est de faciliter la compréhension de ce qui se passe dans les internes d’EF Core. Cela peut être utile à toute personne utilisant EF Core, mais la motivation principale est de le rendre plus facile pour les personnes externes à:

* Contribuer au code EF Core
* Créer des fournisseurs de bases de données
* Construire d’autres extensions

## <a name="microsoftdatasqlite-documentation"></a>Documentation Microsoft.Data.Sqlite

Documenteur principal :@bricelam

Suivi par [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

Taille du T-shirt: M

Statut: Terminé. La nouvelle documentation est [en direct sur le site de docs Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

L’équipe EF est également propriétaire du fournisseur de ADO.NET Microsoft.Data.Sqlite. Nous prévoyons de documenter entièrement ce fournisseur dans le cadre de la version 5.0.

## <a name="general-documentation"></a>Documentation générale

Documenteur principal :@ajcvickers

Suivi par [des questions dans le repo docs dans le jalon 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Taille T-shirt: L

Statut: En cours

Nous sommes déjà en train de mettre à jour la documentation pour les versions 3.0 et 3.1. Nous travaillons également sur :
  * Une refonte des documents qui commencent à les rendre plus accessibles/plus faciles à suivre
  * Réorganisation des docs pour faciliter la recherche et ajouter des références croisées
  * Ajout de plus de détails et de clarifications aux documents existants
  * Mise à jour des échantillons et ajout d’autres exemples

## <a name="fixing-bugs"></a>Correction des bogues

Suivi par [des `type-bug` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Développeurs: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers

Taille T-shirt: L

Statut: En cours

Au moment d’écrire ces lignes, nous avons 135 bogues triés pour être corrigés dans la version 5.0 (avec 62 déjà fixés), mais il ya un chevauchement significatif avec la section _d’amélioration des requêtes générales_ ci-dessus.

Le taux entrant (les questions qui finissent comme le travail dans une étape importante) était d’environ 23 questions par mois au cours de la version 3.0. Tous ces éléments n’auront pas besoin d’être fixés en 5.0. Comme estimation approximative, nous prévoyons fixer 150 problèmes supplémentaires dans le délai de 5,0.

## <a name="small-enhancements"></a>Petites améliorations

Suivi par [des `type-enhancement` questions étiquetées avec dans le jalon 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Développeurs: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , ,@ajcvickers

Taille T-shirt: L

Statut: En cours

En plus des plus grandes caractéristiques décrites ci-dessus, nous avons également de nombreuses améliorations plus petites prévues pour 5.0 pour fixer "paper-cuts". Notez que bon nombre de ces améliorations sont également couvertes par les thèmes plus généraux décrits ci-dessus.

## <a name="below-the-line"></a>En dessous de la ligne

Suivi par [des `consider-for-next-release` questions étiquetées avec](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Ce sont des corrections de bogues et des améliorations qui ne sont **pas** actuellement prévues pour la version 5.0, mais nous allons regarder comme des objectifs extensibles en fonction des progrès réalisés sur le travail ci-dessus.

En outre, nous considérons toujours les [questions les plus votées](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) lors de la planification. Couper l’une ou l’autre de ces questions d’une version est toujours douloureux, mais nous avons besoin d’un plan réaliste pour les ressources dont nous disposons.

## <a name="feedback"></a>Commentaires

Vos commentaires sur la planification sont importants. La meilleure façon d’indiquer l’importance d’un problème est de voter (pouce vers le haut) pour ce problème sur GitHub. Ces données alimenteront ensuite le [processus de planification](../release-planning.md) de la prochaine version.
