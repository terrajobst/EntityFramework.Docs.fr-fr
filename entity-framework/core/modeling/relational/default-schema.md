---
title: Schéma par défaut-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655966"
---
# <a name="default-schema"></a><span data-ttu-id="2de4d-102">Schéma par défaut</span><span class="sxs-lookup"><span data-stu-id="2de4d-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="2de4d-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="2de4d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2de4d-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="2de4d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2de4d-105">Le schéma par défaut est le schéma de base de données dans lequel les objets seront créés si un schéma n’est pas explicitement configuré pour cet objet.</span><span class="sxs-lookup"><span data-stu-id="2de4d-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="2de4d-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="2de4d-106">Conventions</span></span>

<span data-ttu-id="2de4d-107">Par Convention, le fournisseur de base de données choisit le schéma par défaut le plus approprié.</span><span class="sxs-lookup"><span data-stu-id="2de4d-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="2de4d-108">Par exemple, Microsoft SQL Server utilise le schéma `dbo` et SQLite n’utilise pas de schéma (puisque les schémas ne sont pas pris en charge dans SQLite).</span><span class="sxs-lookup"><span data-stu-id="2de4d-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2de4d-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="2de4d-109">Data Annotations</span></span>

<span data-ttu-id="2de4d-110">Vous ne pouvez pas définir le schéma par défaut à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="2de4d-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2de4d-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="2de4d-111">Fluent API</span></span>

<span data-ttu-id="2de4d-112">Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.</span><span class="sxs-lookup"><span data-stu-id="2de4d-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
