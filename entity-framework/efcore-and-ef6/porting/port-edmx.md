---
title: Portage de EF6 à EF Core - Porting an EDMX-Based Model - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416922"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="82d11-102">Portage d’un modèle EF6 basé sur EDMX à EF Core</span><span class="sxs-lookup"><span data-stu-id="82d11-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="82d11-103">EF Core ne prend pas en charge le format de fichier EDMX pour les modèles.</span><span class="sxs-lookup"><span data-stu-id="82d11-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="82d11-104">La meilleure option pour porter ces modèles, est de générer un nouveau modèle basé sur le code à partir de la base de données pour votre application.</span><span class="sxs-lookup"><span data-stu-id="82d11-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="82d11-105">Installer des paquets EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="82d11-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="82d11-106">Installez le package NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="82d11-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="82d11-107">Régénérer le modèle</span><span class="sxs-lookup"><span data-stu-id="82d11-107">Regenerate the model</span></span>

<span data-ttu-id="82d11-108">Vous pouvez maintenant utiliser la fonctionnalité de l’ingénieur inversé pour créer un modèle basé sur votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="82d11-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="82d11-109">Exécutez la commande suivante dans la console de gestionnaire de paquets (Outils > NuGet Package Manager - > console de gestionnaire de paquets).</span><span class="sxs-lookup"><span data-stu-id="82d11-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="82d11-110">Voir [La console De gestionnaire de paquet (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour les options de commande pour échafauder un sous-ensemble de tables, etc.</span><span class="sxs-lookup"><span data-stu-id="82d11-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="82d11-111">Par exemple, voici la commande d’échafaudage d’un modèle de la base de données Blogging sur votre instance SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="82d11-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="82d11-112">Supprimer le modèle EF6</span><span class="sxs-lookup"><span data-stu-id="82d11-112">Remove EF6 model</span></span>

<span data-ttu-id="82d11-113">Vous supprimeriez maintenant le modèle EF6 de votre application.</span><span class="sxs-lookup"><span data-stu-id="82d11-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="82d11-114">Il est bon de laisser le paquet EF6 NuGet (EntityFramework) installé, comme EF Core et EF6 peuvent être utilisés côte à côte dans la même application.</span><span class="sxs-lookup"><span data-stu-id="82d11-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="82d11-115">Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans tous les domaines de votre application, puis le désinstallation du paquet aidera à compiler des erreurs sur les morceaux de code qui nécessitent une attention particulière.</span><span class="sxs-lookup"><span data-stu-id="82d11-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="82d11-116">Mettre à jour votre code</span><span class="sxs-lookup"><span data-stu-id="82d11-116">Update your code</span></span>

<span data-ttu-id="82d11-117">À ce stade, il s’agit de traiter les erreurs de compilation et de revoir le code pour voir si les changements de comportement entre EF6 et EF Core auront un impact sur vous.</span><span class="sxs-lookup"><span data-stu-id="82d11-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="82d11-118">Tester le port</span><span class="sxs-lookup"><span data-stu-id="82d11-118">Test the port</span></span>

<span data-ttu-id="82d11-119">Ce n’est pas parce que votre application est compilée qu’elle est portée avec succès à EF Core.</span><span class="sxs-lookup"><span data-stu-id="82d11-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="82d11-120">Vous devrez tester tous les domaines de votre application pour vous assurer qu’aucun des changements de comportement n’a eu d’impact négatif sur votre application.</span><span class="sxs-lookup"><span data-stu-id="82d11-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
