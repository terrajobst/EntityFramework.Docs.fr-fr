---
title: Clés primaires-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656051"
---
# <a name="primary-keys"></a><span data-ttu-id="6a028-102">Clés primaires</span><span class="sxs-lookup"><span data-stu-id="6a028-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="6a028-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="6a028-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6a028-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="6a028-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6a028-105">Une contrainte de clé primaire est introduite pour la clé de chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6a028-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="6a028-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="6a028-106">Conventions</span></span>

<span data-ttu-id="6a028-107">Par Convention, la clé primaire de la base de données sera nommée `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="6a028-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6a028-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="6a028-108">Data Annotations</span></span>

<span data-ttu-id="6a028-109">Aucun aspect spécifique à la base de données relationnelle d’une clé primaire ne peut être configuré à l’aide des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="6a028-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6a028-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="6a028-110">Fluent API</span></span>

<span data-ttu-id="6a028-111">Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé primaire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6a028-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
