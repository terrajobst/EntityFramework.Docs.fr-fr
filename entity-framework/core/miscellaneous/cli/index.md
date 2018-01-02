---
title: "Référence de la ligne de commande - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
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

Si votre projet cible un autre framework (par exemple, Windows universel ou Xamarin), nous vous recommandons de créer un projet .NET Standard distinct et d’effectuer un ciblage croisé d’un des frameworks pris en charge.

Pour le ciblage croisé de .NET Core, par exemple, cliquez avec le bouton droit sur le projet, puis sélectionnez **Modifier \*.csproj**. Mettez à jour la propriété `TargetFramework` comme suit. (Notez la forme plurielle du nom de la propriété.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Si vous utilisez une bibliothèque de classes .NET Standard, vous n’avez pas besoin d’effectuer un ciblage croisé si votre projet de démarrage cible .NET Framework ou .NET Core.

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
