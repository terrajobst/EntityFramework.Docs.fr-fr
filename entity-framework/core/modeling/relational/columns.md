---
title: Mappage de colonnes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197213"
---
# <a name="column-mapping"></a><span data-ttu-id="ac98b-102">Mappage de colonnes</span><span class="sxs-lookup"><span data-stu-id="ac98b-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="ac98b-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="ac98b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ac98b-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="ac98b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ac98b-105">Le mappage de colonnes identifie les données de colonne à partir desquelles les données doivent être interrogées et enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="ac98b-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ac98b-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="ac98b-106">Conventions</span></span>

<span data-ttu-id="ac98b-107">Par Convention, chaque propriété est configurée pour être mappée à une colonne portant le même nom que la propriété.</span><span class="sxs-lookup"><span data-stu-id="ac98b-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ac98b-108">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="ac98b-108">Data Annotations</span></span>

<span data-ttu-id="ac98b-109">Vous pouvez utiliser des annotations de données pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="ac98b-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="ac98b-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="ac98b-110">Fluent API</span></span>

<span data-ttu-id="ac98b-111">Vous pouvez utiliser l’API Fluent pour configurer la colonne à laquelle une propriété est mappée.</span><span class="sxs-lookup"><span data-stu-id="ac98b-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
