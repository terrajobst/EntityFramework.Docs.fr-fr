---
title: Installation d’Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575637"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="334ff-102">Installation d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="334ff-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="334ff-103">Prérequis</span><span class="sxs-lookup"><span data-stu-id="334ff-103">Prerequisites</span></span>

* <span data-ttu-id="334ff-104">Pour développer des applications qui ciblent .NET Core 2.1, installez [le SDK .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="334ff-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="334ff-105">Le kit SDK doit être installé même si vous disposez de la dernière version de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="334ff-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="334ff-106">Pour utiliser Visual Studio pour développer des applications qui ciblent .NET Core 2.1, installez Visual Studio 2017 version 15.7 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="334ff-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="334ff-107">Pour utiliser Entity Framework 2.1 dans les applications ASP.NET Core, utilisez ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="334ff-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="334ff-108">Les applications qui utilisent des versions antérieures d’ASP.NET Core doivent être mises à jour avec la version 2.1.</span><span class="sxs-lookup"><span data-stu-id="334ff-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="334ff-109">Vous pouvez utiliser Visual Studio 2015 pour les applications qui ciblent .NET Framework 4.6.1 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="334ff-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="334ff-110">Par contre, vous avez besoin d’une version NuGet qui connaît .NET Standard 2.0 et ses frameworks compatibles.</span><span class="sxs-lookup"><span data-stu-id="334ff-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="334ff-111">Pour obtenir ceci dans Visual Studio 2015, [mettez à niveau le client NuGet avec la version 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="334ff-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="334ff-112">Obtenir le runtime Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="334ff-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="334ff-113">Pour ajouter les bibliothèques du runtime EF Core dans une application, installez le package NuGet du fournisseur de base de données que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="334ff-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="334ff-114">Pour obtenir la liste des fournisseurs pris en charge et leurs noms de package NuGet, consultez [Fournisseurs de bases de données](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="334ff-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="334ff-115">Pour installer ou mettre à jour les packages NuGet, utilisez l’interface CLI .NET Core, la boîte de dialogue Gestionnaire de package Visual Studio ou la console du Gestionnaire de package Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="334ff-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="334ff-116">Pour les applications ASP.NET Core 2.1, les fournisseurs SQL Server en mémoire sont automatiquement inclus, il est donc inutile de les installer séparément.</span><span class="sxs-lookup"><span data-stu-id="334ff-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="334ff-117">Si vous devez mettre à jour une application qui utilise un fournisseur de base de données tiers, recherchez toujours une mise à jour du fournisseur qui est compatible avec la version d’EF Core à utiliser.</span><span class="sxs-lookup"><span data-stu-id="334ff-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="334ff-118">Par exemple, les fournisseurs de base de données pour les versions antérieures ne sont pas compatibles avec la version 2.1 du runtime EF Core.</span><span class="sxs-lookup"><span data-stu-id="334ff-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="334ff-119">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="334ff-119">.NET Core CLI</span></span>

<span data-ttu-id="334ff-120">La commande de l’interface CLI .NET Core suivante installe ou met à jour le fournisseur SQL Server :</span><span class="sxs-lookup"><span data-stu-id="334ff-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="334ff-121">Vous pouvez indiquer une version spécifique dans la commande `dotnet add package`, à l’aide du modificateur `-v`.</span><span class="sxs-lookup"><span data-stu-id="334ff-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="334ff-122">Par exemple, pour installer des packages EF Core 2.1.0, ajoutez `-v 2.1.0` à la commande.</span><span class="sxs-lookup"><span data-stu-id="334ff-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="334ff-123">Boîte de dialogue Gestionnaire de package NuGet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="334ff-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="334ff-124">Dans le menu, sélectionnez **Projet > Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="334ff-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="334ff-125">Cliquez sur l’onglet **Parcourir** ou **Mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="334ff-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="334ff-126">Pour installer ou mettre à jour le fournisseur SQL Server, sélectionnez le package `Microsoft.EntityFrameworkCore.SqlServer` et confirmez.</span><span class="sxs-lookup"><span data-stu-id="334ff-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="334ff-127">Pour plus d’informations, consultez [Boîte de dialogue Gestionnaire de package NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="334ff-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="334ff-128">Console du Gestionnaire de package NuGet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="334ff-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="334ff-129">Dans le menu, sélectionnez **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="334ff-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="334ff-130">Pour installer le fournisseur SQL Server, exécutez la commande suivante dans la Console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="334ff-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="334ff-131">Pour mettre à jour le fournisseur, utilisez la commande `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="334ff-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="334ff-132">Pour indiquer une version spécifique, utilisez le modificateur `-Version`.</span><span class="sxs-lookup"><span data-stu-id="334ff-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="334ff-133">Par exemple, pour installer les packages EF Core 2.1.0, ajoutez `-Version 2.1.0` aux commandes.</span><span class="sxs-lookup"><span data-stu-id="334ff-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="334ff-134">Pour plus d’informations, consultez [Console Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="334ff-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="334ff-135">Obtenir les outils Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="334ff-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="334ff-136">En plus des bibliothèques du runtime, vous pouvez installer les outils capables d’effectuer des tâches liées à EF Core dans votre projet au moment du design.</span><span class="sxs-lookup"><span data-stu-id="334ff-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="334ff-137">Par exemple, vous pouvez créer des migrations, appliquer des migrations et créer un modèle basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="334ff-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="334ff-138">Deux ensembles d’outils sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="334ff-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="334ff-139">Les [outils de l’interface de ligne de commande (CLI)](../../miscellaneous/cli/dotnet.md) .NET Core peuvent être utilisés sur Windows, Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="334ff-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="334ff-140">Ces commandes commencent par `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="334ff-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="334ff-141">Les [outils de la console du Gestionnaire de package](../../miscellaneous/cli/powershell.md) s’exécutent dans Visual Studio 2017 sur Windows.</span><span class="sxs-lookup"><span data-stu-id="334ff-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="334ff-142">Ces commandes commencent par un verbe, par exemple `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="334ff-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="334ff-143">Même si vous pouvez utiliser les commandes `dotnet ef` à partir de la console du Gestionnaire de package, il est plus pratique de se servir des outils de la console du Gestionnaire de package quand vous utilisez Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="334ff-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="334ff-144">Ils fonctionnent automatiquement avec le projet sélectionné dans la console du Gestionnaire de package et vous évitent de devoir passer d’un répertoire à l’autre manuellement.</span><span class="sxs-lookup"><span data-stu-id="334ff-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="334ff-145">Elles ouvrent automatiquement les fichiers générés par les commandes de Visual Studio après avoir été exécutées.</span><span class="sxs-lookup"><span data-stu-id="334ff-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="334ff-146">Obtenir les outils CLI</span><span class="sxs-lookup"><span data-stu-id="334ff-146">Get the CLI tools</span></span>

<span data-ttu-id="334ff-147">Les commandes `dotnet ef` sont dans le SDK .NET Core, mais pour les activer, vous devez installer le package `Microsoft.EntityFrameworkCore.Design` :</span><span class="sxs-lookup"><span data-stu-id="334ff-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="334ff-148">Pour les applications ASP.NET Core 2.1, ce package est inclus automatiquement.</span><span class="sxs-lookup"><span data-stu-id="334ff-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="334ff-149">Comme expliqué précédemment dans [Prérequis](#prerequisites), vous devez également installer le SDK .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="334ff-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="334ff-150">Utilisez toujours la version des packages d’outils qui correspond à la version principale des packages du runtime.</span><span class="sxs-lookup"><span data-stu-id="334ff-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="334ff-151">Obtenir les outils de la console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="334ff-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="334ff-152">Pour obtenir les outils de la console du Gestionnaire de package pour EF Core, installez le package `Microsoft.EntityFrameworkCore.Tools` :</span><span class="sxs-lookup"><span data-stu-id="334ff-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="334ff-153">Pour les applications ASP.NET Core 2.1, ce package est inclus automatiquement.</span><span class="sxs-lookup"><span data-stu-id="334ff-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="334ff-154">Mise à niveau vers EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="334ff-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="334ff-155">Si vous mettez à niveau une application existante avec EF Core 2.1, vous pouvez être amené à supprimer manuellement des références à des packages EF Core plus anciens :</span><span class="sxs-lookup"><span data-stu-id="334ff-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="334ff-156">Les packages design-time des fournisseurs de base de données tels que `Microsoft.EntityFrameworkCore.SqlServer.Design` ne sont plus obligatoires ou pris en charge dans EF Core 2.1, mais ne sont pas automatiquement supprimés durant la mise à niveau des autres packages.</span><span class="sxs-lookup"><span data-stu-id="334ff-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="334ff-157">Les outils CLI .NET étant désormais inclus dans le Kit de développement logiciel (SDK) .NET, la référence à ce package peut être supprimée du fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="334ff-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="334ff-158">Pour les applications qui ciblent le .NET Framework et qui ont été créées dans des versions antérieures de Visual Studio, vérifiez qu’elles sont compatibles avec les bibliothèques .NET Standard 2.0 :</span><span class="sxs-lookup"><span data-stu-id="334ff-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="334ff-159">Modifiez le fichier projet et vérifiez que l’entrée suivante apparaît dans le groupe de propriétés initiales :</span><span class="sxs-lookup"><span data-stu-id="334ff-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="334ff-160">Pour les projets de test, vérifiez également que l’entrée suivante est présente :</span><span class="sxs-lookup"><span data-stu-id="334ff-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
