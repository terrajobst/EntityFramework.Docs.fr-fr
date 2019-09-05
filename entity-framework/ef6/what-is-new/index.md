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
# <a name="whats-new-in-ef6"></a>Nouveautés dans EF6

Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.
Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.
Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).

Cette page décrit les fonctionnalités incluses dans chaque nouvelle version.

## <a name="recent-releases"></a>Versions récentes

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Mise à jour d’EF Tools dans Visual Studio 2017 15.7

En mai 2018, nous avons publié une version mise à jour d’EF Tools dans Visual Studio 2017 15.7.
Cette version contient des améliorations sur certains points de problème courants :

- Correctifs pour plusieurs bogues d’accessibilité de l’interface utilisateur
- Solution de contournement pour la régression des performances de SQL Server pendant la génération de modèles à partir de bases de données existantes [#4](https://github.com/aspnet/entityframework6/issues/4)
- Prise en charge de la mise à jour des gros modèles sur SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Une autre amélioration de cette nouvelle version d’EF Tools est l’installation du runtime EF 6.2 quand vous créez un modèle dans un nouveau projet. Avec les versions précédentes de Visual Studio, vous pouvez utiliser le runtime EF 6.2 (ainsi que n’importe quelle version précédente d’EF) en installant la version correspondante du package NuGet.

### <a name="ef-62-runtime"></a>Runtime EF 6.2

Le runtime EF 6.2 a été publié sur NuGet en octobre 2017.
Grâce en majeure partie aux efforts de notre communauté de contributeurs open source, EF 6.2 comprend de nombreux [correctifs de bogues](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) et [améliorations de produit](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Voici une courte liste des changements les plus importants du runtime EF 6.2 :

- Réduction du temps de démarrage en chargeant les modèles Code First terminés à partir d’un cache persistant [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- API Fluent pour définir des index [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() pour activer l’écriture de requêtes LINQ qui se traduisent en LIKE dans SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe prend désormais en charge l’option -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 peut maintenant utiliser des valeurs de clé générées par une séquence dans SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Liste de mise à jour des erreurs temporaires pour la stratégie d’exécution de SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Bogue : La réexécution des requêtes ou des commandes SQL échoue avec le message « SqlParameter est déjà contenu par un autre SqlParameterCollection » [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Bogue : L’évaluation de DbQuery.ToString() expire dans le débogueur [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Versions futures

Pour plus d’informations sur la prochaine version d’EF6, regardez notre [Feuille de route](roadmap.md).

## <a name="past-releases"></a>Versions précédentes

La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.
