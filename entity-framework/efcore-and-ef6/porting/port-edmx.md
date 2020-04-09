---
title: Portage de EF6 à EF Core - Porting an EDMX-Based Model - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416922"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portage d’un modèle EF6 basé sur EDMX à EF Core

EF Core ne prend pas en charge le format de fichier EDMX pour les modèles. La meilleure option pour porter ces modèles, est de générer un nouveau modèle basé sur le code à partir de la base de données pour votre application.

## <a name="install-ef-core-nuget-packages"></a>Installer des paquets EF Core NuGet

Installez le package NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Régénérer le modèle

Vous pouvez maintenant utiliser la fonctionnalité de l’ingénieur inversé pour créer un modèle basé sur votre base de données existante.

Exécutez la commande suivante dans la console de gestionnaire de paquets (Outils > NuGet Package Manager - > console de gestionnaire de paquets). Voir [La console De gestionnaire de paquet (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pour les options de commande pour échafauder un sous-ensemble de tables, etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Par exemple, voici la commande d’échafaudage d’un modèle de la base de données Blogging sur votre instance SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Supprimer le modèle EF6

Vous supprimeriez maintenant le modèle EF6 de votre application.

Il est bon de laisser le paquet EF6 NuGet (EntityFramework) installé, comme EF Core et EF6 peuvent être utilisés côte à côte dans la même application. Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans tous les domaines de votre application, puis le désinstallation du paquet aidera à compiler des erreurs sur les morceaux de code qui nécessitent une attention particulière.

## <a name="update-your-code"></a>Mettre à jour votre code

À ce stade, il s’agit de traiter les erreurs de compilation et de revoir le code pour voir si les changements de comportement entre EF6 et EF Core auront un impact sur vous.

## <a name="test-the-port"></a>Tester le port

Ce n’est pas parce que votre application est compilée qu’elle est portée avec succès à EF Core. Vous devrez tester tous les domaines de votre application pour vous assurer qu’aucun des changements de comportement n’a eu d’impact négatif sur votre application.
