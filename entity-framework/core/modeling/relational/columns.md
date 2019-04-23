---
title: Mappage de colonnes - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929860"
---
# <a name="column-mapping"></a><span data-ttu-id="e8b26-102">Mappage de colonnes</span><span class="sxs-lookup"><span data-stu-id="e8b26-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="e8b26-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="e8b26-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e8b26-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="e8b26-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e8b26-105">Mappage de colonne identifie les données de la colonne doivent être interrogées à partir d’et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e8b26-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e8b26-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="e8b26-106">Conventions</span></span>

<span data-ttu-id="e8b26-107">Par convention, chaque propriété sera être configurée pour mapper à une colonne portant le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="e8b26-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e8b26-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="e8b26-108">Data Annotations</span></span>

<span data-ttu-id="e8b26-109">Vous pouvez utiliser des Annotations de données pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="e8b26-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="e8b26-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e8b26-110">Fluent API</span></span>

<span data-ttu-id="e8b26-111">Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="e8b26-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
