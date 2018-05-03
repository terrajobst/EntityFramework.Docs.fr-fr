---
title: Portage de EF6 vers Core EF - portage d’un modèle basé sur le fichier EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="f6e27-102">Portage d’un modèle EDMX EF6 vers EF Core</span><span class="sxs-lookup"><span data-stu-id="f6e27-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="f6e27-103">EF Core ne prend pas en charge le format de fichier EDMX pour les modèles.</span><span class="sxs-lookup"><span data-stu-id="f6e27-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="f6e27-104">La meilleure option pour ces modèles, le port doit générer un nouveau modèle basé sur le code à partir de la base de données pour votre application.</span><span class="sxs-lookup"><span data-stu-id="f6e27-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="f6e27-105">Installer les packages NuGet de noyaux EF</span><span class="sxs-lookup"><span data-stu-id="f6e27-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="f6e27-106">Installer le `Microsoft.EntityFrameworkCore.Tools` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6e27-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="f6e27-107">Régénérer le modèle</span><span class="sxs-lookup"><span data-stu-id="f6e27-107">Regenerate the model</span></span>

<span data-ttu-id="f6e27-108">Vous pouvez désormais utiliser les fonctionnalités de l’ingénierie à rebours pour créer un modèle basé sur votre base de données existante.</span><span class="sxs-lookup"><span data-stu-id="f6e27-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="f6e27-109">Exécutez la commande suivante dans la Console du Gestionnaire de Package (Outils -> Gestionnaire de Package NuGet -> Console du Gestionnaire de Package).</span><span class="sxs-lookup"><span data-stu-id="f6e27-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="f6e27-110">Consultez [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour structurer un sous-ensemble de tables, etc. des options de commande.</span><span class="sxs-lookup"><span data-stu-id="f6e27-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="f6e27-111">Par exemple, voici la commande pour structurer un modèle à partir de la base de données de création de blogs sur votre instance de base de données SQL Server locale.</span><span class="sxs-lookup"><span data-stu-id="f6e27-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="f6e27-112">Supprimer le modèle de EF6</span><span class="sxs-lookup"><span data-stu-id="f6e27-112">Remove EF6 model</span></span>

<span data-ttu-id="f6e27-113">Vous pouvez maintenant supprimer le modèle de EF6 à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="f6e27-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="f6e27-114">Il convient de laisser le package NuGet de EF6 (EntityFramework) est installé, comme EF Core et EF6 peuvent être utilisé côte à côte dans la même application.</span><span class="sxs-lookup"><span data-stu-id="f6e27-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="f6e27-115">Toutefois, si vous n’êtes pas l’intention d’utiliser des EF6 dans les zones de votre application, puis désinstaller le package vous permettent d’exercer des erreurs de compilation sur les segments de code nécessitant une attention particulière.</span><span class="sxs-lookup"><span data-stu-id="f6e27-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="f6e27-116">Mettre à jour votre code.</span><span class="sxs-lookup"><span data-stu-id="f6e27-116">Update your code</span></span>

<span data-ttu-id="f6e27-117">À ce stade, il est une question d’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement entre EF6 et EF Core auront un impact.</span><span class="sxs-lookup"><span data-stu-id="f6e27-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="f6e27-118">Le port de test</span><span class="sxs-lookup"><span data-stu-id="f6e27-118">Test the port</span></span>

<span data-ttu-id="f6e27-119">Le simple fait que votre application est compilé, ne signifie pas qu’il est correctement déplacée vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="f6e27-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="f6e27-120">Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont affectées votre application.</span><span class="sxs-lookup"><span data-stu-id="f6e27-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="f6e27-121">Consultez [mise en route avec EF de base sur ASP.NET Core avec une base de données](xref:core/get-started/aspnetcore/existing-db) pour une référence supplémentaire sur la façon de travailler avec une base de données existante</span><span class="sxs-lookup"><span data-stu-id="f6e27-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
