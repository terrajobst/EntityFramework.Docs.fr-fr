---
title: Informations de référence sur les outils Entity Framework Core - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452252"
---
# <a name="entity-framework-core-tools-reference"></a>Informations de référence sur les outils Entity Framework Core

Les outils Entity Framework Core aident à effectuer les tâches de développement au moment du design. Ils servent principalement à gérer les migrations et à structurer un `DbContext` et des types d’entité en reconstituant la logique du schéma d’une base de données.

* Les [outils de la Console du Gestionnaire de package EF Core](powershell.md) s’exécutent dans la [Console du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console) dans Visual Studio. Ces outils fonctionnent avec les projets .NET Framework et .NET Core.

* Les [outils de l’interface de ligne de commande (CLI) EF Core .NET](dotnet.md) sont une extension multiplateforme des [outils CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/). Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).

Les deux outils exposent les mêmes fonctionnalités. Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la **Console du Gestionnaire de package**, car ils procurent une expérience plus intégrée.

## <a name="next-steps"></a>Étapes suivantes

* [Informations de référence sur les outils de la Console du Gestionnaire de package EF Core](powershell.md)
* [Informations de référence sur les outils CLI .NET EF Core](dotnet.md)
