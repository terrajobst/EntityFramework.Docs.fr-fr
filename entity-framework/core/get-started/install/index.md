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
# <a name="installing-entity-framework-core"></a>Installation d’Entity Framework Core

## <a name="prerequisites"></a>Prérequis

* Pour développer des applications qui ciblent .NET Core 2.1, installez [le SDK .NET Core 2.1](https://www.microsoft.com/net/download/core). Le kit SDK doit être installé même si vous disposez de la dernière version de Visual Studio 2017.

* Pour utiliser Visual Studio pour développer des applications qui ciblent .NET Core 2.1, installez Visual Studio 2017 version 15.7 ou ultérieure.

* Pour utiliser Entity Framework 2.1 dans les applications ASP.NET Core, utilisez ASP.NET Core 2.1. Les applications qui utilisent des versions antérieures d’ASP.NET Core doivent être mises à jour avec la version 2.1.

* Vous pouvez utiliser Visual Studio 2015 pour les applications qui ciblent .NET Framework 4.6.1 ou ultérieur. Par contre, vous avez besoin d’une version NuGet qui connaît .NET Standard 2.0 et ses frameworks compatibles. Pour obtenir ceci dans Visual Studio 2015, [mettez à niveau le client NuGet avec la version 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Obtenir le runtime Entity Framework Core

Pour ajouter les bibliothèques du runtime EF Core dans une application, installez le package NuGet du fournisseur de base de données que vous souhaitez utiliser. Pour obtenir la liste des fournisseurs pris en charge et leurs noms de package NuGet, consultez [Fournisseurs de bases de données](../../providers/index.md).

Pour installer ou mettre à jour les packages NuGet, utilisez l’interface CLI .NET Core, la boîte de dialogue Gestionnaire de package Visual Studio ou la console du Gestionnaire de package Visual Studio.

Pour les applications ASP.NET Core 2.1, les fournisseurs SQL Server en mémoire sont automatiquement inclus, il est donc inutile de les installer séparément.

> [!TIP]  
> Si vous devez mettre à jour une application qui utilise un fournisseur de base de données tiers, recherchez toujours une mise à jour du fournisseur qui est compatible avec la version d’EF Core à utiliser. Par exemple, les fournisseurs de base de données pour les versions antérieures ne sont pas compatibles avec la version 2.1 du runtime EF Core.  

### <a name="net-core-cli"></a>CLI .NET Core

La commande de l’interface CLI .NET Core suivante installe ou met à jour le fournisseur SQL Server :

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Vous pouvez indiquer une version spécifique dans la commande `dotnet add package`, à l’aide du modificateur `-v`. Par exemple, pour installer des packages EF Core 2.1.0, ajoutez `-v 2.1.0` à la commande.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Boîte de dialogue Gestionnaire de package NuGet Visual Studio

* Dans le menu, sélectionnez **Projet > Gérer les packages NuGet**.

* Cliquez sur l’onglet **Parcourir** ou **Mises à jour**.

* Pour installer ou mettre à jour le fournisseur SQL Server, sélectionnez le package `Microsoft.EntityFrameworkCore.SqlServer` et confirmez.

Pour plus d’informations, consultez [Boîte de dialogue Gestionnaire de package NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Console du Gestionnaire de package NuGet Visual Studio

* Dans le menu, sélectionnez **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.

* Pour installer le fournisseur SQL Server, exécutez la commande suivante dans la Console du Gestionnaire de package :

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Pour mettre à jour le fournisseur, utilisez la commande `Update-Package`.

* Pour indiquer une version spécifique, utilisez le modificateur `-Version`. Par exemple, pour installer les packages EF Core 2.1.0, ajoutez `-Version 2.1.0` aux commandes.

Pour plus d’informations, consultez [Console Gestionnaire de package](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Obtenir les outils Entity Framework Core

En plus des bibliothèques du runtime, vous pouvez installer les outils capables d’effectuer des tâches liées à EF Core dans votre projet au moment du design. Par exemple, vous pouvez créer des migrations, appliquer des migrations et créer un modèle basé sur une base de données existante.

Deux ensembles d’outils sont disponibles :
* Les [outils de l’interface de ligne de commande (CLI)](../../miscellaneous/cli/dotnet.md) .NET Core peuvent être utilisés sur Windows, Linux ou macOS. Ces commandes commencent par `dotnet ef`. 
* Les [outils de la console du Gestionnaire de package](../../miscellaneous/cli/powershell.md) s’exécutent dans Visual Studio 2017 sur Windows. Ces commandes commencent par un verbe, par exemple `Add-Migration`, `Update-Database`.

Même si vous pouvez utiliser les commandes `dotnet ef` à partir de la console du Gestionnaire de package, il est plus pratique de se servir des outils de la console du Gestionnaire de package quand vous utilisez Visual Studio :
* Ils fonctionnent automatiquement avec le projet sélectionné dans la console du Gestionnaire de package et vous évitent de devoir passer d’un répertoire à l’autre manuellement.  
* Elles ouvrent automatiquement les fichiers générés par les commandes de Visual Studio après avoir été exécutées.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Obtenir les outils CLI

Les commandes `dotnet ef` sont dans le SDK .NET Core, mais pour les activer, vous devez installer le package `Microsoft.EntityFrameworkCore.Design` :

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Pour les applications ASP.NET Core 2.1, ce package est inclus automatiquement.

Comme expliqué précédemment dans [Prérequis](#prerequisites), vous devez également installer le SDK .NET Core 2.1.

> [!IMPORTANT]      
> Utilisez toujours la version des packages d’outils qui correspond à la version principale des packages du runtime.

### <a name="get-the-package-manager-console-tools"></a>Obtenir les outils de la console du Gestionnaire de package

Pour obtenir les outils de la console du Gestionnaire de package pour EF Core, installez le package `Microsoft.EntityFrameworkCore.Tools` :

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Pour les applications ASP.NET Core 2.1, ce package est inclus automatiquement.

## <a name="upgrading-to-ef-core-21"></a>Mise à niveau vers EF Core 2.1

Si vous mettez à niveau une application existante avec EF Core 2.1, vous pouvez être amené à supprimer manuellement des références à des packages EF Core plus anciens :

* Les packages design-time des fournisseurs de base de données tels que `Microsoft.EntityFrameworkCore.SqlServer.Design` ne sont plus obligatoires ou pris en charge dans EF Core 2.1, mais ne sont pas automatiquement supprimés durant la mise à niveau des autres packages.

* Les outils CLI .NET étant désormais inclus dans le Kit de développement logiciel (SDK) .NET, la référence à ce package peut être supprimée du fichier *.csproj* :

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Pour les applications qui ciblent le .NET Framework et qui ont été créées dans des versions antérieures de Visual Studio, vérifiez qu’elles sont compatibles avec les bibliothèques .NET Standard 2.0 :

  * Modifiez le fichier projet et vérifiez que l’entrée suivante apparaît dans le groupe de propriétés initiales :

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Pour les projets de test, vérifiez également que l’entrée suivante est présente :

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
