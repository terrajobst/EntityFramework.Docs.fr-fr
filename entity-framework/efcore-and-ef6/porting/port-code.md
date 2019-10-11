---
title: Portage à partir de EF6 vers EF Core-Portage d’un modèle basé sur du code-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181213"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portage d’un modèle basé sur du code EF6 vers EF Core

Si vous avez lu tous les avertissements et que vous êtes prêt à utiliser le port, voici quelques conseils pour vous aider à démarrer.

## <a name="install-ef-core-nuget-packages"></a>Installer EF Core des packages NuGet

Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de base de données que vous souhaitez utiliser. Par exemple, lorsque vous ciblez SQL Server, vous devez installer `Microsoft.EntityFrameworkCore.SqlServer`. Pour plus d’informations, consultez [fournisseurs de bases de données](../../core/providers/index.md) .

Si vous envisagez d’utiliser des migrations, vous devez également installer le package `Microsoft.EntityFrameworkCore.Tools`.

Il est parfait de conserver le package NuGet EF6 (EntityFramework) installé, car EF Core et EF6 peuvent être utilisés côte à côte dans la même application. Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans les zones de votre application, la désinstallation du package permet d’obtenir des erreurs de compilation sur des portions de code nécessitant votre attention.

## <a name="swap-namespaces"></a>Permuter les espaces de noms

La plupart des API que vous utilisez dans EF6 se trouvent dans l’espace de noms `System.Data.Entity` (et les sous-espaces de noms associés). La première modification du code consiste à basculer vers l’espace de noms `Microsoft.EntityFrameworkCore`. En général, vous démarrez avec votre fichier de code de contexte dérivé, puis vous travaillez à partir de là, en résolvant les erreurs de compilation à mesure qu’elles se produisent.

## <a name="context-configuration-connection-etc"></a>Configuration du contexte (connexion, etc.)

Comme décrit dans [la section s’assurer EF Core fonctionne pour votre application](ensure-requirements.md), EF Core a moins de magie pour détecter la base de données à laquelle se connecter. Vous devez remplacer la méthode `OnConfiguring` sur votre contexte dérivé et utiliser l’API spécifique du fournisseur de base de données pour configurer la connexion à la base de données.

La plupart des applications EF6 stockent la chaîne de connexion dans le fichier des applications `App/Web.config`. Dans EF Core, vous lisez cette chaîne de connexion à l’aide de l’API `ConfigurationManager`. Vous devrez peut-être ajouter une référence à l’assembly de Framework `System.Configuration` pour pouvoir utiliser cette API.

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

À ce stade, il s’agit de traiter les erreurs de compilation et de consulter le code pour voir si les changements de comportement ont un impact sur vous.

## <a name="existing-migrations"></a>Migrations existantes

Il n’existe pas vraiment un moyen pratique de porter des migrations EF6 existantes vers EF Core.

Si possible, il est préférable de supposer que toutes les migrations précédentes de EF6 ont été appliquées à la base de données, puis de commencer à migrer le schéma à partir de ce point à l’aide de EF Core. Pour ce faire, vous devez utiliser la commande `Add-Migration` pour ajouter une migration une fois que le modèle est porté sur EF Core. Vous supprimez ensuite tout le code des méthodes `Up` et `Down` de la migration par génération de modèles automatique. Les migrations suivantes seront comparées au modèle lors de la génération de modèles automatique de la migration initiale.

## <a name="test-the-port"></a>Tester le port

Simplement parce que votre application est compilée, ne signifie pas qu’elle est correctement reportée vers EF Core. Vous devrez tester toutes les zones de votre application pour vous assurer qu’aucune des modifications de comportement n’a un impact négatif sur votre application.
