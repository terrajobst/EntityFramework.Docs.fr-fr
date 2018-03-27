---
title: Feuille de route d’Entity Framework Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 5aef679df2ecdfe7f59458c8994d0d17b4a889ff
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
---
# <a name="entity-framework-core-roadmap"></a>Feuille de route d’Entity Framework Core

> [!IMPORTANT]
> Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.

La première préversion d’EF Core 2.1 a été publiée en février 2018. Vous trouverez des informations supplémentaires sur cette version dans [Nouveautés d’EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Nous avons l’intention de publier des préversions supplémentaires d’EF Core 2.1 tous les mois et une version finale durant le deuxième trimestre 2018.

Nous n’avons pas terminé le [planning des versions](#release-planning-process) pour la version qui suivra la version 2.1.

## <a name="schedule"></a>Planning

Le planning pour EF Core est synchronisé avec le [planning .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) et le [planning ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Backlog

Nous utilisons le [jalon Backlog](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) dans notre suivi des problèmes afin de tenir à jour une liste détaillée des problèmes et des fonctionnalités. Les clients peuvent les commenter et voter pour eux.

Nous laissons généralement ouverts les problèmes sur lesquels nous prévoyons de travailler à un moment donné, ou ceux qu’un membre de la Communauté pourrait traiter, mais cela n’implique pas notre intention de les résoudre dans un laps de temps spécifique tant que nous ne les avons pas assignés à un jalon spécifique dans le cadre de notre [processus de planning des versions](#release-planning-process).

Si nous prévoyons de ne pas implémenter une fonctionnalité, nous fermons généralement le problème. Un problème que nous avons fermé peut être ré-étudié ultérieurement si nous obtenons de nouvelles informations à son sujet.

Ceci dit, nous n’avons pas suffisamment de visibilité pour pouvoir affirmer qu’une fonctionnalité X sera résolue d’ici telle date ou telle version. Comme dans tous les projets logiciels, les priorités, les plannings des versions et les ressources disponibles peuvent changer à tout moment.

## <a name="release-planning-process"></a>Processus de planning des versions

On nous demande souvent comment nous choisissons les fonctionnalités qui seront intégrées dans une version donnée. Notre backlog ne se traduit pas du tout automatiquement en plannings de versions. De plus, la présence d’une fonctionnalité dans EF6 ne signifie pas automatiquement qu’elle doit être implémentée dans EF Core.

Il est difficile de détailler ici l’ensemble du processus que nous suivons pour planifier une version, en partie car il s’agit pour la plupart de discussions concernant les fonctionnalités spécifiques, les opportunités et les priorités, et en partie car le processus lui-même évolue généralement avec chaque version. Toutefois, il est relativement facile de récapituler les questions courantes auxquelles nous essayons de répondre quand nous choisissons les éléments sur lesquels nous allons travailler :

1. **Selon nous, combien de développeurs utiliseront la fonctionnalité et en quoi améliorera-t-elle leur application/expérience ?** Nous rassemblons les commentaires de nombreuses sources. Les commentaires et les votes sur les problèmes constituent l’une de ces sources.

2. **Quelles sont les solutions de contournement possibles si nous n’implémentons pas encore cette fonctionnalité ?** Par exemple, de nombreux développeurs peuvent mapper une table de jointure afin de pallier l’absence de prise en charge native du mappage plusieurs-à-plusieurs. Évidemment, tous les développeurs n’en sont pas capables, mais nombre d’entre eux le sont et il s’agit d’un facteur qui compte.

3. **L’implémentation de cette fonctionnalité fait-elle évoluer l’architecture d’EF Core de telle manière que l’implémentation d’autres fonctionnalités s’en voit facilitée ou plus probable ?** Nous avons tendance à favoriser les fonctionnalités qui servent de pivots à d’autres fonctionnalités. Par exemple, le fractionnement de table qui a été effectué pour les types acquis nous aide à avancer vers la prise en charge de TPT.

4. **La fonctionnalité est-elle un point d’extensibilité ?** Nous avons tendance aussi à favoriser les points d’extensibilité, car ils permettent aux développeurs de raccorder plus facilement leurs propres comportements et d’obtenir ainsi certaines des fonctionnalités manquantes. Nous prévoyons de faire cela entre autres pour commencer le travail de chargement différé.

5. **Quelle est la synergie de la fonctionnalité quand elle est utilisée conjointement avec d’autres produits ?** Nous avons tendance à favoriser les fonctionnalités qui permettent d’utiliser EF Core avec d’autres produits ou d’améliorer sensiblement l’expérience d’utilisation d’autres produits, tels que .NET Core, la dernière version de Visual Studio, Microsoft Azure et ainsi de suite.

6. **Quelles sont les compétences des personnes disponibles pour travailler sur une fonctionnalité, et comment exploiter au mieux ces ressources ?** Chaque membre de l’équipe EF, et même nos collaborateurs de la Communauté, ont différents niveaux d’expérience dans différents domaines, et nous devons planifier en conséquence. Même si nous souhaitions faire appel en même temps à toutes ces ressources pour travailler sur une fonctionnalité spécifique telle que les traductions GroupBy ou le mappage plusieurs-à-plusieurs, ce ne serait pas pratique.

Comme mentionné précédemment, ce processus évolue à chaque version et nous souhaiterions offrir davantage d’opportunités aux membres de la Communauté de développeurs de participer aux plannings de version, par exemple en facilitant la révision des brouillons de fonctionnalités proposés et du planning de version proprement dit.
