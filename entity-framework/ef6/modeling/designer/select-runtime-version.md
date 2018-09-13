---
title: En sélectionnant la Version du Runtime Entity Framework pour les modèles de Concepteur EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488489"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="ae399-102">En sélectionnant la Version du Runtime Entity Framework pour les modèles de Concepteur EF</span><span class="sxs-lookup"><span data-stu-id="ae399-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="ae399-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ae399-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ae399-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="ae399-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ae399-105">L’écran suivant depuis EF6 a été ajoutée au Concepteur d’Entity Framework vous permettent de sélectionner la version du runtime à cibler lors de la création d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="ae399-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="ae399-106">L’écran s’affiche lorsque la dernière version d’Entity Framework n’est pas déjà installée dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ae399-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="ae399-107">Si la version la plus récente est déjà installée. il sera uniquement utilisé par défaut.</span><span class="sxs-lookup"><span data-stu-id="ae399-107">If the latest version is already installed it will just be used by default.</span></span>

![Écran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="ae399-109">Ciblage EF6.x</span><span class="sxs-lookup"><span data-stu-id="ae399-109">Targeting EF6.x</span></span>

<span data-ttu-id="ae399-110">Vous pouvez choisir d’EF6 à partir de l’écran « Choisir votre Version » pour ajouter le runtime EF6 à votre projet.</span><span class="sxs-lookup"><span data-stu-id="ae399-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="ae399-111">Une fois que vous avez ajouté EF6, vous allez cessent de se voir cet écran dans le projet actuel.</span><span class="sxs-lookup"><span data-stu-id="ae399-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="ae399-112">EF6 va être désactivé si vous avez déjà une version antérieure d’EF installé (étant donné que vous ne pouvez pas cibler plusieurs versions du runtime à partir du même projet).</span><span class="sxs-lookup"><span data-stu-id="ae399-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="ae399-113">Si l’option d’EF6 n’est pas activée ici, suivez ces étapes pour mettre à niveau votre projet EF6 :</span><span class="sxs-lookup"><span data-stu-id="ae399-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="ae399-114">Avec le bouton droit sur votre projet dans l’Explorateur de solutions et sélectionnez **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="ae399-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="ae399-115">Sélectionnez **mises à jour**</span><span class="sxs-lookup"><span data-stu-id="ae399-115">Select **Updates**</span></span>
3.  <span data-ttu-id="ae399-116">Sélectionnez **EntityFramework** (Assurez-vous qu’elle va mettre à jour vers la version souhaitée)</span><span class="sxs-lookup"><span data-stu-id="ae399-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="ae399-117">Cliquez sur **mise à jour**</span><span class="sxs-lookup"><span data-stu-id="ae399-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="ae399-118">Ciblage EF5.x</span><span class="sxs-lookup"><span data-stu-id="ae399-118">Targeting EF5.x</span></span>

<span data-ttu-id="ae399-119">Vous pouvez choisir d’EF5 à partir de l’écran « Choisir votre Version » pour ajouter le runtime EF5 à votre projet.</span><span class="sxs-lookup"><span data-stu-id="ae399-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="ae399-120">Une fois que vous avez ajouté EF5, vous continuerez à voir l’écran avec l’option EF6 désactivée.</span><span class="sxs-lookup"><span data-stu-id="ae399-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="ae399-121">Si vous avez une version EF4.x déjà installée du runtime vous verrez que la version d’EF répertorié dans l’écran, plutôt que dans EF5.</span><span class="sxs-lookup"><span data-stu-id="ae399-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="ae399-122">Dans ce cas, vous pouvez mettre à niveau vers EF5 en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="ae399-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="ae399-123">Sélectionnez **Tools -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**</span><span class="sxs-lookup"><span data-stu-id="ae399-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="ae399-124">Exécutez **Install-Package EntityFramework-version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="ae399-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="ae399-125">Ciblage EF4.x</span><span class="sxs-lookup"><span data-stu-id="ae399-125">Targeting EF4.x</span></span>

<span data-ttu-id="ae399-126">Vous pouvez installer le runtime EF4.x à votre projet en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="ae399-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="ae399-127">Sélectionnez **Tools -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**</span><span class="sxs-lookup"><span data-stu-id="ae399-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="ae399-128">Exécutez **Install-Package EntityFramework-version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="ae399-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
