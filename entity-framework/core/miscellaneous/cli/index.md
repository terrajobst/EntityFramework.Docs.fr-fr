---
title: Informations de référence sur les outils Entity Framework Core - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447116"
---
# <a name="entity-framework-core-tools-reference"></a>Informations de référence sur les outils Entity Framework Core

Les outils Entity Framework Core aident à effectuer les tâches de développement au moment du design. Ils servent principalement à gérer les migrations et à structurer un `DbContext` et des types d’entité en reconstituant la logique du schéma d’une base de données.

* Les [outils de la Console du Gestionnaire de package EF Core](powershell.md)) s’exécutent dans la [Console du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console) dans Visual Studio. Ces outils fonctionnent avec les projets .NET Framework et .NET Core.

* Les [outils de l’interface de ligne de commande (CLI) EF Core .NET](dotnet.md) sont une extension multiplateforme des [outils CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/). Ces outils nécessitent un projet de SDK .NET Core (dont le fichier projet contient `Sdk="Microsoft.NET.Sdk"` ou une ligne similaire).

Les deux outils exposent les mêmes fonctionnalités. Si vous développez dans Visual Studio, nous vous recommandons d’utiliser les outils de la **Console du Gestionnaire de package**, car ils procurent une expérience plus intégrée.

## <a name="next-steps"></a>Étapes suivantes

* [Informations de référence sur les outils de la Console du Gestionnaire de package EF Core](powershell.md)
* [Informations de référence sur les outils CLI .NET EF Core](dotnet.md)