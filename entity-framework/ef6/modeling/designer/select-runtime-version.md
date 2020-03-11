---
title: Sélection de Entity Framework version du runtime pour les modèles du concepteur EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418146"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="1f888-102">Sélection de Entity Framework version du runtime pour les modèles du concepteur EF</span><span class="sxs-lookup"><span data-stu-id="1f888-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="1f888-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1f888-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="1f888-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="1f888-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1f888-105">À partir de EF6, l’écran suivant a été ajouté au concepteur EF pour vous permettre de sélectionner la version du runtime que vous souhaitez cibler lors de la création d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="1f888-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="1f888-106">L’écran s’affiche lorsque la dernière version de Entity Framework n’est pas déjà installée dans le projet.</span><span class="sxs-lookup"><span data-stu-id="1f888-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="1f888-107">Si la version la plus récente est déjà installée, elle est utilisée par défaut.</span><span class="sxs-lookup"><span data-stu-id="1f888-107">If the latest version is already installed it will just be used by default.</span></span>

![Écran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="1f888-109">Ciblage de EF6. x</span><span class="sxs-lookup"><span data-stu-id="1f888-109">Targeting EF6.x</span></span>

<span data-ttu-id="1f888-110">Vous pouvez choisir EF6 dans l’écran « choisir votre version » pour ajouter le runtime EF6 à votre projet.</span><span class="sxs-lookup"><span data-stu-id="1f888-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="1f888-111">Une fois que vous avez ajouté EF6, vous ne voyez plus cet écran dans le projet actuel.</span><span class="sxs-lookup"><span data-stu-id="1f888-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="1f888-112">EF6 sera désactivé si vous avez déjà installé une version antérieure d’EF (puisque vous ne pouvez pas cibler plusieurs versions du runtime à partir du même projet).</span><span class="sxs-lookup"><span data-stu-id="1f888-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="1f888-113">Si l’option EF6 n’est pas activée ici, procédez comme suit pour mettre à niveau votre projet vers EF6 :</span><span class="sxs-lookup"><span data-stu-id="1f888-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="1f888-114">Dans Explorateur de solutions, cliquez avec le bouton droit sur votre projet, puis sélectionnez **gérer les packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="1f888-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="1f888-115">Sélectionner les **mises à jour**</span><span class="sxs-lookup"><span data-stu-id="1f888-115">Select **Updates**</span></span>
3.  <span data-ttu-id="1f888-116">Sélectionnez **EntityFramework** (Assurez-vous qu’il va le mettre à jour vers la version de votre choix)</span><span class="sxs-lookup"><span data-stu-id="1f888-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="1f888-117">Cliquez sur **Mettre à jour**</span><span class="sxs-lookup"><span data-stu-id="1f888-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="1f888-118">Ciblage de EF5. x</span><span class="sxs-lookup"><span data-stu-id="1f888-118">Targeting EF5.x</span></span>

<span data-ttu-id="1f888-119">Vous pouvez choisir EF5 dans l’écran « choisir votre version » pour ajouter le runtime EF5 à votre projet.</span><span class="sxs-lookup"><span data-stu-id="1f888-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="1f888-120">Une fois que vous avez ajouté EF5, vous voyez toujours l’écran avec l’option EF6 désactivée.</span><span class="sxs-lookup"><span data-stu-id="1f888-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="1f888-121">Si vous avez une version EF4. x du runtime déjà installée, vous verrez que la version d’EF est indiquée à l’écran plutôt que EF5.</span><span class="sxs-lookup"><span data-stu-id="1f888-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="1f888-122">Dans ce cas, vous pouvez effectuer la mise à niveau vers EF5 en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="1f888-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="1f888-123">Sélectionnez **Outils-gestionnaire de package de la bibliothèque&gt;-&gt; console du gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="1f888-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="1f888-124">Exécuter l' **Install-Package EntityFramework-version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="1f888-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="1f888-125">Ciblage de EF4. x</span><span class="sxs-lookup"><span data-stu-id="1f888-125">Targeting EF4.x</span></span>

<span data-ttu-id="1f888-126">Vous pouvez installer le runtime EF4. x dans votre projet en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="1f888-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="1f888-127">Sélectionnez **Outils-gestionnaire de package de la bibliothèque&gt;-&gt; console du gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="1f888-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="1f888-128">Exécuter l' **Install-Package EntityFramework-version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="1f888-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
