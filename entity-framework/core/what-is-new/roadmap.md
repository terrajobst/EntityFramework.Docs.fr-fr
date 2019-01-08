---
title: Feuille de route d’Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: f18de8e8cb4fbe81bb2f983a00c9dd2f46be6073
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182018"
---
# <a name="entity-framework-core-roadmap"></a>Feuille de route d’Entity Framework Core

> [!IMPORTANT]
> Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.

### <a name="ef-core-30"></a>EF Core 3.0

Maintenant que EF Core 2.2 est sorti, notre priorité est EF Core 3.0, qui s’alignera sur la publication de .NET Core 3.0 et d’ASP.NET 3.0.

Nous n’avons pas encore implémenté de nouvelles fonctionnalités. Les  [packages EF Core 3.0 Preview 1 publiés sur la galerie NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1)  en décembre 2018 ne comportent que [des correctifs de bogues, des améliorations mineures et des modifications apportées en prévision du travail sur la version 3.0](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

Il nous reste en effet à affiner notre [planning de lancement](#release-planning-process) pour la version 3.0, afin de définir la liste des fonctionnalités à développer dans le temps imparti.
Nous communiquerons des informations complémentaires dès que nous en saurons plus. Voici déjà les grands thèmes et fonctionnalités sur lesquels nous prévoyons de travailler :

- **Améliorations LINQ ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))** : LINQ permet d’écrire des requêtes de base de données sans abandonner son langage de prédilection, en tirant parti de riches informations de type pour bénéficier d’IntelliSense et du contrôle de type au moment de la compilation.
  Cependant, LINQ offre également la possibilité d’écrire un nombre illimité de requêtes complexes, ce qui a toujours représenté un défi de taille pour les fournisseurs LINQ.
  Dans les premières versions d’EF Core, nous avons résolu ce problème d’une part en identifiant les parties traduisibles en SQL d’une requête, et d’autre part en exécutant le reste de la requête en mémoire sur le client.
  Si l’exécution côté client est souhaitable dans certaines situations, elle peut aboutir dans la majorité des cas à des requêtes inefficaces, non identifiées comme telles avant le déploiement en production de l’application.
  Dans EF Core 3.0, nous avons l’intention de modifier en profondeur le fonctionnement de notre implémentation LINQ et nos modes de test.
  L’objectif est multiple : la rendre plus robuste (par exemple, pour éviter d’interrompre les requêtes dans les versions de correctif), traduire correctement plus d’expressions en SQL, générer des requêtes efficaces dans davantage de cas et empêcher celles qui ne le sont pas de passer inaperçues.

- **Prise en charge de Cosmos DB ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))** : Nous travaillons sur un fournisseur Cosmos DB pour EF Core, pour permettre aux développeurs habitués au modèle de programmation EF de cibler facilement Azure Cosmos DB comme base de données d’application.
  L’objectif est de rendre certains des avantages de Cosmos DB (p. ex., distribution mondiale, disponibilité « Always On », scalabilité élastique et faible latence) encore plus accessibles aux développeurs .NET.
  Le fournisseur proposera la plupart des fonctionnalités d’EF Core, comme le suivi automatique des modifications, les conversions de valeurs et LINQ, sur l’API SQL dans Cosmos DB. Nous avons commencé ce travail avant EF Core 2.2, et [nous avons publié quelques préversions du fournisseur](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Nous prévoyons de continuer à développer le fournisseur en parallèle d’EF Core 3.0.   

- **Prise en charge de C# 8.0 ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))** : nous voulons que nos clients bénéficient avec EF Core d’une partie des [nouvelles fonctionnalités de C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) comme les flux de données asynchrones (y compris await for each) et les types référence Nullable.

- **Ingénierie à rebours des vues de base de données en types de requêtes ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))** : dans EF Core 2.1, nous avons ajouté la prise en charge des types de requêtes, qui peuvent représenter des données lisibles dans la base de données, mais non modifiables.
  Les types de requêtes sont parfaitement adaptés au mappage de vues de base de données. C’est pourquoi, dans EF Core 3.0, nous aimerions automatiser la création de types de requêtes pour les vues de base de données.

- **Entités conteneurs des propriétés ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) et [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))** : cette fonctionnalité permet de créer des entités qui stockent les données dans des propriétés indexées au lieu de propriétés standard, et d’utiliser des instances de la même classe .NET (potentiellement aussi simple que `Dictionary<string, object>`) pour représenter différents types d’entités dans le même modèle EF Core.
  Cette fonctionnalité est un tremplin pour prendre en charge les relations plusieurs-à-plusieurs sans entité de jointure – l’une des améliorations les plus demandées pour EF Core.

- **EF 6.3 sur .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))** : nous sommes conscients du fait que de nombreuses applications utilisent d’anciennes versions d’EF et qu’une migration vers EF Core dans l’unique objectif de tirer parti de .NET Core représente parfois un effort considérable.
  C’est pourquoi nous adapterons la prochaine version d’EF 6 de façon à ce qu’elle s’exécute sur .NET Core 3.0.
  L’objectif est de faciliter le portage d’applications avec aussi peu de modifications que possible.
  Certaines limitations s’appliqueront (par exemple, de nouveaux fournisseurs seront nécessaires, la prise en charge spatiale avec SQL Server ne sera pas activée) et il n’y a pas de nouvelles fonctionnalités planifiées pour EF 6.

En attendant, vous pouvez utiliser [cette requête dans notre outil de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) pour connaître les éléments de travail provisoirement assignés à la version 3.0.

## <a name="schedule"></a>Planification

Le planning pour EF Core est synchronisé avec le [planning .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) et le [planning ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Backlog

Le [jalon Backlog](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) de notre système de suivi des problèmes répertorie les problèmes sur lesquels nous comptons nous pencher ou qu’une personne de la Communauté est susceptible de résoudre.
Les clients sont invités à envoyer leurs commentaires et leurs votes concernant ces problèmes.
Les collaborateurs qui souhaitent travailler sur un de ces problèmes sont encouragés à lancer dans un premier temps une discussion sur la façon d’approcher un problème.

Il n’y a aucune garantie sur le développement d’une fonctionnalité donnée d’une version spécifique d’EF Core.
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

1. **Selon nous, combien de développeurs utiliseront la fonctionnalité et en quoi améliorera-t-elle leur application/expérience ?** Pour répondre à cette questions, nous rassemblons les commentaires de nombreuses sources, et les commentaires et les votes sur les problèmes constituent l’une de ces sources.

2. **Quelles sont les solutions de contournement possibles si nous n’implémentons pas encore cette fonctionnalité ?** Par exemple, de nombreux développeurs peuvent mapper une table de jointure afin de pallier l’absence de prise en charge native du mappage plusieurs-à-plusieurs. Évidemment, tous les développeurs ne peuvent pas le faire, mais beaucoup en sont capables, ce qui constitue un facteur important dans notre décision.

3. **L’implémentation de cette fonctionnalité fait-elle évoluer l’architecture d’EF Core de telle manière que l’implémentation d’autres fonctionnalités s’en voit facilitée ou plus probable ?** Nous avons tendance à privilégier les fonctionnalités qui agissent comme blocs de construction pour d’autres fonctionnalités. Par exemple, les entités contenant des jeux de propriétés peuvent nous aider à développer la prise en charge plusieurs-à-plusieurs, et les constructeurs d’entité favorisent la prise en charge du chargement différé. 

4. **La fonctionnalité est-elle un point d’extensibilité ?** Nous avons tendance aussi à favoriser les points d’extensibilité au détriment des fonctionnalités standard, car ils permettent aux développeurs de raccorder leurs propres comportements et d’obtenir ainsi une compensation pour certaines fonctionnalités manquantes. 

5. **Quelle est la synergie de la fonctionnalité quand elle est utilisée conjointement avec d’autres produits ?** Nous favorisons les fonctionnalités qui facilitent ou améliorent sensiblement l’expérience d’utilisation d’EF Core avec d’autres produits, tels que .NET Core, la dernière version de Visual Studio, Microsoft Azure et ainsi de suite.

6. **Quelles sont les compétences des personnes disponibles pour travailler sur une fonctionnalité, et comment exploiter au mieux ces ressources ?** Chaque membre de l’équipe EF et nos collaborateurs de la Communauté ont différents niveaux d’expérience dans différents domaines, et nous devons donc planifier en conséquence. Même si nous souhaitions faire appel en même temps à toutes ces ressources pour travailler sur une fonctionnalité spécifique telle que les traductions GroupBy ou le mappage plusieurs-à-plusieurs, ce ne serait pas pratique.

Comme mentionné précédemment, le processus évolue à chaque version.
À l’avenir, nous essaierons de proposer davantage d’opportunités aux membres de la Communauté afin qu’ils puissent participer à nos plans de mise en production.
Par exemple, nous aimerions faciliter le processus de révision de nos ébauches de fonctionnalités et du plan de mise en production lui-même.
