---
title: Héritage (base de données relationnelle)-EF Core
description: Comment configurer l’héritage de type d’entité dans une base de données relationnelle à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824747"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="a5766-103">Héritage (base de données relationnelle)</span><span class="sxs-lookup"><span data-stu-id="a5766-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="a5766-104">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="a5766-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a5766-105">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="a5766-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a5766-106">L’héritage dans le modèle EF est utilisé pour contrôler le mode de représentation de l’héritage dans les classes d’entité dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a5766-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="a5766-107">Actuellement, seul le modèle TPH (table par hiérarchie) est implémenté dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="a5766-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="a5766-108">D’autres modèles courants tels que table par type (TPT) et table par type (TPC) ne sont pas encore disponibles.</span><span class="sxs-lookup"><span data-stu-id="a5766-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="a5766-109">Conventions</span><span class="sxs-lookup"><span data-stu-id="a5766-109">Conventions</span></span>

<span data-ttu-id="a5766-110">Par défaut, l’héritage est mappé à l’aide du modèle TPH (table par hiérarchie).</span><span class="sxs-lookup"><span data-stu-id="a5766-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="a5766-111">TPH utilise une table unique pour stocker les données de tous les types dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="a5766-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="a5766-112">Une colonne de discriminateur est utilisée pour identifier le type représenté par chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="a5766-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="a5766-113">EF Core configure l’héritage uniquement si au moins deux types hérités sont explicitement inclus dans le modèle (pour plus d’informations, consultez [héritage](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="a5766-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="a5766-114">Voici un exemple qui illustre un scénario d’héritage simple et les données stockées dans une table de base de données relationnelle à l’aide du modèle TPH.</span><span class="sxs-lookup"><span data-stu-id="a5766-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="a5766-115">La colonne de *discriminateur* identifie le type de *blog* qui est stocké dans chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="a5766-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="a5766-117">Les colonnes de base de données deviennent automatiquement Nullable si nécessaire lors de l’utilisation du mappage TPH.</span><span class="sxs-lookup"><span data-stu-id="a5766-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a5766-118">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a5766-118">Data Annotations</span></span>

<span data-ttu-id="a5766-119">Vous ne pouvez pas utiliser des annotations de données pour configurer l’héritage.</span><span class="sxs-lookup"><span data-stu-id="a5766-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a5766-120">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a5766-120">Fluent API</span></span>

<span data-ttu-id="a5766-121">Vous pouvez utiliser l’API Fluent pour configurer le nom et le type de la colonne de discriminateur et les valeurs utilisées pour identifier chaque type dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="a5766-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="a5766-122">Configuration de la propriété de discriminateur</span><span class="sxs-lookup"><span data-stu-id="a5766-122">Configuring the discriminator property</span></span>

<span data-ttu-id="a5766-123">Dans les exemples ci-dessus, le discriminateur est créé en tant que [propriété Shadow](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="a5766-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="a5766-124">Étant donné qu’il s’agit d’une propriété dans le modèle, elle peut être configurée comme d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="a5766-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="a5766-125">Par exemple, pour définir la longueur maximale lorsque le discriminateur par défaut par convention est utilisé :</span><span class="sxs-lookup"><span data-stu-id="a5766-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="a5766-126">Le discriminateur peut également être mappé à une propriété .NET dans votre entité et le configurer.</span><span class="sxs-lookup"><span data-stu-id="a5766-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="a5766-127">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a5766-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="a5766-128">Colonnes partagées</span><span class="sxs-lookup"><span data-stu-id="a5766-128">Shared columns</span></span>

<span data-ttu-id="a5766-129">Lorsque deux types d’entités frères ont une propriété portant le même nom, ils sont mappés à deux colonnes distinctes par défaut.</span><span class="sxs-lookup"><span data-stu-id="a5766-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="a5766-130">Mais s’ils sont compatibles, ils peuvent être mappés à la même colonne :</span><span class="sxs-lookup"><span data-stu-id="a5766-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]