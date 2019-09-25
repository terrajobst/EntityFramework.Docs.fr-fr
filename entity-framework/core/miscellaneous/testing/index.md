---
title: Tester des composants avec EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197494"
---
# <a name="testing-components-using-ef-core"></a>Tests de composants avec EF Core

Vous pouvez tester les composants en simulant plus ou moins une connexion à la base de données réelle, sans la surcharge liée aux opérations d’E/S réelles de la base de données.

Pour ce faire, vous disposez de deux options :
 * Le [mode en mémoire SQLite](sqlite.md) vous permet d’écrire des tests efficaces par rapport à un fournisseur qui se comporte comme une base de données relationnelle.
 * Le [fournisseur InMemory](in-memory.md) est un fournisseur léger qui a des dépendances minimales, mais qui ne se comporte pas toujours comme une base de données relationnelle.
