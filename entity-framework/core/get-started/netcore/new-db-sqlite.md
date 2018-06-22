---
title: Bien démarrer avec .NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Bien démarrer avec .NET Core à l’aide d’Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812532"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Bien démarrer avec EF Core sur une application console .NET Core avec une nouvelle base de données

Dans cette procédure pas à pas, vous allez créer une application console .NET Core qui exécute l’accès aux données de base d’une base de données SQLite à l’aide d’Entity Framework Core. Vous utiliserez des migrations pour créer la base de données à partir de votre modèle. Consultez [ASP.NET Core - Nouvelle base de données](xref:core/get-started/aspnetcore/new-db) pour une version de Visual Studio utilisant ASP.NET Core MVC.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) sur GitHub.

## <a name="prerequisites"></a>Prérequis

Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :
* Un système d’exploitation prenant en charge .NET Core
* Le [Kit SDK .NET Core](https://www.microsoft.com/net/core) 2.0 (bien que les instructions puissent être utilisées pour créer une application avec une version précédente avec très peu de modifications)

## <a name="create-a-new-project"></a>Créer un projet

* Créez un dossier `ConsoleApp.SQLite` pour votre projet et utilisez la commande `dotnet` pour la renseigner avec une application .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQLite. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Installez Microsoft.EntityFrameworkCore.Sqlite et Microsoft.EntityFrameworkCore.Design.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Modifiez manuellement `ConsoleApp.SQLite.csproj` pour ajouter un élément DotNetCliToolReference à Microsoft.EntityFrameworkCore.Tools.DotNet :

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` doit désormais avoir le contenu suivant :

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Remarque : les numéros de version ci-dessus étaient corrects au moment de cette publication.

*  Exécutez `dotnet restore` pour installer les nouveaux packages.

## <a name="create-the-model"></a>Créer le modèle

Définissez le contexte et les classes d’entité qui composeront le modèle.

* Créez un nouveau fichier *Model.cs* avec le contenu suivant.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Conseil : dans une application réelle, vous placeriez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration. Pour simplifier le didacticiel, nous plaçons tout dans un même fichier.

## <a name="create-the-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.

* Exécutez `dotnet ef migrations add InitialCreate` pour générer automatiquement un modèle de migration et créer l’ensemble initial de tables du modèle.
* Exécutez `dotnet ef database update` pour appliquer la nouvelle migration à la base de données. Cette commande crée la base de données avant d’appliquer des migrations.

> [!NOTE]  
> Durant l’utilisation de chemins d’accès relatifs avec SQLite, le chemin d’accès dépend de l’assembly principal de l’application. Dans cet exemple, le fichier binaire principal est `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, de sorte que la base de données SQLite se trouvera dans `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Utiliser le modèle

* Ouvrez le fichier *Program.cs* et remplacez le contenu par le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testez l’application :

  `dotnet run`

  Un blog est enregistré dans la base de données et les détails de tous les blogs s’affichent dans la console.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Modification du modèle :

- Si vous apportez des modifications au modèle, vous pouvez utiliser la commande `dotnet ef migrations add` pour générer automatiquement un nouveau modèle de [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour appliquer les modifications du schéma correspondantes à la base de données. Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `dotnet ef database update` pour appliquer les modifications à la base de données.
- EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.
- SQLite ne prend pas en charge toutes les migrations (modifications du schéma) en raison des limitations de SQLite. Consultez [Limitations de SQLite](../../providers/sqlite/limitations.md). Pour tout nouveau développement, il est préférable de supprimer la base de données et d’en créer une nouvelle plutôt que d’utiliser des migrations quand votre modèle est modifié.

## <a name="additional-resources"></a>Ressources supplémentaires

* [.NET Core - Nouvelle base de données avec SQLite](xref:core/get-started/netcore/new-db-sqlite) : didacticiel sur la console multiplateforme EF
* [Introduction à ASP.NET Core MVC sur Mac ou Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introduction à ASP.NET Core MVC avec Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
