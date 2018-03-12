---
title: "Référence de la ligne de commande - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
<a name="entity-framework-core-tools"></a>Outils Entity Framework Core
===========================
Les outils Entity Framework Core vous permettent de développer des applications EF Core. Ils servent principalement à structurer un DbContext et des types d’entité en reconstituant la logique du schéma d’une base de données et à gérer les migrations.

Les [outils de la console du Gestionnaire de package EF Core][1] procurent une expérience de qualité supérieure dans Visual Studio. Exécutez-les dans la [console du Gestionnaire de package][2] de NuGet. Ces outils fonctionnent avec les projets .NET Framework et .NET Core.

Les [outils en ligne de commande .NET EF Core][3] sont une extension des [outils de l’interface de ligne de commande .NET Core][4] qui sont multiplateformes et peuvent s’exécuter en dehors de Visual Studio. Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).

Les deux outils exposent les mêmes fonctionnalités. Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la console du Gestionnaire de package, car ils procurent une expérience plus intégrée.

<a name="frameworks"></a>Frameworks
----------
Les outils prennent en charge les projets qui ciblent .NET Framework ou .NET Core.

Si vous voulez utiliser une bibliothèque de classes, envisagez d’utiliser si possible une bibliothèque de classes .NET Core ou .NET Framework. Ceci permet de rencontrer moins de problèmes avec les outils .NET. Si vous voulez utiliser à la place une bibliothèque de classes .NET Standard, vous devez utiliser un projet de démarrage qui cible .NET Framework ou .NET Core, pour que les outils aient une plateforme cible concrète dans laquelle ils peuvent charger votre bibliothèque de classes. Ce projet de démarrage peut être un projet factice sans code réel ; il est nécessaire seulement pour fournir une cible aux outils.

Si votre projet cible un autre framework (par exemple Windows universel ou Xamarin), vous devez créer une bibliothèque de classes .NET Standard distincte. Dans ce cas, suivez les instructions ci-dessus pour créer également un projet de démarrage qui peut être utilisé par les outils.

<a name="startup-and-target-projects"></a>Projets de démarrage et cible
---------------------------
Chaque fois que vous appelez une commande, deux projets sont impliqués : le projet cible et le projet de démarrage.

Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).

Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.

Le projet cible et le projet de démarrage peuvent être le même.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
