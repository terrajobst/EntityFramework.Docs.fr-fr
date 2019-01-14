---
title: Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 4734586adc89e9c1d866a1b4accd8b5e51fe2bb0
ms.sourcegitcommit: ebf661025d2ad2b62466fa7bf0e0772a7811cbe7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211164"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données

Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core. Le tutoriel utilise des migrations pour créer la base de données à partir du modèle de données.

Vous pouvez suivre le tutoriel à l’aide de Visual Studio 2017 sur Windows, ou à l’aide de l’interface CLI .NET Core sur Windows, macOS ou Linux.

Affichez l’exemple proposé dans cet article sur GitHub :
* [Visual Studio 2017 avec SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [CLI .NET Core avec SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec ces charges de travail :
  * **ASP.NET et développement web** (sous **Web et cloud**)
  * **Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)
* [SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* [SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)

---

## <a name="create-a-new-project"></a>Créer un projet

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ouvrez Visual Studio 2017.
* **Fichier > Nouveau > Projet**
* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.
* Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :
  * Vérifiez que les options **.NET Core** et **ASP.NET Core 2.1** sont sélectionnées dans les listes déroulantes.
  * Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.
  * Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.
  * Cliquez sur **OK**.

Avertissement : Si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté au projet dans `Models\IdentityModel.cs`. À l’aide des techniques présentées dans ce tutoriel, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* Exécutez la commande suivante pour créer un projet MVC :

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Accédez au répertoire du projet. Les commandes suivantes que vous entrez doivent s’exécuter sur le nouveau projet.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server. Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Ce tutoriel utilise SQLite, car il s’exécute sur toutes les plateformes prises en charge par .NET Core.

* Exécutez la commande suivante pour installer le fournisseur SQLite :

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Créer le modèle

Définissez une classe de contexte et des classes d’entité qui composent le modèle.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.
* Entrez le nom **Model.cs** et cliquez sur **OK**.
* Remplacez le contenu du fichier par le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* Dans le dossier **Models**, créez un fichier **Model.cs** avec le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Une application de production placerait généralement chaque classe dans un fichier distinct. Par souci de simplicité, ce tutoriel place ces classes dans un même fichier.

## <a name="register-the-context-with-dependency-injection"></a>Inscrire le contexte avec l’injection de dépendance

Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application. Ces services sont affectés aux composants qui en ont besoin (tels que les contrôleurs MVC) par le biais de propriétés ou de paramètres de constructeur.

Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans **Startup.cs** ajoutez les instructions `using` suivantes :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* Dans **Startup.cs** ajoutez les instructions `using` suivantes :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Une application de production placerait généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement. Par souci de simplicité, ce tutoriel la définit dans le code. Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Créer la base de données

Les étapes suivantes utilisent des [migrations](xref:core/managing-schemas/migrations/index) pour créer une base de données.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**
* Exécutez les commandes suivantes :

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Si vous obtenez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.

  La commande `Add-Migration` structure une migration afin de créer le jeu initial de tables du modèle. La commande `Update-Database` crée la base de données et lui applique la nouvelle migration.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* Exécutez les commandes suivantes :

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  La commande `migrations` structure une migration afin de créer le jeu initial de tables du modèle. La commande `database update` crée la base de données et lui applique la nouvelle migration.

---

## <a name="create-a-controller"></a>Créer un contrôleur

Générez automatiquement un contrôleur et des vues pour l’entité `Blog`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.
* Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **Ajouter**.
* Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.
* Cliquez sur **Ajouter**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

* Exécutez les commandes suivantes :

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  Les commandes `tool install` et `add package` installent les outils qui peuvent structurer les contrôleurs et les vues. La commande `restore` garantit que tous les packages du projet sont téléchargés, et la commande `aspnet-codegenerator` effectue la génération de modèles automatique.
---

Le moteur de génération de modèles automatique crée les fichiers suivants :

* Un contrôleur (*Controllers/BlogsController.cs*)
* Des vues Razor pour les pages Créer, Supprimer, Détails, Modifier et Index (_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>Exécuter l'application

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Déboguer** > **Exécuter sans débogage**

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Accédez à `/Blogs`.

* Utilisez le lien **Créer** pour créer des entrées de blog.

  ![Créer une page](_static/create.png)

* Testez les liens **Détails**, **Modifier** et **Supprimer**.

  ![Page d’index](_static/index-new-db.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Tutoriel : Bien démarrer avec EF Core sur .NET Core avec une nouvelle base de données à l’aide de SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutoriel : Pages Razor avec Entity Framework Core dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
