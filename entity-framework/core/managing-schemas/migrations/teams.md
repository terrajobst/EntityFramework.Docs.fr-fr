---
title: "Migrations dans des environnements d’équipe - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="cd385-102">Migrations dans des environnements d’équipe</span><span class="sxs-lookup"><span data-stu-id="cd385-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="cd385-103">Lorsque vous utilisez Migrations dans les environnements d’équipe, une attention supplémentaire dans le fichier de capture instantanée de modèle.</span><span class="sxs-lookup"><span data-stu-id="cd385-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="cd385-104">Ce fichier peut vous indiquer si la migration de votre idéal fusionne correctement avec les vôtres de si vous avez besoin résoudre un conflit en recréant la migration avant de le partager.</span><span class="sxs-lookup"><span data-stu-id="cd385-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="cd385-105">fusion</span><span class="sxs-lookup"><span data-stu-id="cd385-105">Merging</span></span>
-------
<span data-ttu-id="cd385-106">Lorsque vous fusionnez des migrations à partir de vos coéquipiers, vous pouvez obtenir des conflits dans votre fichier de capture instantanée de modèle.</span><span class="sxs-lookup"><span data-stu-id="cd385-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="cd385-107">Si les deux modifications ne sont pas liées, la fusion est triviale, et les deux migrations peuvent coexister.</span><span class="sxs-lookup"><span data-stu-id="cd385-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="cd385-108">Par exemple, vous pouvez recevoir un conflit de fusion dans la configuration de type d’entité customer qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="cd385-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="cd385-109">Étant donné que ces deux propriétés doivent exister dans le modèle final, fin de la fusion en ajoutant les deux propriétés.</span><span class="sxs-lookup"><span data-stu-id="cd385-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="cd385-110">Dans de nombreux cas, votre système de contrôle de version peut automatiquement fusionner ces modifications pour vous.</span><span class="sxs-lookup"><span data-stu-id="cd385-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="cd385-111">Dans ces cas, la migration et la migration de votre idéal sont indépendants.</span><span class="sxs-lookup"><span data-stu-id="cd385-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="cd385-112">Étant donné qu’un d’eux peut être appliqué en premier, vous n’avez pas besoin apporter des modifications supplémentaires à la migration avant de le partager avec votre équipe.</span><span class="sxs-lookup"><span data-stu-id="cd385-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="cd385-113">Résolution des conflits</span><span class="sxs-lookup"><span data-stu-id="cd385-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="cd385-114">Parfois, vous rencontrez un conflit true lors de la fusion du modèle d’instantané.</span><span class="sxs-lookup"><span data-stu-id="cd385-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="cd385-115">Par exemple, vous et votre idéal chacun avez renommé la même propriété.</span><span class="sxs-lookup"><span data-stu-id="cd385-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="cd385-116">Si vous rencontrez ce genre de conflit, résolvez-le en recréant la migration.</span><span class="sxs-lookup"><span data-stu-id="cd385-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="cd385-117">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="cd385-117">Follow these steps:</span></span>

1. <span data-ttu-id="cd385-118">Abandon de la fusion et restauration dans votre répertoire de travail avant la fusion</span><span class="sxs-lookup"><span data-stu-id="cd385-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="cd385-119">Supprimer votre migration (en conservant vos modifications de modèle)</span><span class="sxs-lookup"><span data-stu-id="cd385-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="cd385-120">Fusionner les modifications de votre idéal dans votre répertoire de travail</span><span class="sxs-lookup"><span data-stu-id="cd385-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="cd385-121">Ajoutez de nouveau votre migration.</span><span class="sxs-lookup"><span data-stu-id="cd385-121">Re-add your migration</span></span>

<span data-ttu-id="cd385-122">Après cette opération, les deux migrations peuvent être appliquées dans l’ordre approprié.</span><span class="sxs-lookup"><span data-stu-id="cd385-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="cd385-123">Leur migration est appliquée en premier, modification du nom de la colonne à *Alias*, par la suite votre migration renomme à *nom d’utilisateur*.</span><span class="sxs-lookup"><span data-stu-id="cd385-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="cd385-124">Votre migration peut être partagée en toute sécurité avec le reste de l’équipe.</span><span class="sxs-lookup"><span data-stu-id="cd385-124">Your migration can safely be shared with the rest of the team.</span></span>
