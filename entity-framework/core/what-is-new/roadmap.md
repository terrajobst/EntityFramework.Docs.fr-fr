---
title: Feuille de route d’Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 937bfff4cbc3ec0b2235a78cb2f1f246697128d5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929782"
---
# <a name="entity-framework-core-roadmap"></a>Feuille de route d’Entity Framework Core

> [!IMPORTANT]
> Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.

## <a name="ef-core-30"></a>EF Core 3.0

EF Core 2.2 n’étant plus d’actualité, nous pouvons maintenant nous concentrer sur EF Core 3.0.
Consultez [Nouveautés d’EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index) pour plus d’informations sur les [nouvelles fonctionnalités](xref:core/what-is-new/ef-core-3.0/features) planifiées et les [changements cassants](xref:core/what-is-new/ef-core-3.0/breaking-changes) intentionnels inclus dans cette mise en production.

## <a name="schedule"></a>Planification

La planification de la version d’EF Core est synchronisée avec la [planification de la version de .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

## <a name="backlog"></a>Backlog

Le [jalon Backlog](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) dans notre suivi de problème contient les problèmes sur lesquels nous prévoyons de travailler un jour ou susceptibles selon nous d’être traités par la communauté.
Les clients sont invités à envoyer leurs commentaires et leurs votes concernant ces problèmes.
Les collaborateurs qui souhaitent travailler sur un de ces problèmes sont encouragés à lancer dans un premier temps une discussion sur la façon d’approcher un problème.

Il n’est jamais garanti que nous allons travailler sur une fonctionnalité donnée dans une version spécifique d’EF Core.
Comme dans tous les projets de logiciels, les priorités, les calendriers de publication et les ressources disponibles peuvent changer à tout moment.
Mais si nous avons l’intention de résoudre un problème dans un laps de temps défini, nous lui attribuerons un jalon de version plutôt qu’un jalon de type backlog.
Nous déplaçons régulièrement des problèmes entre les jalons backlog et de mise en production dans le cadre de notre [processus de planification de mise en production](#release-planning-process).

Nous fermons généralement un problème si nous ne prévoyons pas de le résoudre.
Mais nous pouvons reconsidérer un problème que nous avions fermé si nous obtenons de nouvelles informations à ce sujet.

## <a name="release-planning-process"></a>Processus de planning des versions

On nous demande souvent comment nous choisissons les fonctionnalités qui seront intégrées dans une version donnée.
Notre backlog ne se traduit pas du tout automatiquement en plannings de versions.
De plus, la présence d’une fonctionnalité dans EF6 ne signifie pas automatiquement qu’elle doit être implémentée dans EF Core.

Il est difficile de détailler l’ensemble du processus que nous mettons en place pour planifier une mise en production.
Une grande partie de ce processus se résume est étudier les fonctionnalités, les opportunités et les priorités spécifiques, et le processus lui-même évolue également avec chaque nouvelle version.
Mais il est relativement facile de récapituler les questions courantes auxquelles nous essayons de répondre quand nous choisissons les éléments sur lesquels nous allons travailler :

1. **Combien de développeurs vont utiliser la fonctionnalité selon nous et dans quelle mesure va-t-elle améliorer leurs applications ou leur expérience ?** Pour répondre à cette question, nous recueillons les commentaires provenant de nombreuses sources (les commentaires et votes sur les problèmes en sont une).

2. **Quelles sont les solutions de contournement possibles si nous n’implémentons pas encore cette fonctionnalité ?** Par exemple, de nombreux développeurs peuvent mapper une table de jointure afin de pallier l’absence de prise en charge native du mappage plusieurs-à-plusieurs. Évidemment, tous les développeurs ne peuvent pas le faire, mais beaucoup en sont capables, ce qui constitue un facteur important dans notre décision.

3. **L’implémentation de cette fonctionnalité fait-elle évoluer l’architecture d’EF Core de telle manière que l’implémentation d’autres fonctionnalités s’en voit facilitée ou plus probable ?** Nous avons tendance à privilégier les fonctionnalités qui agissent comme blocs de construction pour d’autres fonctionnalités. Par exemple, les entités de sac de propriétés peuvent faciliter le passage à la prise en charge du mappage plusieurs-à-plusieurs et les constructeurs d’entité permettent la prise en charge de notre chargement différé.

4. **La fonctionnalité est-elle un point d’extensibilité ?** Nous avons tendance aussi à favoriser les points d’extensibilité au détriment des fonctionnalités standard, car ils permettent aux développeurs de raccorder leurs propres comportements et d’obtenir ainsi une compensation pour certaines fonctionnalités manquantes.

5. **Quelle est la synergie de la fonctionnalité quand elle est utilisée conjointement avec d’autres produits ?** Nous favorisons les fonctionnalités qui permettent ou améliorent considérablement l’utilisation d’EF Core avec d’autres produits, comme .NET Core, la dernière version de Visual Studio, Microsoft Azure et ainsi de suite.

6. **Quelles sont les compétences des personnes disponibles pour travailler sur une fonctionnalité, et comment exploiter au mieux ces ressources ?** Chaque membre de l’équipe EF et nos collaborateurs de la Communauté ont différents niveaux d’expérience dans différents domaines, et nous devons donc planifier en conséquence. Même si nous souhaitions faire appel en même temps à toutes ces ressources pour travailler sur une fonctionnalité spécifique telle que les traductions GroupBy ou le mappage plusieurs-à-plusieurs, ce ne serait pas pratique.

Comme mentionné précédemment, le processus évolue à chaque version.
À l’avenir, nous essaierons d’ajouter davantage d’opportunités pour les membres de la communauté de fournir des entrées dans nos plans de mise en production.
Par exemple, nous aimerions faciliter le processus de révision de nos ébauches de fonctionnalités et du plan de mise en production lui-même.
