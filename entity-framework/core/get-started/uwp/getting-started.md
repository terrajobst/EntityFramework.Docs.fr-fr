---
title: "Bien démarrer avec UWP - Nouvelle base de données - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données

> [!NOTE]
> Ce didacticiel utilise EF Core 2.0.1 (version publiée avec ASP.NET Core et le Kit SDK .NET Core 2.0.3). EF Core 2.0.0 ne dispose pas de certains correctifs de bogues essentiels pour une bonne expérience UWP.

Dans cette procédure pas à pas, vous allez générer une plateforme Windows universelle (UWP) qui exécute l’accès aux données de base d’une base de données SQLite locale à l’aide d’Entity Framework.

> [!IMPORTANT]
> **Évitez les types anonymes dans les requêtes LINQ sur UWP**. Pour déployer une application UWP dans le magasin d’applications, votre application doit être générée avec .NET Native. Les requêtes avec des types anonymes ont des performances inférieures sur .NET Native.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) sur GitHub.

## <a name="prerequisites"></a>Conditions préalables

Les éléments suivants sont nécessaires pour effectuer cette procédure pas à pas :

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 ou ultérieure avec la charge de travail **Développement pour la plateforme Windows universelle**

## <a name="create-a-new-model-project"></a>Créer un projet de modèle

> [!WARNING]
> En raison des limitations au niveau de l’interaction des outils .NET Core avec les projets UWP, le modèle doit être placé dans un projet autre que UWP pour pouvoir exécuter des commandes de migration dans la console du Gestionnaire de package.

* Ouvrir Visual Studio

* Fichier > Nouveau > Projet...

* Dans le menu de gauche, sélectionnez Modèles > Visual C#.

* Sélectionnez le modèle de projet **Bibliothèque de classes (.NET Standard)**.

* Nommez le projet et cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQLite. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

Un peu plus loin dans cette procédure pas à pas, nous utiliserons également des outils Entity Framework pour gérer la base de données. Nous installerons donc également le package d’outils.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

* Modifiez le fichier .csproj et remplacez `<TargetFramework>netstandard2.0</TargetFramework>` par `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.

## <a name="create-your-model"></a>Créer un modèle

Définissons à présent le contexte et les classes d’entité qui composeront le modèle.

* Projet > Ajouter une classe...

* Entrez le nom *Model.cs* et cliquez sur **OK**.

* Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Créer un projet UWP

* Ouvrir Visual Studio

* Fichier > Nouveau > Projet...

* Dans le menu de gauche, sélectionnez Modèles > Visual C# > Windows universel.

* Sélectionnez le modèle de projet **Application vide (Windows universel)**.

* Nommez le projet et cliquez sur **OK**.

* Définissez la cible et les versions minimales sur `Windows 10 Fall Creators Update (10.0; build 16299.0)` au minimum.

## <a name="create-your-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.

* Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package

* Sélectionnez le projet de modèle en tant que projet par défaut et définissez-le comme projet de démarrage.

* Exécutez `Add-Migration MyFirstMigration` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.

Étant donné que nous voulons créer la base de données sur l’appareil sur lequel est exécutée l’application, nous allons ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application. Au cours de la première exécution de l’application, ce code se chargera de créer la base de données locale pour nous.

* Cliquez avec le bouton de droite sur **App.xaml** dans l’**Explorateur de solutions**, puis sélectionnez **Afficher le code**.

* Ajoutez la ligne using mise en surbrillance au début du fichier.

* Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Si vous apportez des modifications ultérieures au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les modifications correspondantes à la base de données. Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.
>
>EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.

## <a name="use-your-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *MainPage.xaml*.

* Ajoutez le gestionnaire de chargement de page et le contenu de l’interface utilisateur mis en surbrillance ci-dessous.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Nous allons maintenant ajouter du code pour connecter l’interface utilisateur à la base de données.

* Cliquez avec le bouton de droite sur **MainPage.xaml** dans l’**Explorateur de solutions**, puis sélectionnez **Afficher le code**.

* Ajoutez le code mis en surbrillance de la liste suivante.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Vous pouvez à présent exécuter l’application pour la voir en action.

* Déboguer > Exécuter sans débogage

* L’application est générée et lancée.

* Entrez une URL et cliquez sur le bouton **Ajouter**.

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a>Étapes suivantes

> [!TIP]
> La performance `SaveChanges()` peut être améliorée en implémentant [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx) et [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) dans vos types d’entité et en utilisant `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Et le tour est joué ! Vous avez désormais une application UWP simple exécutant Entity Framework.

Consultez les autres articles de cette documentation pour en savoir plus sur les fonctionnalités d’Entity Framework.
