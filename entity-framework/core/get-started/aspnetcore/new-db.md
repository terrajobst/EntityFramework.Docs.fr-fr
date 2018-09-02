---
title: Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996062"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données

Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core. Vous utilisez des migrations pour créer la base de données à partir de votre modèle EF Core.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :
  * **ASP.NET et développement web** (sous **Web et cloud**)
  * **Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)
* [SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)

## <a name="create-a-new-project-in-visual-studio-2017"></a>Créer un projet dans Visual Studio 2017

* Ouvrez Visual Studio 2017.
* **Fichier > Nouveau > Projet**
* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.
* Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :
  * Vérifiez que les options **.NET Core** et **ASP.NET Core 2.1** sont sélectionnées dans les listes déroulantes.
  * Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.
  * Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.
  * Cliquez sur **OK**.

Avertissement: si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté à votre projet dans `Models\IdentityModel.cs`. À l’aide des techniques présentées dans ce tutoriel, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md). 

Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server. Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Créer le modèle

Définissez une classe de contexte et les classes d’entité qui composent le modèle :

* Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.
* Entrez le nom **Model.cs** et cliquez sur **OK**.
* Remplacez le contenu du fichier par le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Dans une application réelle, vous placez généralement chaque classe de votre modèle dans un fichier distinct. Par souci de simplicité, ce tutoriel place toutes les classes dans un même fichier.

## <a name="register-your-context-with-dependency-injection"></a>Inscrire le contexte avec l’injection de dépendances

Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application. Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.

Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.

* Ouvrez **Startup.cs**.
* Ajoutez les instructions `using` suivantes :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Appelez la méthode `AddDbContext` pour inscrire le contexte en tant que service.

* Ajoutez le code en surbrillance suivant à la méthode `ConfigureServices` :

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Remarque : Une application réelle place généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement. Par souci de simplicité, ce tutoriel la définit dans le code. Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**
* Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle. Si vous recevez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.
* Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données. Cette commande crée la base de données avant d’appliquer des migrations.

## <a name="create-a-controller"></a>Créer un contrôleur

Générez automatiquement un contrôleur et des vues pour l’entité `Blog`.

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.
* Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **Ajouter**.
* Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.
* Cliquez sur **Ajouter**.


## <a name="run-the-application"></a>Exécuter l'application

Appuyez sur la touche F5 pour exécuter et tester l’application.

* Accédez à `/Blogs`.
* Utilisez le lien de création pour créer des entrées de blog. Testez les liens de détails et de suppression.

![image](_static/create.png)

![image](_static/index-new-db.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [EF - Nouvelle base de données avec SQLite](xref:core/get-started/netcore/new-db-sqlite) : didacticiel sur la console multiplateforme EF
* [Introduction à ASP.NET Core MVC sur Mac ou Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introduction à ASP.NET Core MVC avec Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
