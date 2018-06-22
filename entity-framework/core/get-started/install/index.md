---
title: Installation d’EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049241"
---
# <a name="installing-ef-core"></a><span data-ttu-id="eb825-102">Installation d’EF Core</span><span class="sxs-lookup"><span data-stu-id="eb825-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb825-103">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="eb825-103">Prerequisites</span></span>

<span data-ttu-id="eb825-104">Pour développer des applications .NET Core 2.0 (y compris des applications ASP.NET Core 2.0 qui ciblent .NET Core), vous devez télécharger et installer une version du [SDK .NET Core 2.0](https://www.microsoft.com/net/download/core) qui convient à votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="eb825-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="eb825-105">**Cela est vrai même si vous avez installé Visual Studio 2017 version 15.3.**</span><span class="sxs-lookup"><span data-stu-id="eb825-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="eb825-106">Pour utiliser EF Core 2.0 ou toute autre bibliothèque .NET Standard 2.0 avec une plateforme .NET en plus de .NET Core 2.0 (par exemple, avec .NET Framework 4.6.1 ou version supérieure), vous avez besoin d’une version de NuGet qui prend en charge .NET Standard 2.0 et ses frameworks compatibles.</span><span class="sxs-lookup"><span data-stu-id="eb825-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="eb825-107">Voici quelques façons de procéder :</span><span class="sxs-lookup"><span data-stu-id="eb825-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="eb825-108">Installez Visual Studio 2017 version 15.3.</span><span class="sxs-lookup"><span data-stu-id="eb825-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="eb825-109">Si vous utilisez Visual Studio 2015, [téléchargez et mettez à niveau le client NuGet vers la version 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="eb825-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="eb825-110">Les projets créés avec des versions précédentes de Visual Studio et ciblant .NET Framework peuvent nécessiter des modifications supplémentaires pour être compatibles avec les bibliothèques .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="eb825-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="eb825-111">Modifiez le fichier projet et vérifiez que l’entrée suivante apparaît dans le groupe de propriétés initiales :</span><span class="sxs-lookup"><span data-stu-id="eb825-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="eb825-112">Pour les projets de test, vérifiez également que l’entrée suivante est présente :</span><span class="sxs-lookup"><span data-stu-id="eb825-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="eb825-113">Obtention des bits</span><span class="sxs-lookup"><span data-stu-id="eb825-113">Getting the bits</span></span>
<span data-ttu-id="eb825-114">La méthode recommandée pour ajouter des bibliothèques runtime EF Core à une application consiste à installer un fournisseur de base de données EF Core à partir de NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb825-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="eb825-115">Outre les bibliothèques runtime, vous pouvez installer des outils qui facilitent la réalisation de plusieurs tâches EF Core dans votre projet au moment de la conception, telles que la création et l’application de migrations et la création d’un modèle basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="eb825-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="eb825-116">Si vous devez mettre à jour une application qui utilise un fournisseur de base de données tiers, recherchez toujours une mise à jour du fournisseur qui est compatible avec la version d’EF Core à utiliser.</span><span class="sxs-lookup"><span data-stu-id="eb825-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="eb825-117">Par ex.</span><span class="sxs-lookup"><span data-stu-id="eb825-117">E.g.</span></span> <span data-ttu-id="eb825-118">les fournisseurs de base de données pour les versions antérieures ne sont pas compatibles avec la version 2.0 du runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb825-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="eb825-119">Les applications ciblant ASP.NET Core 2.0 peuvent utiliser EF Core 2.0 sans dépendances supplémentaires en plus des fournisseurs de base de données tiers.</span><span class="sxs-lookup"><span data-stu-id="eb825-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="eb825-120">Les applications ciblant des versions antérieures d’ASP.NET Core doivent être mises à niveau vers ASP.NET Core 2.0 pour utiliser EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="eb825-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="eb825-121">Développement multiplateforme à l’aide de l’interface de ligne de commande de base (CLI) .NET Core</span><span class="sxs-lookup"><span data-stu-id="eb825-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="eb825-122">Pour développer des applications qui ciblent [.NET Core](https://www.microsoft.com/net/download/core), vous pouvez choisir d’utiliser les [commandes CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) en combinaison avec votre éditeur de texte, ou bien un environnement de développement intégré (IDE) tel que Visual Studio, Visual Studio pour Mac ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="eb825-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="eb825-123">Les applications qui ciblent .NET Core requièrent des versions spécifiques de Visual Studio, par exemple, le développement .NET Core 1.x nécessite Visual Studio 2017, tandis que le développement .NET Core 2.0 requiert Visual Studio 2017 version 15.3.</span><span class="sxs-lookup"><span data-stu-id="eb825-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="eb825-124">Pour installer ou mettre à niveau le fournisseur SQL Server dans une application .NET Core multiplateforme, basculez vers le répertoire de l’application et exécutez la commande suivante dans une ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="eb825-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="eb825-125">Vous pouvez indiquer une installation de version spécifique dans la commande `dotnet add package`, à l’aide du modificateur `-v`.</span><span class="sxs-lookup"><span data-stu-id="eb825-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="eb825-126">Par ex.</span><span class="sxs-lookup"><span data-stu-id="eb825-126">E.g.</span></span> <span data-ttu-id="eb825-127">pour installer les packages EF Core 2.0, ajoutez `-v 2.0.0` à la commande.</span><span class="sxs-lookup"><span data-stu-id="eb825-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="eb825-128">EF Core comprend un ensemble de [commandes supplémentaires pour l’interface CLI `dotnet`](../../miscellaneous/cli/dotnet.md), commençant par `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="eb825-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="eb825-129">Pour que vous puissiez utiliser les commandes CLI `dotnet ef`, le fichier `.csproj` de votre application doit contenir l’entrée suivante :</span><span class="sxs-lookup"><span data-stu-id="eb825-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="eb825-130">Les outils CLI .NET Core pour EF Core nécessitent également un package distinct appelé Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="eb825-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="eb825-131">Vous pouvez simplement l’ajouter au projet à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="eb825-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="eb825-132">Utilisez toujours des versions des packages d’outils qui correspondent à la version principale des packages du runtime.</span><span class="sxs-lookup"><span data-stu-id="eb825-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="eb825-133">Développement Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb825-133">Visual Studio development</span></span>

<span data-ttu-id="eb825-134">Vous pouvez développer de nombreux types d’applications différents qui ciblent .NET Core, .NET Framework ou d’autres plateformes prises en charge par EF Core à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb825-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="eb825-135">Vous pouvez installer un fournisseur de base de données EF Core dans votre application à partir de Visual Studio de deux façons :</span><span class="sxs-lookup"><span data-stu-id="eb825-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="eb825-136">Utilisation de [l’interface utilisateur du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-ui) de NuGet</span><span class="sxs-lookup"><span data-stu-id="eb825-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="eb825-137">Sélectionnez le menu **Projet > Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb825-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="eb825-138">Cliquez sur l’onglet **Parcourir** ou **Mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="eb825-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="eb825-139">Sélectionnez le package `Microsoft.EntityFrameworkCore.SqlServer` et la version souhaitée, puis confirmez.</span><span class="sxs-lookup"><span data-stu-id="eb825-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="eb825-140">Utilisation de la [console du Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console) de NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb825-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="eb825-141">Sélectionnez le menu **Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="eb825-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="eb825-142">Tapez et exécutez la commande suivante dans la console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="eb825-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="eb825-143">Vous pouvez utiliser la commande `Update-Package` à la place pour mettre à jour un package qui est déjà installé dans une version plus récente.</span><span class="sxs-lookup"><span data-stu-id="eb825-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="eb825-144">Pour spécifier une version spécifique, vous pouvez utiliser le modificateur `-Version` ; par exemple, pour installer des packages EF Core 2.0, ajoutez `-Version 2.0.0` aux commandes.</span><span class="sxs-lookup"><span data-stu-id="eb825-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="eb825-145">Outils</span><span class="sxs-lookup"><span data-stu-id="eb825-145">Tools</span></span>

<span data-ttu-id="eb825-146">Il existe également dans Visual Studio une version de PowerShell des [commandes EF Core qui s’exécutent dans la console du Gestionnaire de package](../../miscellaneous/cli/powershell.md), avec des fonctionnalités similaires aux commandes `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="eb825-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="eb825-147">Pour utiliser ces commandes, installez le package `Microsoft.EntityFrameworkCore.Tools` à l’aide de l’interface utilisateur du Gestionnaire de package ou de la console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="eb825-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="eb825-148">Utilisez toujours des versions des packages d’outils qui correspondent à la version principale des packages du runtime.</span><span class="sxs-lookup"><span data-stu-id="eb825-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="eb825-149">Bien que vous puissiez utiliser les commandes `dotnet ef` à partir de la console du Gestionnaire de package dans Visual Studio, il est beaucoup plus pratique d’utiliser la version PowerShell :</span><span class="sxs-lookup"><span data-stu-id="eb825-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="eb825-150">Elles fonctionnent automatiquement avec le projet sélectionné dans la console du Gestionnaire de package sans que vous ayez besoin de basculer manuellement entre les répertoires.</span><span class="sxs-lookup"><span data-stu-id="eb825-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="eb825-151">Elles ouvrent automatiquement les fichiers générés par les commandes de Visual Studio après avoir été exécutées.</span><span class="sxs-lookup"><span data-stu-id="eb825-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="eb825-152">**Packages dépréciés dans EF Core 2.0 :** si vous mettez à niveau une application existante vers EF Core 2.0, vous pouvez être amené à supprimer manuellement des références à des packages EF Core plus anciens.</span><span class="sxs-lookup"><span data-stu-id="eb825-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="eb825-153">En particulier, les packages design-time des fournisseurs de base de données tels que `Microsoft.EntityFrameworkCore.SqlServer.Design` ne sont plus requis ou pris en charge dans EF Core 2.0, mais ils ne sont pas automatiquement supprimés durant la mise à niveau des autres packages.</span><span class="sxs-lookup"><span data-stu-id="eb825-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
