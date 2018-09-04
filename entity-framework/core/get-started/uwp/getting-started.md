---
title: Bien démarrer avec UWP - Nouvelle base de données - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996908"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données

Dans ce tutoriel, vous allez générer une application UWP (Universal Windows Platform) qui effectue un accès aux données élémentaire sur une base de données SQLite locale à l’aide d’Entity Framework Core.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Prérequis

* [Windows 10 Fall Creators Update (10.0 ; Build 16299) ou version ultérieure](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **Développement pour la plateforme Windows universelle**.

* [SDK .NET Core 2.1](https://www.microsoft.com/net/core) ou version ultérieure.

## <a name="create-a-model-project"></a>Créer un projet de modèle

> [!IMPORTANT]
> En raison des limitations au niveau de l’interaction des outils .NET Core avec les projets UWP, le modèle doit être placé dans un projet autre que UWP pour que vous puissiez exécuter des commandes de migration dans la **Console du Gestionnaire de package**.

* Ouvrir Visual Studio

* **Fichier > Nouveau > Projet**

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Standard**.

* Sélectionnez le modèle **Bibliothèque de classes (.NET Standard)**.

* Nommez le projet *Blogging.Model*.

* Nommez la solution *Blogging*.

* Cliquez sur **OK**.

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Ce tutoriel utilise SQLite. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

Un peu plus loin dans ce tutoriel, vous utiliserez certains outils Entity Framework Core pour tenir à jour la base de données. Installez donc le package d’outils.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-the-model"></a>Créer le modèle

Définissons à présent le contexte et les classes d’entité qui composent le modèle.

* Supprimez *Class1.cs*.

* Créez *Model.cs* avec le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Créer un projet UWP

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows universel**.

* Sélectionnez le modèle de projet **Application vide (Windows universel)**.

* Nommez le projet *Blogging.UWP*, puis cliquez sur **OK**.

* Définissez les versions minimale et cible sur **Windows 10 Fall Creators Update (10.0 ; build 16299.0)** au minimum.

## <a name="create-the-initial-migration"></a>Créer la migration initiale

Maintenant que vous avez un modèle, configurez l’application pour créer une base de données lors de sa première exécution. Dans cette section, vous allez créer la migration initiale. Dans la section suivante, vous ajouterez du code qui applique cette migration au démarrage de l’application.

Les outils de migration nécessitent un projet de démarrage autre que UWP. Vous allez donc le créer en premier lieu.

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.

* Sélectionnez le modèle de projet **Application console (.NET Core)**.

* Nommez le projet *Blogging.Migrations.Startup*, puis cliquez sur **OK**.

* Ajoutez une référence de projet du projet *Blogging.Migrations.Startup* au projet *Blogging.Model*.

Vous pouvez maintenant créer votre migration initiale.

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Sélectionnez le projet *Blogging.Model* comme **Projet par défaut**.

* Dans l’**Explorateur de solutions**, définissez le projet *Blogging.Migrations.Startup* comme projet de démarrage.

* Exécutez `Add-Migration InitialCreate`.

  Cette commande génère automatiquement un modèle de migration qui crée l’ensemble initial de tables de votre modèle.

## <a name="create-the-database-on-app-startup"></a>Créer la base de données au démarrage de l’application

Étant donné que vous souhaitez créer la base de données sur l’appareil sur lequel l’application s’exécute, vous allez ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application. Lors de la première exécution de l’application, ce code se chargera de créer la base de données locale.

* Ajoutez une référence de projet du projet *Blogging.UWP* au projet *Blogging.Model*.

* Ouvrez *App.xaml.cs*.

* Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Si vous modifiez votre modèle, utilisez la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration afin d’appliquer les modifications correspondantes à la base de données. Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.
>
>EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.

## <a name="use-the-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *MainPage.xaml*.

* Ajoutez le gestionnaire de chargement de page et le contenu de l’interface utilisateur mis en surbrillance ci-dessous.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Maintenant, ajoutez du code pour connecter l’interface utilisateur à la base de données.

* Ouvrez *MainPage.xaml.cs*.

* Ajoutez le code mis en surbrillance de la liste suivante :

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Vous pouvez à présent exécuter l’application pour la voir en action.

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet *Blogging.UWP*, puis sélectionnez **Déployer**.

* Définissez *Blogging.UWP* comme projet de démarrage.

* **Déboguer > Démarrer sans débogage**

  L’application est générée et s’exécute.

* Entrez une URL et cliquez sur le bouton **Ajouter**.

  ![image](_static/create.png)

  ![image](_static/list.png)

  Et le tour est joué ! Vous avez désormais une application UWP simple exécutant Entity Framework Core.

## <a name="next-steps"></a>Étapes suivantes

Pour accéder à des informations sur la compatibilité et les performances que vous devez connaître quand vous utilisez EF Core avec UWP, consultez [Implémentations de .NET prises en charge par EF Core](../../platforms/index.md#universal-windows-platform).

Consultez les autres articles de cette documentation pour en savoir plus sur les fonctionnalités d’Entity Framework Core.
