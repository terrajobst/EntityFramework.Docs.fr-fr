---
title: Héritage-EF Core
description: Comment configurer l’héritage de type d’entité à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502164"
---
# <a name="inheritance"></a><span data-ttu-id="f76e2-103">Héritage</span><span class="sxs-lookup"><span data-stu-id="f76e2-103">Inheritance</span></span>

<span data-ttu-id="f76e2-104">EF peut mapper une hiérarchie de types .NET à une base de données.</span><span class="sxs-lookup"><span data-stu-id="f76e2-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="f76e2-105">Cela vous permet d’écrire vos entités .NET dans le code comme d’habitude, à l’aide de types de base et dérivés, et de faire en sorte que EF crée en toute transparence le schéma de base de données approprié, les requêtes d’émission, etc. Les détails réels de la façon dont une hiérarchie de types est mappée dépendent du fournisseur ; Cette page décrit la prise en charge de l’héritage dans le contexte d’une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="f76e2-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="f76e2-106">Pour le moment, EF Core ne prend en charge que le modèle TPH (table par hiérarchie).</span><span class="sxs-lookup"><span data-stu-id="f76e2-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="f76e2-107">TPH utilise une table unique pour stocker les données de tous les types dans la hiérarchie, et une colonne de discriminateur est utilisée pour identifier le type représenté par chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="f76e2-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="f76e2-108">La table par type (TPT) et la table par type (TPC), qui sont prises en charge par EF6, ne sont pas encore prises en charge par EF Core.</span><span class="sxs-lookup"><span data-stu-id="f76e2-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="f76e2-109">TPT est une fonctionnalité majeure prévue pour EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="f76e2-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="f76e2-110">Mappage de la hiérarchie des types d’entités</span><span class="sxs-lookup"><span data-stu-id="f76e2-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="f76e2-111">Par Convention, EF ne configure que l’héritage si au moins deux types hérités sont explicitement inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="f76e2-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="f76e2-112">EF ne recherche pas automatiquement les types de base ou dérivés qui ne sont pas inclus dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="f76e2-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="f76e2-113">Vous pouvez inclure des types dans le modèle en exposant un DbSet pour chaque type dans la hiérarchie d’héritage :</span><span class="sxs-lookup"><span data-stu-id="f76e2-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="f76e2-114">Ce modèle est mappé au schéma de base de données suivant (Notez la colonne de *discriminateur* créée implicitement, qui identifie le type de *blog* stocké dans chaque ligne) :</span><span class="sxs-lookup"><span data-stu-id="f76e2-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="f76e2-116">Les colonnes de base de données deviennent automatiquement Nullable si nécessaire lors de l’utilisation du mappage TPH.</span><span class="sxs-lookup"><span data-stu-id="f76e2-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="f76e2-117">Par exemple, la colonne *RssUrl* accepte les valeurs NULL, car les instances de *blog* ordinaires n’ont pas cette propriété.</span><span class="sxs-lookup"><span data-stu-id="f76e2-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="f76e2-118">Si vous ne souhaitez pas exposer un DbSet pour une ou plusieurs entités dans la hiérarchie, vous pouvez également utiliser l’API Fluent pour vous assurer qu’elles sont incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="f76e2-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="f76e2-119">Si vous ne vous fiez pas aux conventions, vous pouvez spécifier explicitement le type de base à l’aide de `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="f76e2-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="f76e2-120">Vous pouvez également utiliser `.HasBaseType((Type)null)` pour supprimer un type d’entité de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="f76e2-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="f76e2-121">Configuration de discriminateur</span><span class="sxs-lookup"><span data-stu-id="f76e2-121">Discriminator configuration</span></span>

<span data-ttu-id="f76e2-122">Vous pouvez configurer le nom et le type de la colonne de discriminateur et les valeurs utilisées pour identifier chaque type dans la hiérarchie :</span><span class="sxs-lookup"><span data-stu-id="f76e2-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="f76e2-123">Dans les exemples ci-dessus, EF a ajouté le discriminateur implicitement en tant que [propriété Shadow](xref:core/modeling/shadow-properties) sur l’entité de base de la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="f76e2-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="f76e2-124">Cette propriété peut être configurée comme n’importe quelle autre :</span><span class="sxs-lookup"><span data-stu-id="f76e2-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="f76e2-125">Enfin, le discriminateur peut également être mappé à une propriété .NET normale dans votre entité :</span><span class="sxs-lookup"><span data-stu-id="f76e2-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="f76e2-126">Colonnes partagées</span><span class="sxs-lookup"><span data-stu-id="f76e2-126">Shared columns</span></span>

<span data-ttu-id="f76e2-127">Par défaut, lorsque deux types d’entités frères dans la hiérarchie ont une propriété portant le même nom, ils sont mappés à deux colonnes distinctes.</span><span class="sxs-lookup"><span data-stu-id="f76e2-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="f76e2-128">Toutefois, si leur type est identique, ils peuvent être mappés à la même colonne de base de données :</span><span class="sxs-lookup"><span data-stu-id="f76e2-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
