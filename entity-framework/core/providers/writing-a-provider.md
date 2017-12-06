---
title: "L’écriture d’un fournisseur de base de données - EF Core"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="2ef4f-102">Écriture d’un fournisseur de base de données</span><span class="sxs-lookup"><span data-stu-id="2ef4f-102">Writing a Database Provider</span></span>

<span data-ttu-id="2ef4f-103">Pour plus d’informations sur l’écriture d’un fournisseur de base de données Entity Framework Core, consultez [, vous pouvez écrire un fournisseur EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) par [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="2ef4f-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="2ef4f-104">La base de code EF Core est open source et contient plusieurs fournisseurs de base de données qui peuvent être utilisés en tant que référence.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="2ef4f-105">Vous trouverez le code source à https://github.com/aspnet/EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="2ef4f-106">Les fournisseurs-Méfiez-vous de l’étiquette</span><span class="sxs-lookup"><span data-stu-id="2ef4f-106">The providers-beware label</span></span>

<span data-ttu-id="2ef4f-107">Une fois que vous commencez à travailler sur un fournisseur, observer les [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) étiquette sur nos problèmes de GitHub et les requêtes d’extraction.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="2ef4f-108">Nous utilisons cette étiquette pour identifier les modifications qui peuvent avoir un impact sur les rédacteurs de fournisseur.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="2ef4f-109">Suggéré d’affectation de noms de fournisseurs tiers</span><span class="sxs-lookup"><span data-stu-id="2ef4f-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="2ef4f-110">Nous suggérons d’utiliser la dénomination suivant pour les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="2ef4f-111">Cela est cohérent avec les noms des packages fournis par l’équipe EF Core.</span><span class="sxs-lookup"><span data-stu-id="2ef4f-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="2ef4f-112">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2ef4f-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
