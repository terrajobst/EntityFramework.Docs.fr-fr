---
title: Migrations dans les environnements d’équipe - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228378"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="2a98d-102">Migrations dans les environnements d’équipe</span><span class="sxs-lookup"><span data-stu-id="2a98d-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="2a98d-103">Lorsque vous travaillez avec des Migrations dans les environnements d’équipe, une attention supplémentaire dans le fichier de capture instantanée du modèle.</span><span class="sxs-lookup"><span data-stu-id="2a98d-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="2a98d-104">Ce fichier peut vous indiquer si la migration de votre coéquipier fusionne correctement avec les vôtres ou si vous avez besoin résoudre un conflit en recréant la migration avant de le partager.</span><span class="sxs-lookup"><span data-stu-id="2a98d-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="2a98d-105">fusion</span><span class="sxs-lookup"><span data-stu-id="2a98d-105">Merging</span></span>
-------
<span data-ttu-id="2a98d-106">Lorsque vous fusionnez des migrations à partir de vos collègues, vous pouvez obtenir des conflits dans votre fichier de capture instantanée du modèle.</span><span class="sxs-lookup"><span data-stu-id="2a98d-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="2a98d-107">Si les deux modifications ne sont pas liées, la fusion est simple et les deux migrations peuvent coexister.</span><span class="sxs-lookup"><span data-stu-id="2a98d-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="2a98d-108">Par exemple, vous pouvez recevoir un conflit de fusion dans la configuration de type d’entité customer qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2a98d-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="2a98d-109">Étant donné que ces deux propriétés doivent exister dans le modèle final, effectuer la fusion en ajoutant les deux propriétés.</span><span class="sxs-lookup"><span data-stu-id="2a98d-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="2a98d-110">Dans de nombreux cas, votre système de contrôle de version peut fusionner automatiquement ces modifications pour vous.</span><span class="sxs-lookup"><span data-stu-id="2a98d-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="2a98d-111">Dans ce cas, votre migration et la migration de votre coéquipier sont indépendants de l’autre.</span><span class="sxs-lookup"><span data-stu-id="2a98d-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="2a98d-112">Étant donné qu’un d’eux peut être appliqué en premier, vous n’avez pas besoin d’apporter des modifications supplémentaires à votre migration avant de le partager avec votre équipe.</span><span class="sxs-lookup"><span data-stu-id="2a98d-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="2a98d-113">Résolution des conflits</span><span class="sxs-lookup"><span data-stu-id="2a98d-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="2a98d-114">Parfois, vous rencontrez un conflit true lors de la fusion le modèle de capture instantanée du modèle.</span><span class="sxs-lookup"><span data-stu-id="2a98d-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="2a98d-115">Par exemple, vous et votre collègue chacun avez renommé la même propriété.</span><span class="sxs-lookup"><span data-stu-id="2a98d-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="2a98d-116">Si vous rencontrez ce type de conflit, résolvez-le en recréant votre migration.</span><span class="sxs-lookup"><span data-stu-id="2a98d-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="2a98d-117">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="2a98d-117">Follow these steps:</span></span>

1. <span data-ttu-id="2a98d-118">Abandonner la fusion et restauration dans votre répertoire de travail avant la fusion</span><span class="sxs-lookup"><span data-stu-id="2a98d-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="2a98d-119">Supprimer votre migration (mais conserver vos modifications de modèle)</span><span class="sxs-lookup"><span data-stu-id="2a98d-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="2a98d-120">Fusionner les modifications de votre coéquipier dans votre répertoire de travail</span><span class="sxs-lookup"><span data-stu-id="2a98d-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="2a98d-121">Ajoutez de nouveau votre migration.</span><span class="sxs-lookup"><span data-stu-id="2a98d-121">Re-add your migration</span></span>

<span data-ttu-id="2a98d-122">Après cela, les deux migrations peuvent être appliquées dans le bon ordre.</span><span class="sxs-lookup"><span data-stu-id="2a98d-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="2a98d-123">Leur migration est d’abord appliquée, modification du nom de la colonne à *Alias*, par la suite votre migration renomme à *nom d’utilisateur*.</span><span class="sxs-lookup"><span data-stu-id="2a98d-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="2a98d-124">Votre migration peut être partagée en toute sécurité avec le reste de l’équipe.</span><span class="sxs-lookup"><span data-stu-id="2a98d-124">Your migration can safely be shared with the rest of the team.</span></span>
