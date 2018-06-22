---
title: Portage de EF6 vers Core EF - portage d’un modèle basé sur le fichier EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812688"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portage d’un modèle EDMX EF6 vers EF Core

EF Core ne prend pas en charge le format de fichier EDMX pour les modèles. La meilleure option pour ces modèles, le port doit générer un nouveau modèle basé sur le code à partir de la base de données pour votre application.

## <a name="install-ef-core-nuget-packages"></a>Installer les packages NuGet de noyaux EF

Installer le `Microsoft.EntityFrameworkCore.Tools` package NuGet.

## <a name="regenerate-the-model"></a>Régénérer le modèle

Vous pouvez désormais utiliser les fonctionnalités de l’ingénierie à rebours pour créer un modèle basé sur votre base de données existante.

Exécutez la commande suivante dans la Console du Gestionnaire de Package (Outils -> Gestionnaire de Package NuGet -> Console du Gestionnaire de Package). Consultez [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour structurer un sous-ensemble de tables, etc. des options de commande.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Par exemple, voici la commande pour structurer un modèle à partir de la base de données de création de blogs sur votre instance de base de données SQL Server locale.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Supprimer le modèle de EF6

Vous pouvez maintenant supprimer le modèle de EF6 à partir de votre application.

Il convient de laisser le package NuGet de EF6 (EntityFramework) est installé, comme EF Core et EF6 peuvent être utilisé côte à côte dans la même application. Toutefois, si vous n’êtes pas l’intention d’utiliser des EF6 dans les zones de votre application, puis désinstaller le package vous permettent d’exercer des erreurs de compilation sur les segments de code nécessitant une attention particulière.

## <a name="update-your-code"></a>Mettre à jour votre code.

À ce stade, il est une question d’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement entre EF6 et EF Core auront un impact.

## <a name="test-the-port"></a>Le port de test

Le simple fait que votre application est compilé, ne signifie pas qu’il est correctement déplacée vers EF Core. Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont affectées votre application.

> [!TIP]
> Consultez [mise en route avec EF de base sur ASP.NET Core avec une base de données](xref:core/get-started/aspnetcore/existing-db) pour une référence supplémentaire sur la façon de travailler avec une base de données existante 
