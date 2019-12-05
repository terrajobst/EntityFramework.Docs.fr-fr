---
title: Contraintes de clé étrangère-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824581"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="af8c2-102">Contraintes de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="af8c2-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="af8c2-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="af8c2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="af8c2-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="af8c2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="af8c2-105">Une contrainte de clé étrangère est introduite pour chaque relation dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="af8c2-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="af8c2-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="af8c2-106">Conventions</span></span>

<span data-ttu-id="af8c2-107">Par Convention, les contraintes de clé étrangère sont nommées `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="af8c2-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="af8c2-108">Pour les clés étrangères composites `<foreign key property name>` devient une liste de noms de propriétés de clé étrangère séparés par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="af8c2-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="af8c2-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="af8c2-109">Data Annotations</span></span>

<span data-ttu-id="af8c2-110">Les noms de contrainte de clé étrangère ne peuvent pas être configurés à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="af8c2-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="af8c2-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="af8c2-111">Fluent API</span></span>

<span data-ttu-id="af8c2-112">Vous pouvez utiliser l’API Fluent pour configurer le nom de la contrainte de clé étrangère pour une relation.</span><span class="sxs-lookup"><span data-stu-id="af8c2-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
