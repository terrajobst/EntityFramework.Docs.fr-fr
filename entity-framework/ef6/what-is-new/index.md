---
title: Nouveautés - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149128"
---
# <a name="whats-new-in-ef6"></a>Nouveautés dans EF6

Nous vous recommandons vivement d’utiliser la dernière version publiée d’Entity Framework pour bénéficier des dernières fonctionnalités et d’une stabilité optimale.
Toutefois, vous pouvez aussi avoir besoin d’utiliser une version antérieure ou vouloir tester les nouvelles améliorations de la dernière préversion.
Pour installer des versions spécifiques d’EF, consultez [Obtenir Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

Le runtime EF 6.3.0 a été publié sur NuGet en septembre 2019. L’objectif principal de cette version était de faciliter la migration des applications existantes qui utilisent EF 6 vers .NET Core 3.0. La communauté a également contribué à plusieurs correctifs de bogues et améliorations. Pour plus d’informations, consultez les problèmes fermés à chaque [jalon](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la version 6.3.0. Voici les plus connus :

- Prise en charge de .NET Core 3.0
  - Le package EntityFramework cible maintenant .NET Standard 2.1 en plus de .NET Framework 4.x
  - Les commandes de migrations ont été réécrites pour s’exécuter hors processus et fonctionner avec des projets de type SDK
- Prise en charge de SQL Server HierarchyId
- Compatibilité améliorée avec Roslyn et NuGet PackageReference
- Ajout de ef6.exe pour l’activation, l’ajout, l’écriture de scripts et l’application de migrations à partir d’assemblys. Cela remplace migrate.exe

## <a name="past-releases"></a>Versions précédentes

La page [Versions précédentes](past-releases.md) contient une archive de toutes les versions précédentes d’Entity Framework et des principales fonctionnalités introduites dans chaque version.
