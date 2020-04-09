---
title: Planification de sortie EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417335"
---
# <a name="release-planning-process"></a>Processus de planning des versions

On nous demande souvent comment nous choisissons les fonctionnalités qui seront intégrées dans une version donnée.
Ce document décrit le processus que nous utilisons.
Le processus est en constante évolution à mesure que nous trouvons de meilleures façons de planifier, mais les idées générales demeurent les mêmes.

## <a name="different-kinds-of-releases"></a>Différents types de sorties

Différents types de libération contiennent différents types de changements.
Cela signifie à son tour que la planification de la version est différente pour différents types de libération.

### <a name="patch-releases"></a>Communiqués patch

Les versions patch ne changent que la partie "patch" de la version.
Par exemple, EF Core 3.1. **1** est une version qui corrige les problèmes trouvés dans EF Core 3.1. **0**.

Les versions patch sont destinées à corriger les bogues critiques des clients.
Cela signifie que les versions de patch n’incluent pas de nouvelles fonctionnalités.
Les modifications de l’API ne sont pas autorisées dans les rejets de correctifs, sauf dans des circonstances particulières.

La barre pour faire un changement dans une version de patch est très élevé.
C’est parce qu’il est essentiel que les versions de patch n’introduisent pas de nouveaux bogues.
Par conséquent, le processus décisionnel met l’accent sur la valeur élevée et le faible risque.

Nous sommes plus susceptibles de corriger un problème si :
  * Il a un impact sur plusieurs clients
  * Il s’agit d’une régression par rapport à une version précédente
  * L’échec provoque la corruption des données

Nous sommes moins susceptibles de corriger un problème si :
  * Il existe des solution de contournement raisonnables
  * Le correctif a un risque élevé de briser autre chose
  * Le bogue est dans un coin-cas

Cette barre augmente progressivement à travers la durée de vie d’un [support à long terme (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) communiqué. C’est parce que les communiqués LTS mettent l’accent sur la stabilité.

La décision finale de corriger ou non un problème est prise par les administrateurs .NET chez Microsoft.

### <a name="minor-releases"></a>Sorties mineures

Les versions mineures ne modifient que la partie "mineure" de la version.
Par exemple, EF Core 3. **1**.0 est une version qui s’améliore sur EF Core 3. **0**.0.

Sorties mineures:
* Sont destinés à améliorer la qualité et les caractéristiques de la version précédente
* Contiennent généralement des corrections de bogues et de nouvelles fonctionnalités
* N’incluez pas les changements de rupture intentionnels
* Faites pousser quelques avant-premières avant la mort à NuGet

### <a name="major-releases"></a>Communiqués majeurs

Les versions majeures modifient le numéro de version « majeur » de l’EF.
Par exemple, EF Core **3**.0.0 est une version majeure qui fait un grand pas en avant sur EF Core 2.2.x.

Communiqués majeurs:
* Sont destinés à améliorer la qualité et les caractéristiques de la version précédente
* Contiennent généralement des corrections de bogues et de nouvelles fonctionnalités
  * Certaines des nouvelles fonctionnalités peuvent être des changements fondamentaux dans la façon dont EF Core fonctionne
* Typiquement inclure des changements de rupture intentionnelles
  * Briser les changements sont nécessaires dans l’évolution du noyau EF au fur et à mesure que nous apprenons
  * Cependant, nous réfléchissons très attentivement à faire tout changement de rupture en raison de l’impact potentiel du client. Nous avons peut-être été trop agressifs en cassant les changements dans le passé. À l’avenir, nous nous efforcerons de minimiser les changements qui cassent les applications et de réduire les modifications qui brisent les fournisseurs de bases de données et les extensions.
* Avoir de nombreux aperçus prérelease poussé à NuGet

## <a name="planning-for-majorminor-releases"></a>Planification des rejets majeurs/mineurs

### <a name="github-issue-tracking"></a>Suivi des problèmes GitHub

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) est la source de vérité pour toute la planification EF Core.

Les questions sur GitHub ont:

* Un état
  * [Les](https://github.com/dotnet/efcore/issues) questions ouvertes n’ont pas été abordées.
  * Les questions [fermées](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) ont été abordées.
    * Tous les problèmes qui ont été corrigés sont [étiquetés avec fermé-fixe](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Un problème étiqueté avec fixe fermée est fixe et fusionné, mais peut-être pas publié.
    * D’autres `closed-` étiquettes indiquent d’autres raisons de clore un problème. Par exemple, les doublons sont étiquetés avec dupliqué fermé.
* Un type
  * [Les bogues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) représentent des bogues.
  * [Les améliorations](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) représentent de nouvelles fonctionnalités ou de meilleures fonctionnalités dans les fonctionnalités existantes.
* Une étape importante
  * [Les questions sans jalon](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) sont prises en considération par l’équipe. La décision sur ce qu’il faut prendre avec la question n’a pas encore été prise ou un changement dans la décision est à l’étude.
  * [Les questions de l’étape de l’arriéré](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) sont des éléments sur lequel l’équipe EF envisagera de travailler dans un communiqué futur.
    * Les problèmes dans l’arriéré peuvent être [étiquetés avec l’examen pour la prochaine version](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indiquant que cet élément de travail est en haut de la liste pour la prochaine version.
  * Les questions ouvertes dans une étape importante en version sont des éléments sur lequel l’équipe prévoit de travailler dans cette version. Par exemple, [ce sont les questions sur lesquelles nous prévoyons de travailler pour EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Les questions fermées dans une étape importante en version sont des questions qui sont complétées pour cette version. Notez que la version n’a peut-être pas encore été publiée. Par exemple, [ce sont les questions remplies pour EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Votes!
  * Le vote est la meilleure façon d’indiquer qu’un problème est important pour vous.
  * Pour voter, il suffit d’ajouter 👍 un "pouce vers le haut" à la question. Par exemple, [il s’agit des questions les mieux votées](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * S’il vous plaît également commenter décrivant les raisons spécifiques dont vous avez besoin de la fonctionnalité si vous pensez que cela ajoute de la valeur. Commenter « 1 » ou similaire n’ajoute pas de valeur.

### <a name="the-planning-process"></a>Processus de planification

Le processus de planification est plus impliqué que de simplement prendre les caractéristiques les plus demandées de l’arriéré.
C’est parce que nous recueillons les commentaires de plusieurs intervenants de multiples façons.
Nous façonnons ensuite une version basée sur :

* Entrée des clients
* Commentaires d’autres parties prenantes
* Orientation stratégique
* Ressources disponibles
* Planifier

Voici quelques-unes des questions que nous posons :

1. **Combien de développeurs vont utiliser la fonctionnalité selon nous et dans quelle mesure va-t-elle améliorer leurs applications ou leur expérience ?** Pour répondre à cette question, nous recueillons les commentaires provenant de nombreuses sources (les commentaires et votes sur les problèmes en sont une). Des engagements spécifiques avec des clients importants en sont un autre.

2. **Quelles sont les solutions de contournement possibles si nous n’implémentons pas encore cette fonctionnalité ?** Par exemple, de nombreux développeurs peuvent mapper une table de jointure afin de pallier l’absence de prise en charge native du mappage plusieurs-à-plusieurs. Évidemment, tous les développeurs ne peuvent pas le faire, mais beaucoup en sont capables, ce qui constitue un facteur important dans notre décision.

3. **L’implémentation de cette fonctionnalité fait-elle évoluer l’architecture d’EF Core de telle manière que l’implémentation d’autres fonctionnalités s’en voit facilitée ou plus probable ?** Nous avons tendance à privilégier les fonctionnalités qui agissent comme blocs de construction pour d’autres fonctionnalités. Par exemple, les entités de sac de propriétés peuvent faciliter le passage à la prise en charge du mappage plusieurs-à-plusieurs et les constructeurs d’entité permettent la prise en charge de notre chargement différé.

4. **La fonctionnalité est-elle un point d’extensibilité ?** Nous avons tendance aussi à favoriser les points d’extensibilité au détriment des fonctionnalités standard, car ils permettent aux développeurs de raccorder leurs propres comportements et d’obtenir ainsi une compensation pour certaines fonctionnalités manquantes.

5. **Quelle est la synergie de la fonctionnalité quand elle est utilisée conjointement avec d’autres produits ?** Nous favorisons les fonctionnalités qui permettent ou améliorent considérablement l’utilisation d’EF Core avec d’autres produits, comme .NET Core, la dernière version de Visual Studio, Microsoft Azure et ainsi de suite.

6. **Quelles sont les compétences des personnes disponibles pour travailler sur une fonctionnalité, et comment pouvons-nous tirer le meilleur parti de ces ressources?** Chaque membre de l’équipe EF et nos collaborateurs de la Communauté ont différents niveaux d’expérience dans différents domaines, et nous devons donc planifier en conséquence. Même si nous souhaitions faire appel en même temps à toutes ces ressources pour travailler sur une fonctionnalité spécifique telle que les traductions GroupBy ou le mappage plusieurs-à-plusieurs, ce ne serait pas pratique.
