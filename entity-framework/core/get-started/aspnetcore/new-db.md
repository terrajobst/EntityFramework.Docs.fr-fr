---
title: "Bien démarrer avec ASP.NET Core - Nouvelle base de données - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Bien démarrer avec EF Core sur ASP.NET Core avec une nouvelle base de données

Dans cette procédure pas à pas, vous allez générer une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core. Vous allez utiliser des migrations pour créer la base de données à partir de votre modèle EF Core. Consultez [Ressources supplémentaires](#additional-resources) pour accéder aux autres didacticiels sur Entity Framework Core.

Ce didacticiel requiert :
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :
  * **ASP.NET et développement web** (sous **Web et cloud**)
  * **Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)
* [Kit SDK .NET Core 2.0](https://www.microsoft.com/net/download/core)

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) sur GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Créer un projet dans Visual Studio 2017

* **Fichier > Nouveau > Projet**
* Dans le menu de gauche, sélectionnez **Installé > Modèles > Visual C# > .NET Core**.
* Sélectionnez **Nouvelle application web ASP.NET Core**.
* Entrez le nom **EFGetStarted.AspNetCore.NewDb** et cliquez sur **OK**.
* Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :
  * Vérifiez que les options **.NET Core** et **ASP.NET Core 2.0** sont sélectionnées dans les listes déroulantes.
  * Sélectionnez le modèle de projet **Application web (Model-View-Controller)**.
  * Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.
  * Cliquez sur **OK**.

Avertissement: si vous définissez le paramètre **Authentification** sur **Comptes d’utilisateur individuels** au lieu de **Aucune**, un modèle Entity Framework Core sera ajouté à votre projet dans `Models\IdentityModel.cs`. À l’aide des techniques que vous allez découvrir dans cette procédure pas à pas, vous pouvez ajouter un second modèle ou étendre ce modèle existant pour qu’il puisse contenir vos classes d’entité.

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Installez le package pour le ou les fournisseurs de bases de données EF Core à cibler. Cette procédure pas à pas utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Nous utiliserons des outils Entity Framework Core pour créer une base de données à partir de votre modèle EF Core. Nous installerons donc également le package d’outils :

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

Nous utiliserons des outils de génération de modèles automatique ASP.NET Core pour créer par la suite des contrôleurs et des vues. Nous installerons donc également ce package de conception :

* Exécutez `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.

## <a name="create-the-model"></a>Créer le modèle

Définissez le contexte et les classes d’entité qui composeront le modèle :

* Cliquez avec le bouton de droite sur le dossier **Modèles** et sélectionnez **Ajouter > Classe**.
* Entrez le nom **Model.cs** et cliquez sur **OK**.
* Remplacez le contenu du fichier par le code suivant :

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Remarque : dans une application réelle, vous placeriez généralement chaque classe de votre modèle dans un fichier distinct. Dans le cadre de ce didacticiel, par souci de simplicité, nous plaçons toutes les classes dans un même fichier.

## <a name="register-your-context-with-dependency-injection"></a>Inscrire le contexte avec l’injection de dépendances

Des services (tels que `BloggingContext`) sont inscrits avec l’[injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) au démarrage de l’application. Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur.

Afin de permettre à nos contrôleurs MVC d’utiliser `BloggingContext`, nous allons inscrire ce dernier en tant que service.

* Ouvrez **Startup.cs**.
* Ajoutez les instructions `using` suivantes :

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Ajoutez la méthode `AddDbContext` pour l’inscrire en tant que service :

* Ajoutez le code suivant à la méthode `ConfigureServices` :

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Remarque : une application réelle placerait généralement la chaîne de connexion dans un fichier de configuration. Par souci de simplicité, nous allons la définir dans le code. Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).

## <a name="create-your-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) pour créer une base de données.

* Ouvrez la console du Gestionnaire de package (PMC) :

  **Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**
* Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle. Si vous recevez l’erreur `The term 'add-migration' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.
* Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données. Cette commande crée la base de données avant d’appliquer des migrations.

## <a name="create-a-controller"></a>Créer un contrôleur

Activez la génération de modèles automatique dans le projet :

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.
* Sélectionnez **Dépendances minimales** et cliquez sur **Ajouter**.
* Vous pouvez ignorer ou supprimer le fichier *ScaffoldingReadMe.txt*.

Une fois activée la génération de modèles automatique, nous pouvons générer automatiquement un modèle de contrôleur pour l’entité `Blog`.

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter > Contrôleur**.
* Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.
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
