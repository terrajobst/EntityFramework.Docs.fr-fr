---
title: Bien démarrer avec .NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
description: Bien démarrer avec .NET Core à l’aide d’Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: a0df80a8fe96be4f8cc3177919e2b087e14cb49c
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325325"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Bien démarrer avec EF Core sur une application console .NET Core avec une nouvelle base de données

Dans ce tutoriel, vous allez créer une application console .NET Core qui effectue un accès aux données d’une base de données SQLite à l’aide d’Entity Framework Core. Vous utilisez des migrations pour créer la base de données à partir du modèle. Consultez [ASP.NET Core - Nouvelle base de données](xref:core/get-started/aspnetcore/new-db) pour une version de Visual Studio utilisant ASP.NET Core MVC.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Prérequis

* [SDK .NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Créer un projet

* Créez un projet de console :

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Changer le répertoire actif

Lors des étapes suivantes, nous devons émettre des commandes `dotnet` sur l’application.

* Nous sélectionnons le répertoire de l’application comme répertoire actif en procédant comme suit :

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQLite. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Installez Microsoft.EntityFrameworkCore.Sqlite et Microsoft.EntityFrameworkCore.Design.

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Exécutez `dotnet restore` pour installer les nouveaux packages.

## <a name="create-the-model"></a>Créer le modèle

Définissez le contexte et les classes d’entité qui composeront le modèle.

* Créez un nouveau fichier *Model.cs* avec le contenu suivant.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Conseil : Dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration ou une variable d’environnement. Pour simplifier le tutoriel, tout est placé dans un seul et même fichier.

## <a name="create-the-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous utilisez des [migrations](xref:core/managing-schemas/migrations/index) pour créer une base de données.

* Exécutez `dotnet ef migrations add InitialCreate` pour générer automatiquement un modèle de migration et créer l’ensemble initial de tables du modèle.
* Exécutez `dotnet ef database update` pour appliquer la nouvelle migration à la base de données. Cette commande crée la base de données avant d’appliquer des migrations.

La base de données SQLite *blogging.db* se trouve dans le répertoire du projet.

## <a name="use-the-model"></a>Utiliser le modèle

* Ouvrez le fichier *Program.cs* et remplacez le contenu par le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testez l’application à partir de la console. Consultez la [Remarque sur Visual Studio](#vs) pour exécuter l’application à partir de Visual Studio.

  `dotnet run`

  Un blog est enregistré dans la base de données et les détails de tous les blogs s’affichent dans la console.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Modification du modèle :

- Si vous apportez des modifications au modèle, vous pouvez utiliser la commande `dotnet ef migrations add` pour générer automatiquement une nouvelle [migration](xref:core/managing-schemas/migrations/index). Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `dotnet ef database update` pour appliquer les modifications de schéma à la base de données.
- EF Core utilise une table `__EFMigrationsHistory` dans la base de données pour effectuer le suivi des migrations qui ont déjà été appliquées à la base de données.
- Le moteur de base de données SQLite ne prend pas en charge certaines modifications de schéma qui sont prises en charge par la plupart des autres bases de données relationnelles. Par exemple, l’opération `DropColumn` n’est pas prise en charge. Les migrations EF Core génèrent du code pour ces opérations, mais si vous tentez de les appliquer à une base de données ou de générer un script, EF Core lève des exceptions. Consultez [Limitations de SQLite](../../providers/sqlite/limitations.md). Pour tout nouveau développement, il est préférable de supprimer la base de données et d’en créer une nouvelle plutôt que d’utiliser des migrations quand le modèle change.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Exécuter à partir de Visual Studio

Pour exécuter cet exemple à partir de Visual Studio, vous devez définir manuellement le répertoire de travail comme racine du projet. Si vous ne définissez pas le répertoire de travail, l’exception `Microsoft.Data.Sqlite.SqliteException` suivante est levée : `SQLite Error 1: 'no such table: Blogs'`.

Pour définir le répertoire de travail :

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.
* Sélectionnez l’onglet **Déboguer** dans le volet gauche.
* Définissez le répertoire du projet comme **Répertoire de travail**.
* Enregistrez les modifications.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tutoriel : Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données à l’aide de SQLite](xref:core/get-started/aspnetcore/new-db)
* [Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutoriel : Pages Razor avec Entity Framework Core dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
