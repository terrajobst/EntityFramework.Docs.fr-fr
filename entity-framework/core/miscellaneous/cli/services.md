---
title: Services au moment du design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416727"
---
# <a name="design-time-services"></a><span data-ttu-id="8cf7a-102">Services au moment du design</span><span class="sxs-lookup"><span data-stu-id="8cf7a-102">Design-time services</span></span>

<span data-ttu-id="8cf7a-103">Certains services utilisés par les outils sont utilisés uniquement au moment du Design.</span><span class="sxs-lookup"><span data-stu-id="8cf7a-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="8cf7a-104">Ces services sont gérés séparément des services d’exécution de EF Core pour les empêcher d’être déployés avec votre application.</span><span class="sxs-lookup"><span data-stu-id="8cf7a-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="8cf7a-105">Pour remplacer l’un de ces services (par exemple, le service pour générer des fichiers de migration), ajoutez une implémentation de `IDesignTimeServices` à votre projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="8cf7a-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
