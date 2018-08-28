---
title: Installation d’EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996926"
---
# <a name="installing-ef-core"></a><span data-ttu-id="a02c0-102">Installation d’EF Core</span><span class="sxs-lookup"><span data-stu-id="a02c0-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a02c0-103">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a02c0-103">Prerequisites</span></span>

<span data-ttu-id="a02c0-104">Pour développer des applications .NET Core 2.1 (y compris des applications ASP.NET Core 2.1 qui ciblent .NET Core), vous devez télécharger et installer une version du [SDK .NET Core 2.1](https://www.microsoft.com/net/download/core) qui convient à votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="a02c0-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="a02c0-105">**Cela est vrai même si vous avez installé Visual Studio 2017 version 15.7.**</span><span class="sxs-lookup"><span data-stu-id="a02c0-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="a02c0-106">Pour utiliser EF Core 2.1 ou toute autre bibliothèque .NET Standard 2.0 avec une plateforme .NET en plus de .NET Core 2.1 (par exemple, avec .NET Framework 4.6.1 ou version ultérieure), vous avez besoin d’une version de NuGet qui prend en charge .NET Standard 2.0 et ses frameworks compatibles.</span><span class="sxs-lookup"><span data-stu-id="a02c0-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="a02c0-107">Voici quelques façons de procéder :</span><span class="sxs-lookup"><span data-stu-id="a02c0-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="a02c0-108">Installer Visual Studio 2017 version 15.7</span><span class="sxs-lookup"><span data-stu-id="a02c0-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="a02c0-109">Si vous utilisez Visual Studio 2015, [téléchargez et mettez à niveau le client NuGet vers la version 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="a02c0-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="a02c0-110">Les projets créés avec des versions précédentes de Visual Studio et ciblant .NET Framework peuvent nécessiter des modifications supplémentaires pour être compatibles avec les bibliothèques .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="a02c0-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="a02c0-111">Modifiez le fichier projet et vérifiez que l’entrée suivante apparaît dans le groupe de propriétés initiales :</span><span class="sxs-lookup"><span data-stu-id="a02c0-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="a02c0-112">Pour les projets de test, vérifiez également que l’entrée suivante est présente :</span><span class="sxs-lookup"><span data-stu-id="a02c0-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="a02c0-113">Obtention des bits</span><span class="sxs-lookup"><span data-stu-id="a02c0-113">Getting the bits</span></span>
<span data-ttu-id="a02c0-114">La méthode recommandée pour ajouter des bibliothèques runtime EF Core à une application consiste à installer un fournisseur de base de données EF Core à partir de NuGet.</span><span class="sxs-lookup"><span data-stu-id="a02c0-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="a02c0-115">Outre les bibliothèques runtime, vous pouvez installer des outils qui facilitent la réalisation de plusieurs tâches EF Core dans votre projet au moment de la conception, telles que la création et l’application de migrations et la création d’un modèle basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="a02c0-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="a02c0-116">Si vous devez mettre à jour une application qui utilise un fournisseur de base de données tiers, recherchez toujours une mise à jour du fournisseur qui est compatible avec la version d’EF Core à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a02c0-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="a02c0-117">Par exemple, les fournisseurs de base de données pour les versions antérieures ne sont pas compatibles avec la version 2.1 du runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="a02c0-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="a02c0-118">Les applications ciblant ASP.NET Core 2.1 peuvent utiliser EF Core 2.1 sans dépendances supplémentaires en plus des fournisseurs de base de données tiers.</span><span class="sxs-lookup"><span data-stu-id="a02c0-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="a02c0-119">Les applications ciblant des versions antérieures d’ASP.NET Core doivent être mises à niveau vers ASP.NET Core 2.1 pour utiliser EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a02c0-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="a02c0-120">Développement multiplateforme à l’aide de l’interface de ligne de commande de base (CLI) .NET Core</span><span class="sxs-lookup"><span data-stu-id="a02c0-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="a02c0-121">Pour développer des applications qui ciblent [.NET Core](https://www.microsoft.com/net/download/core), vous pouvez choisir d’utiliser les [commandes CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) en combinaison avec votre éditeur de texte, ou bien un environnement de développement intégré (IDE) tel que Visual Studio, Visual Studio pour Mac ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a02c0-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="a02c0-122">Les applications qui ciblent .NET Core nécessitent des versions spécifiques de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a02c0-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="a02c0-123">Par exemple, le développement .NET Core 1.x nécessite Visual Studio 2017, tandis que le développement .NET Core 2.1 nécessite Visual Studio 2017 version 15.7.</span><span class="sxs-lookup"><span data-stu-id="a02c0-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="a02c0-124">Pour installer ou mettre à niveau le fournisseur SQL Server dans une application .NET Core multiplateforme, basculez vers le répertoire de l’application et exécutez la commande suivante dans une ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="a02c0-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="a02c0-125">Vous pouvez indiquer une installation de version spécifique dans la commande `dotnet add package`, à l’aide du modificateur `-v`.</span><span class="sxs-lookup"><span data-stu-id="a02c0-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="a02c0-126">Par exemple, pour installer des packages EF Core 2.1, ajoutez `-v 2.1.0` à la commande.</span><span class="sxs-lookup"><span data-stu-id="a02c0-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="a02c0-127">EF Core comprend un ensemble de [commandes supplémentaires pour l’interface CLI `dotnet`](../../miscellaneous/cli/dotnet.md), commençant par `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="a02c0-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="a02c0-128">Les outils CLI .NET Core pour EF Core nécessitent un package appelé `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="a02c0-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="a02c0-129">Vous pouvez l’ajouter au projet à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a02c0-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="a02c0-130">Utilisez toujours la version des packages d’outils qui correspond à la version principale des packages du runtime.</span><span class="sxs-lookup"><span data-stu-id="a02c0-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="a02c0-131">Développement Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a02c0-131">Visual Studio development</span></span>

<span data-ttu-id="a02c0-132">Vous pouvez développer de nombreux types d’applications différents qui ciblent .NET Core, .NET Framework ou d’autres plateformes prises en charge par EF Core à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a02c0-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="a02c0-133">Vous pouvez installer un fournisseur de base de données EF Core dans votre application à partir de Visual Studio de deux façons :</span><span class="sxs-lookup"><span data-stu-id="a02c0-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="a02c0-134">Utilisation de [l’interface utilisateur du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-ui) de NuGet</span><span class="sxs-lookup"><span data-stu-id="a02c0-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="a02c0-135">Sélectionnez le menu **Projet > Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a02c0-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="a02c0-136">Cliquez sur l’onglet **Parcourir** ou **Mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="a02c0-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="a02c0-137">Sélectionnez le package `Microsoft.EntityFrameworkCore.SqlServer` et la version souhaitée, puis confirmez.</span><span class="sxs-lookup"><span data-stu-id="a02c0-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="a02c0-138">Utilisation de la [console du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console) de NuGet.</span><span class="sxs-lookup"><span data-stu-id="a02c0-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="a02c0-139">Sélectionnez le menu **Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a02c0-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="a02c0-140">Tapez et exécutez la commande suivante dans la console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="a02c0-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="a02c0-141">Vous pouvez utiliser la commande `Update-Package` à la place pour mettre à jour un package qui est déjà installé dans une version plus récente.</span><span class="sxs-lookup"><span data-stu-id="a02c0-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="a02c0-142">Pour spécifier une version spécifique, vous pouvez utiliser le modificateur `-Version`.</span><span class="sxs-lookup"><span data-stu-id="a02c0-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="a02c0-143">Par exemple, pour installer des packages EF Core 2.1, ajoutez `-Version 2.1.0` aux commandes.</span><span class="sxs-lookup"><span data-stu-id="a02c0-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="a02c0-144">Outils</span><span class="sxs-lookup"><span data-stu-id="a02c0-144">Tools</span></span>

<span data-ttu-id="a02c0-145">Il existe également dans Visual Studio une version de PowerShell des [commandes EF Core qui s’exécutent dans la console du Gestionnaire de package](../../miscellaneous/cli/powershell.md), avec des fonctionnalités similaires aux commandes `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="a02c0-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="a02c0-146">Bien que vous puissiez utiliser les commandes `dotnet ef` à partir de la console du Gestionnaire de package dans Visual Studio, il est beaucoup plus pratique d’utiliser la version PowerShell :</span><span class="sxs-lookup"><span data-stu-id="a02c0-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="a02c0-147">Elles fonctionnent automatiquement avec le projet sélectionné dans la console du Gestionnaire de package sans que vous ayez besoin de basculer manuellement entre les répertoires.</span><span class="sxs-lookup"><span data-stu-id="a02c0-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="a02c0-148">Elles ouvrent automatiquement les fichiers générés par les commandes de Visual Studio après avoir été exécutées.</span><span class="sxs-lookup"><span data-stu-id="a02c0-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="a02c0-149">**Packages dépréciés dans EF Core 2.1 :** si vous mettez à niveau une application existante vers EF Core 2.1, vous pouvez être amené à supprimer manuellement des références à des packages EF Core plus anciens :</span><span class="sxs-lookup"><span data-stu-id="a02c0-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="a02c0-150">Les packages design-time des fournisseurs de base de données tels que `Microsoft.EntityFrameworkCore.SqlServer.Design` ne sont plus requis ou pris en charge dans EF Core 2.1, mais ils ne sont pas automatiquement supprimés durant la mise à niveau des autres packages.</span><span class="sxs-lookup"><span data-stu-id="a02c0-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="a02c0-151">Les outils CLI .NET étant désormais inclus dans le Kit de développement logiciel (SDK) .NET, la référence à ce package peut être supprimée du fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="a02c0-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
