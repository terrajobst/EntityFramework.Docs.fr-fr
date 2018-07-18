---
title: Versions de Visual Studio, EF6
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
caps.latest.revision: 3
ms.openlocfilehash: 7bd08a46b1d6acc5a565952e834f01546a5262c8
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120792"
---
# <a name="visual-studio-releases"></a>Versions de Visual Studio

Nous vous recommandons de toujours utiliser la dernière version de Visual Studio, car elle contient les derniers outils pour .NET, NuGet et Entity Framework.
En fait, les différents exemples et procédures pas à pas dans la documentation d’Entity Framework supposent que vous utilisez une version récente de Visual Studio.

Il est possible cependant, pour utiliser les versions antérieures de Visual Studio avec différentes versions d’Entity Framework, que vous prenez en compte de certaines différences :

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 et ultérieures

- Cette version de Visual Studio inclut la dernière version des outils Entity Framework et le runtime EF 6.2 et ne nécessite pas d’étapes de configuration supplémentaires.
Consultez [What ' s New](~/ef6/what-is-new/index.md) pour plus d’informations sur ces versions.
- Ajout d’Entity Framework pour les nouveaux projets à l’aide des outils Entity Framework ajoute automatiquement le package NuGet EF 6.2.
Vous pouvez manuellement installer ou mettre à niveau vers n’importe quel package NuGet d’EF disponible en ligne.
- Par défaut, l’instance de SQL Server disponible avec cette version de Visual Studio est une instance de base de données locale appelée MSSQLLocalDB.
La section serveur de la chaîne de connexion que vous devez utiliser est « (localdb)\\MSSQLLocalDB ».
N’oubliez pas d’utiliser un préfixe de chaîne textuelle `@` ou doubles barres obliques inverses «\\\\» lorsque vous spécifiez une chaîne de connexion dans le code c#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 vers Visual Studio 2017 15.6

- Ces versions de Visual Studio incluent des outils Entity Framework et runtime 6.1.3.
Consultez [libère dernières](~/ef6/what-is-new/past-releases.md#ef-613) pour plus d’informations sur ces versions.
- Ajout d’Entity Framework pour les nouveaux projets à l’aide des outils Entity Framework ajoute automatiquement le EF 6.1.3 package NuGet.
Vous pouvez manuellement installer ou mettre à niveau vers n’importe quel package NuGet d’EF disponible en ligne.
- Par défaut, l’instance de SQL Server disponible avec cette version de Visual Studio est une instance de base de données locale appelée MSSQLLocalDB.
La section serveur de la chaîne de connexion que vous devez utiliser est « (localdb)\\MSSQLLocalDB ».
N’oubliez pas d’utiliser un préfixe de chaîne textuelle `@` ou doubles barres obliques inverses «\\\\» lorsque vous spécifiez une chaîne de connexion dans le code c#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Cette version de Visual Studio inclut et version antérieure du runtime et les outils Entity Framework.
Il est recommandé de mettre à niveau vers Entity Framework Tools 6.1.3, à l’aide de [le programme d’installation](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibles dans du Microsoft Download Center.
Consultez [libère dernières](~/ef6/what-is-new/past-releases.md#ef-613) pour plus d’informations sur ces versions.
- Ajout d’Entity Framework pour les nouveaux projets à l’aide des outils Entity Framework mis à niveau ajoute automatiquement le EF 6.1.3 package NuGet.
Vous pouvez manuellement installer ou mettre à niveau vers n’importe quel package NuGet d’EF disponible en ligne.
- Par défaut, l’instance de SQL Server disponible avec cette version de Visual Studio est une instance de base de données locale appelée MSSQLLocalDB.
La section serveur de la chaîne de connexion que vous devez utiliser est « (localdb)\\MSSQLLocalDB ».
N’oubliez pas d’utiliser un préfixe de chaîne textuelle `@` ou doubles barres obliques inverses «\\\\» lorsque vous spécifiez une chaîne de connexion dans le code c#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Cette version de Visual Studio inclut et version antérieure du runtime et les outils Entity Framework.
Il est recommandé de mettre à niveau vers Entity Framework Tools 6.1.3, à l’aide de [le programme d’installation](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibles dans du Microsoft Download Center.
Consultez [libère dernières](~/ef6/what-is-new/past-releases.md#ef-613) pour plus d’informations sur ces versions.
- Ajout d’Entity Framework pour les nouveaux projets à l’aide des outils Entity Framework mis à niveau ajoute automatiquement le EF 6.1.3 package NuGet.
Vous pouvez manuellement installer ou mettre à niveau vers n’importe quel package NuGet d’EF disponible en ligne.
- Par défaut, l’instance de SQL Server disponible avec cette version de Visual Studio est une instance de base de données locale appelée v11.0.
La section serveur de la chaîne de connexion que vous devez utiliser est « (localdb)\\v11.0 ».
N’oubliez pas d’utiliser un préfixe de chaîne textuelle `@` ou doubles barres obliques inverses «\\\\» lorsque vous spécifiez une chaîne de connexion dans le code c#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- La version d’Entity Framework Tools est disponible avec cette version de Visual Studio n’est pas compatible avec le runtime Entity Framework 6 et ne peut pas être mis à niveau.
- Par défaut, les outils Entity Framework ajoute Entity Framework 4.0 à vos projets.
Pour créer des applications à l’aide des versions les plus récentes d’EF, vous devez d’abord installer le [extension du Gestionnaire de Package NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Par défaut, tous les génération de code dans la version des outils Entity Framework est basé sur EntityObject et Entity Framework 4.
Nous vous recommandons de passer la génération de code doit être basé sur DbContext et Entity Framework 5, en installant les modèles de génération de code de DbContext pour [c#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) ou [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Une fois que vous avez installé les extensions du Gestionnaire de Package NuGet, vous pouvez manuellement installer ou mettre à niveau vers n’importe quel package NuGet d’EF disponible en ligne et utiliser EF6 avec Code First, qui ne nécessite pas d’un concepteur.
- Par défaut, l’instance de SQL Server disponible avec cette version de Visual Studio est SQL Server Express nommée SQLEXPRESS.
La section serveur de la chaîne de connexion que vous devez utiliser est ». \\SQLEXPRESS ».
N’oubliez pas d’utiliser un préfixe de chaîne textuelle `@` ou doubles barres obliques inverses «\\\\» lorsque vous spécifiez une chaîne de connexion dans le code c#.
