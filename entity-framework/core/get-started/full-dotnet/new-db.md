---
title: Bien démarrer avec .NET Framework - Nouvelle base de données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614426"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Bien démarrer avec EF Core sur .NET Framework avec une nouvelle base de données

Dans ce tutoriel, vous générez une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework. Vous utilisez des migrations pour créer la base de données à partir d’un modèle.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017 version 15.7 ou ultérieur](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Créer un projet

* Ouvrez Visual Studio 2017.

* **Fichier > Nouveau > Projet...**

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows Desktop**.

* Sélectionnez le modèle de projet **Application console (.NET Framework)**.

* Vérifiez que le projet cible le **.NET Framework 4.6.1** ou ultérieur.

* Nommez le projet *ConsoleApp.NewDb* et cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Ce tutoriel utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Un peu plus loin dans ce tutoriel, vous utiliserez certains outils Entity Framework pour gérer la base de données. Installez donc le package d’outils.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-the-model"></a>Créer le modèle

Définissons à présent le contexte et les classes d’entité qui composent le modèle.

* **Projet > Ajouter une classe...**

* Entrez le nom *Model.cs* et cliquez sur **OK**.

* Remplacez le contenu du fichier par le code suivant.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> Dans une application réelle, vous placez chaque classe dans un fichier distinct et la chaîne de connexion dans un fichier de configuration ou une variable d’environnement. Par souci de simplicité, ce tutoriel place le tout dans un même fichier de code.

## <a name="create-the-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Exécutez `Add-Migration InitialCreate` pour générer automatiquement un modèle de migration afin de créer le jeu initial de tables du modèle.

* Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données. Étant donné que votre base de données n’existe pas encore, elle sera créée avant l’application de la migration.

> [!TIP]  
> Si vous apportez des changements au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les changements de schéma correspondants à la base de données. Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `Update-Database` pour appliquer les modifications à la base de données.
>
> EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.

## <a name="use-the-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *Program.cs*.

* Remplacez le contenu du fichier par le code suivant.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Déboguer > Démarrer sans débogage**

  Vous voyez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.

  ![image](_static/output-new-db.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [EF Core sur .NET Framework avec une base de données existante](xref:core/get-started/full-dotnet/existing-db)
* [EF Core sur .NET Core avec une nouvelle base de données - SQLite](xref:core/get-started/netcore/new-db-sqlite) : tutoriel sur la console multiplateforme EF.
