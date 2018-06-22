---
title: Tester les composants à l’aide d’Entity Framework - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048861"
---
# <a name="testing"></a>Test

Vous pouvez tester les composants en simulant plus ou moins une connexion à la base de données réelle, sans la surcharge liée aux opérations d’E/S réelles de la base de données.

Pour ce faire, vous disposez de deux options :
 * Le [mode en mémoire SQLite](sqlite.md) vous permet d’écrire des tests efficaces par rapport à un fournisseur qui se comporte comme une base de données relationnelle.
 * Le [fournisseur InMemory](in-memory.md) est un fournisseur léger qui a des dépendances minimales, mais qui ne se comporte pas toujours comme une base de données relationnelle.
