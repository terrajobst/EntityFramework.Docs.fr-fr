---
title: Tester les composants à l’aide d’Entity Framework - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997835"
---
# <a name="testing"></a>Test

Vous pouvez tester les composants en simulant plus ou moins une connexion à la base de données réelle, sans la surcharge liée aux opérations d’E/S réelles de la base de données.

Pour ce faire, vous disposez de deux options :
 * Le [mode en mémoire SQLite](sqlite.md) vous permet d’écrire des tests efficaces par rapport à un fournisseur qui se comporte comme une base de données relationnelle.
 * Le [fournisseur InMemory](in-memory.md) est un fournisseur léger qui a des dépendances minimales, mais qui ne se comporte pas toujours comme une base de données relationnelle.
