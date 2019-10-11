---
title: Portage à partir de EF6 vers EF Core-Portage d’un modèle basé sur EDMX-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182065"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="242f2-102">Portage d’un modèle basé sur EF6 EDMX vers EF Core</span><span class="sxs-lookup"><span data-stu-id="242f2-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="242f2-103">EF Core ne prend pas en charge le format de fichier EDMX pour les modèles.</span><span class="sxs-lookup"><span data-stu-id="242f2-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="242f2-104">La meilleure option pour porter ces modèles consiste à générer un nouveau modèle basé sur du code à partir de la base de données de votre application.</span><span class="sxs-lookup"><span data-stu-id="242f2-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="242f2-105">Installer EF Core des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="242f2-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="242f2-106">Installez le package NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="242f2-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="242f2-107">Régénérer le modèle</span><span class="sxs-lookup"><span data-stu-id="242f2-107">Regenerate the model</span></span>

<span data-ttu-id="242f2-108">Vous pouvez maintenant utiliser la fonctionnalité d’ingénierie à rebours pour créer un modèle basé sur votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="242f2-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="242f2-109">Exécutez la commande suivante dans la console du gestionnaire de package (outils – > Gestionnaire de package NuGet – > console du gestionnaire de package).</span><span class="sxs-lookup"><span data-stu-id="242f2-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="242f2-110">Consultez [console du gestionnaire de package (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour obtenir des options de commande permettant d’effectuer une génération de modèles automatique d’un sous-ensemble de tables.</span><span class="sxs-lookup"><span data-stu-id="242f2-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="242f2-111">Par exemple, voici la commande pour générer un modèle de structure à partir de la base de données de blogs sur votre instance SQL Server de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="242f2-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="242f2-112">Supprimer le modèle EF6</span><span class="sxs-lookup"><span data-stu-id="242f2-112">Remove EF6 model</span></span>

<span data-ttu-id="242f2-113">Vous pouvez maintenant supprimer le modèle EF6 de votre application.</span><span class="sxs-lookup"><span data-stu-id="242f2-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="242f2-114">Il est parfait de conserver le package NuGet EF6 (EntityFramework) installé, car EF Core et EF6 peuvent être utilisés côte à côte dans la même application.</span><span class="sxs-lookup"><span data-stu-id="242f2-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="242f2-115">Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans les zones de votre application, la désinstallation du package permet d’obtenir des erreurs de compilation sur des portions de code nécessitant votre attention.</span><span class="sxs-lookup"><span data-stu-id="242f2-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="242f2-116">Mettre à jour votre code</span><span class="sxs-lookup"><span data-stu-id="242f2-116">Update your code</span></span>

<span data-ttu-id="242f2-117">À ce stade, il s’agit de traiter les erreurs de compilation et de consulter le code pour voir si les changements de comportement entre EF6 et EF Core ont un impact sur vous.</span><span class="sxs-lookup"><span data-stu-id="242f2-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="242f2-118">Tester le port</span><span class="sxs-lookup"><span data-stu-id="242f2-118">Test the port</span></span>

<span data-ttu-id="242f2-119">Simplement parce que votre application est compilée, ne signifie pas qu’elle est correctement reportée vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="242f2-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="242f2-120">Vous devrez tester toutes les zones de votre application pour vous assurer qu’aucune des modifications de comportement n’a un impact négatif sur votre application.</span><span class="sxs-lookup"><span data-stu-id="242f2-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
