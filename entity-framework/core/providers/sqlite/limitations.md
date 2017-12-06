---
title: "Base de données SQLite fournisseur - Limitations - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="c3ec7-102">Limites du fournisseur de base de données SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="c3ec7-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="c3ec7-103">Le fournisseur de SQLite a un nombre de limitations des migrations.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="c3ec7-104">La plupart de ces limitations est le résultat des limitations dans le moteur de base de données SQLite sous-jacent et n’est pas spécifique à EF.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="c3ec7-105">Limitations de modélisation</span><span class="sxs-lookup"><span data-stu-id="c3ec7-105">Modeling limitations</span></span>

<span data-ttu-id="c3ec7-106">La bibliothèque de relationnelle commune (partagée par les fournisseurs de base de données relationnelle d’Entity Framework) définit des API pour la modélisation des concepts qui sont communes à la plupart des moteurs de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="c3ec7-107">Plusieurs de ces concepts ne sont pas pris en charge par le fournisseur de SQLite.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="c3ec7-108">Schémas</span><span class="sxs-lookup"><span data-stu-id="c3ec7-108">Schemas</span></span>
* <span data-ttu-id="c3ec7-109">Séquences</span><span class="sxs-lookup"><span data-stu-id="c3ec7-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="c3ec7-110">Limitations de migration</span><span class="sxs-lookup"><span data-stu-id="c3ec7-110">Migrations limitations</span></span>

<span data-ttu-id="c3ec7-111">Le moteur de base de données SQLite ne prend pas en charge un nombre d’opérations de schéma qui sont pris en charge par la plupart des autres bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="c3ec7-112">Si vous essayez d’appliquer l’une des opérations non prises en charge pour une base de données SQLite un `NotSupportedException` sera levée.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="c3ec7-113">Opération</span><span class="sxs-lookup"><span data-stu-id="c3ec7-113">Operation</span></span>            | <span data-ttu-id="c3ec7-114">Prise en charge ?</span><span class="sxs-lookup"><span data-stu-id="c3ec7-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="c3ec7-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="c3ec7-115">AddColumn</span></span>            | <span data-ttu-id="c3ec7-116">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-116">✔</span></span>          |
| <span data-ttu-id="c3ec7-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="c3ec7-117">AddForeignKey</span></span>        | <span data-ttu-id="c3ec7-118">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-118">✗</span></span>          |
| <span data-ttu-id="c3ec7-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="c3ec7-119">AddPrimaryKey</span></span>        | <span data-ttu-id="c3ec7-120">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-120">✗</span></span>          |
| <span data-ttu-id="c3ec7-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="c3ec7-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="c3ec7-122">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-122">✗</span></span>          |
| <span data-ttu-id="c3ec7-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="c3ec7-123">AlterColumn</span></span>          | <span data-ttu-id="c3ec7-124">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-124">✗</span></span>          |
| <span data-ttu-id="c3ec7-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="c3ec7-125">CreateIndex</span></span>          | <span data-ttu-id="c3ec7-126">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-126">✔</span></span>          |
| <span data-ttu-id="c3ec7-127">Create table</span><span class="sxs-lookup"><span data-stu-id="c3ec7-127">CreateTable</span></span>          | <span data-ttu-id="c3ec7-128">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-128">✔</span></span>          |
| <span data-ttu-id="c3ec7-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="c3ec7-129">DropColumn</span></span>           | <span data-ttu-id="c3ec7-130">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-130">✗</span></span>          |
| <span data-ttu-id="c3ec7-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="c3ec7-131">DropForeignKey</span></span>       | <span data-ttu-id="c3ec7-132">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-132">✗</span></span>          |
| <span data-ttu-id="c3ec7-133">DROP index</span><span class="sxs-lookup"><span data-stu-id="c3ec7-133">DropIndex</span></span>            | <span data-ttu-id="c3ec7-134">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-134">✔</span></span>          |
| <span data-ttu-id="c3ec7-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="c3ec7-135">DropPrimaryKey</span></span>       | <span data-ttu-id="c3ec7-136">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-136">✗</span></span>          |
| <span data-ttu-id="c3ec7-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="c3ec7-137">DropTable</span></span>            | <span data-ttu-id="c3ec7-138">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-138">✔</span></span>          |
| <span data-ttu-id="c3ec7-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="c3ec7-139">DropUniqueConstraint</span></span> | <span data-ttu-id="c3ec7-140">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-140">✗</span></span>          |
| <span data-ttu-id="c3ec7-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="c3ec7-141">RenameColumn</span></span>         | <span data-ttu-id="c3ec7-142">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-142">✗</span></span>          |
| <span data-ttu-id="c3ec7-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="c3ec7-143">RenameIndex</span></span>          | <span data-ttu-id="c3ec7-144">✗</span><span class="sxs-lookup"><span data-stu-id="c3ec7-144">✗</span></span>          |
| <span data-ttu-id="c3ec7-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="c3ec7-145">RenameTable</span></span>          | <span data-ttu-id="c3ec7-146">✔</span><span class="sxs-lookup"><span data-stu-id="c3ec7-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="c3ec7-147">Solution de contournement migrations limitations</span><span class="sxs-lookup"><span data-stu-id="c3ec7-147">Migrations limitations workaround</span></span>

<span data-ttu-id="c3ec7-148">Vous pouvez contourner certaines de ces limitations en écrivant du code dans vos migrations pour effectuer une table manuellement régénérer.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="c3ec7-149">Une régénération de la table implique la modification du nom de la table existante, créez une table, copie de données vers la nouvelle table et la suppression de l’ancienne table.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="c3ec7-150">Vous devez utiliser le `Sql(string)` méthode pour effectuer certaines de ces étapes.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="c3ec7-151">Consultez [rendre autres types de Table de modifications de schéma](http://sqlite.org/lang_altertable.html#otheralter) dans la documentation de SQLite pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="c3ec7-152">À l’avenir, EF peut prendre en charge certaines de ces opérations à l’aide de l’approche de reconstruction de table en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="c3ec7-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="c3ec7-153">Vous pouvez [suivre cette fonctionnalité sur notre projet GitHub](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="c3ec7-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
