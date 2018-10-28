---
title: Bien démarrer avec UWP - Nouvelle base de données - EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022374"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Bien démarrer avec EF Core sur la plateforme Windows universelle (UWP) avec une nouvelle base de données

Dans ce tutoriel, vous allez générer une application UWP (Universal Windows Platform) qui effectue un accès aux données élémentaire sur une base de données SQLite locale à l’aide d’Entity Framework Core.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Prérequis

* [Windows 10 Fall Creators Update (10.0 ; Build 16299) ou version ultérieure](https://support.microsoft.com/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 version 15.7 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **Développement pour la plateforme Windows universelle**.

* [SDK .NET Core 2.1](https://www.microsoft.com/net/core) ou version ultérieure.

> [!IMPORTANT]
> Ce tutoriel utilise des commandes de [migration](xref:core/managing-schemas/migrations/index) Entity Framework Core pour créer et mettre à jour le schéma de la base de données.
> Ces commandes ne fonctionnent pas directement avec les projets UWP.
> Pour cette raison, le modèle de données de l’application est placé dans un projet de bibliothèque partagé, et une application console .NET Core distincte est utilisée pour exécuter les commandes.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Créer un projet de bibliothèque pour stocker le modèle de données

* Ouvrir Visual Studio

* **Fichier > Nouveau > Projet**

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Standard**.

* Sélectionnez le modèle **Bibliothèque de classes (.NET Standard)**.

* Nommez le projet *Blogging.Model*.

* Nommez la solution *Blogging*.

* Cliquez sur **OK**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Installer le runtime Entity Framework Core dans le projet de modèle de données

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Ce tutoriel utilise SQLite. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**.

* Vérifiez que le projet de bibliothèque *Blogging.Model* est sélectionné comme **Projet par défaut** dans la Console du Gestionnaire de package.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

## <a name="create-the-data-model"></a>Créer le modèle de données

Définissons à présent le *DbContext* et les classes d’entité qui composent le modèle.

* Supprimez *Class1.cs*.

* Créez *Model.cs* avec le code suivant :

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Créer un projet de console pour exécuter des commandes de migration

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > .NET Core**.

* Sélectionnez le modèle de projet **Application console (.NET Core)**.

* Nommez le projet *Blogging.Migrations.Startup*, puis cliquez sur **OK**.

* Ajoutez une référence de projet du projet *Blogging.Migrations.Startup* au projet *Blogging.Model*.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Installer les outils Entity Framework Core dans le projet de démarrage de migrations

Pour activer les commandes de migration EF Core dans la Console du Gestionnaire de package, installez le package d’outils EF Core dans l’application console.

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`.

## <a name="create-the-initial-migration"></a>Créer la migration initiale

 Créez la migration initiale, en spécifiant l’application console comme projet de démarrage.

* Exécutez `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`.

Cette commande génère automatiquement un modèle de migration qui crée l’ensemble initial de tables de bases de données pour votre modèle de données.

## <a name="create-the-uwp-project"></a>Créer le projet UWP

* Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis choisissez **Ajouter > Nouveau projet**.

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows universel**.

* Sélectionnez le modèle de projet **Application vide (Windows universel)**.

* Nommez le projet *Blogging.UWP*, puis cliquez sur **OK**.

> [!IMPORTANT]
> Définissez les versions minimale et cible sur **Windows 10 Fall Creators Update (10.0 ; build 16299.0)** au minimum.
> Les versions précédentes de Windows 10 ne prennent pas en charge .NET Standard 2.0, qui est exigé par Entity Framework Core.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Ajouter du code pour créer la base de données au démarrage de l’application

Étant donné que vous souhaitez créer la base de données sur l’appareil sur lequel l’application s’exécute, vous allez ajouter du code pour appliquer à la base de données locale toutes les migrations en attente au démarrage de l’application. Lors de la première exécution de l’application, ce code se chargera de créer la base de données locale.

* Ajoutez une référence de projet du projet *Blogging.UWP* au projet *Blogging.Model*.

* Ouvrez *App.xaml.cs*.

* Ajoutez le code mis en surbrillance pour appliquer toutes les migrations en attente.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Si vous modifiez votre modèle, utilisez la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration afin d’appliquer les modifications correspondantes à la base de données. Les migrations en attente seront appliquées à la base de données locale sur chaque appareil au démarrage de l’application.
>
>EF Core utilise une table `__EFMigrationsHistory` dans la base de données pour effectuer le suivi des migrations qui ont déjà été appliquées à la base de données.

## <a name="use-the-data-model"></a>Utiliser le modèle de données

Vous pouvez à présent utiliser EF Core pour accéder aux données.

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
