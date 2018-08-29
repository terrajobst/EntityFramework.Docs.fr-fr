---
title: Portage depuis EF6 vers EF Core - portage d’un modèle basé sur un EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997409"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portage d’un modèle EDMX EF6 vers EF Core

EF Core ne prend pas en charge le format de fichier EDMX pour les modèles. La meilleure option pour ces modèles, le port consiste à générer un nouveau modèle basé sur le code à partir de la base de données pour votre application.

## <a name="install-ef-core-nuget-packages"></a>Installer les packages NuGet de EF Core

Installer le `Microsoft.EntityFrameworkCore.Tools` package NuGet.

## <a name="regenerate-the-model"></a>Régénérer le modèle

Vous pouvez maintenant utiliser la fonctionnalité d’ingénierie à rebours pour créer un modèle basé sur votre base de données existante.

Exécutez la commande suivante dans la Console du Gestionnaire de Package (Outils -> Gestionnaire de Package NuGet -> Console du Gestionnaire de Package). Consultez [Console du Gestionnaire de Package (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour les options de commande Générer automatiquement un sous-ensemble de tables, etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Par exemple, voici la commande pour générer automatiquement un modèle à partir de la base de données de création de blogs sur votre instance de SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Supprimer le modèle EF6

Vous devez maintenant supprimer le modèle de EF6 à partir de votre application.

Il est possible de laisser le package NuGet d’EF6 (Entity Framework) est installé, comme EF Core et EF6 peuvent être utilisé côté à côte dans la même application. Toutefois, si vous n’êtes pas l’intention d’utiliser EF6 dans les zones de votre application, puis la désinstallation du package vous permettent d’exercer des erreurs de compilation sur les éléments de code nécessitant votre attention.

## <a name="update-your-code"></a>Mettre à jour votre code

À ce stade, il est une question de l’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement entre EF6 et EF Core aura un impact sur vous.

## <a name="test-the-port"></a>Le port de test

Parce que votre application est compilée, ne signifie pas qu’il est correctement déplacée vers EF Core. Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont défavorablement affectées de votre application.

> [!TIP]
> Consultez [mise en route avec EF Core sur ASP.NET Core avec une base de données existante](xref:core/get-started/aspnetcore/existing-db) pour une référence supplémentaire sur la façon de travailler avec une base de données existante 
