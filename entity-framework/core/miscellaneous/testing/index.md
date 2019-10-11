---
title: Tester des composants avec EF Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181307"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="8429d-102">Tests de composants avec EF Core</span><span class="sxs-lookup"><span data-stu-id="8429d-102">Testing components using EF Core</span></span>

<span data-ttu-id="8429d-103">Vous pouvez tester les composants en simulant plus ou moins une connexion à la base de données réelle, sans la surcharge liée aux opérations d’E/S réelles de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8429d-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="8429d-104">Pour ce faire, vous disposez de deux options :</span><span class="sxs-lookup"><span data-stu-id="8429d-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="8429d-105">Le [mode en mémoire SQLite](sqlite.md) vous permet d’écrire des tests efficaces par rapport à un fournisseur qui se comporte comme une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="8429d-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="8429d-106">Le [fournisseur InMemory](in-memory.md) est un fournisseur léger qui a des dépendances minimales, mais qui ne se comporte pas toujours comme une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="8429d-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
