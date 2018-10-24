---
title: Bien démarrer avec .NET Framework - Base de données existante - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: b9e079f88dd35016407b19bb627f8bd46edb3d4c
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447155"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Bien démarrer avec EF Core sur .NET Framework avec une base de données existante

Dans ce tutoriel, vous générez une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework. Vous créez un modèle Entity Framework en rétroconcevant une base de données existante.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017 version 15.7 ou ultérieur](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Créer une base de données de création de blogs

Ce tutoriel utilise une base de données de **création de blogs** sur l’instance LocalDb comme base de données existante. Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.

* Ouvrir Visual Studio

* **Outils > Connexion à une base de données...**

* Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.

* Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.

* Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.

* La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.

* Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.

* Copiez le script ci-dessous dans l’éditeur de requêtes.

* Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Créer un projet

* Ouvrez Visual Studio 2017.

* **Fichier > Nouveau > Projet...**

* Dans le menu de gauche, sélectionnez **Installé > Visual C# > Windows Desktop**.

* Sélectionnez le modèle de projet **Application console (.NET Framework)**.

* Vérifiez que le projet cible le **.NET Framework 4.6.1** ou ultérieur.

* Nommez le projet *ConsoleApp.ExistingDb* et cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Ce tutoriel utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Dans l’étape suivante, vous utilisez des outils Entity Framework pour rétroconcevoir la base de données. Installez donc le package d’outils.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="reverse-engineer-the-model"></a>Rétroconcevoir le modèle

Créons à présent le modèle EF à partir d’une base de données existante.

* **Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**

* Exécutez la commande suivante pour créer un modèle à partir de la base de données existante.

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus. Par exemple, `-Tables Blog,Post`.

Le processus d’ingénierie à rebours créé des classes d’entité (`Blog` et `Post`) et un contexte dérivé (`BloggingContext`) en fonction du schéma de la base de données existante.

Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer. Voici les classes d’entité `Blog` et `Post` :

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).

Le contexte représente une session avec la base de données. Il contient des méthodes que vous pouvez utiliser pour interroger et enregistrer des instances des classes d’entité.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *Program.cs*.

* Remplacez le contenu du fichier par le code suivant.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Déboguer > Exécuter sans débogage

  Vous voyez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.

  ![image](_static/output-existing-db.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la façon de structurer un contexte et des classes d’entité, consultez les articles suivants :
* [Référence des outils Entity Framework Core - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Référence des outils Entity Framework Core - Console du Gestionnaire de Package](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)