---
title: Portage de EF6 à EF Core - Porting a Code-Based Model - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419636"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portage d’un modèle basé sur le code EF6 à EF Core

Si vous avez lu toutes les mises en garde et que vous êtes prêt à bâbord, alors voici quelques lignes directrices pour vous aider à démarrer.

## <a name="install-ef-core-nuget-packages"></a>Installer des paquets EF Core NuGet

Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de bases de données que vous souhaitez utiliser. Par exemple, lorsque vous ciblez SQL Server, vous installeriez `Microsoft.EntityFrameworkCore.SqlServer`. Consultez [les fournisseurs de bases de données](../../core/providers/index.md) pour plus de détails.

Si vous prévoyez d’utiliser les migrations, `Microsoft.EntityFrameworkCore.Tools` alors vous devez également installer le paquet.

Il est bon de laisser le paquet EF6 NuGet (EntityFramework) installé, comme EF Core et EF6 peuvent être utilisés côte à côte dans la même application. Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans tous les domaines de votre application, puis le désinstallation du paquet aidera à compiler des erreurs sur les morceaux de code qui nécessitent une attention particulière.

## <a name="swap-namespaces"></a>Échanger des espaces de nom

La plupart des API que vous `System.Data.Entity` utilisez dans EF6 sont dans l’espace nom (et les sous-noms connexes). La première modification de code `Microsoft.EntityFrameworkCore` est d’échanger vers l’espace nom. Vous commencez généralement avec votre fichier de code contextuelle dérivé, puis travailler à partir de là, en abordant les erreurs de compilation au fur et à mesure qu’elles se produisent.

## <a name="context-configuration-connection-etc"></a>Configuration contextuelle (connexion, etc.)

Comme décrit dans [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core a moins de magie autour de la détection de la base de données pour se connecter à. Vous devrez passer outre `OnConfiguring` à la méthode sur votre contexte dérivé, et utiliser l’API spécifique au fournisseur de base de données pour configurer la connexion à la base de données.

La plupart des applications EF6 `App/Web.config` stockent la chaîne de connexion dans le fichier des applications. Dans EF Core, vous lisez `ConfigurationManager` cette chaîne de connexion à l’aide de l’API. Vous devrez peut-être ajouter `System.Configuration` une référence à l’assemblage-cadre pour pouvoir utiliser cette API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Mettre à jour votre code

À ce stade, il s’agit de traiter les erreurs de compilation et d’examiner le code pour voir si les changements de comportement auront un impact sur vous.

## <a name="existing-migrations"></a>Migrations existantes

Il n’y a pas vraiment de moyen faisable de transférer les migrations EF6 existantes vers EF Core.

Si possible, il est préférable de supposer que toutes les migrations précédentes de EF6 ont été appliquées à la base de données, puis commencer à migrer le schéma à partir de ce point en utilisant EF Core. Pour ce faire, vous `Add-Migration` utiliseriez la commande pour ajouter une migration une fois que le modèle est porté à EF Core. Vous supprimeriez alors tout `Up` `Down` code de la migration et des méthodes de la migration échafaudée. Les migrations subséquentes se compareront au modèle lorsque cette migration initiale a été échafaudée.

## <a name="test-the-port"></a>Tester le port

Ce n’est pas parce que votre application est compilée qu’elle est portée avec succès à EF Core. Vous devrez tester tous les domaines de votre application pour vous assurer qu’aucun des changements de comportement n’a eu d’impact négatif sur votre application.
