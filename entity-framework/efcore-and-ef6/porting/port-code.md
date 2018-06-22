---
title: Portage de EF6 vers Core EF - portage d’un modèle basé sur le Code
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052949"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portage d’un modèle de Code EF6 vers EF Core

Si vous avez lu toutes les mises en garde et que vous êtes prêt à un port, voici quelques indications pour vous aider à démarrer.

## <a name="install-ef-core-nuget-packages"></a>Installer les packages NuGet de noyaux EF

Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de base de données que vous souhaitez utiliser. Par exemple, si vous ciblez SQL Server, vous installez `Microsoft.EntityFrameworkCore.SqlServer`. Consultez [les fournisseurs de base de données](../../core/providers/index.md) pour plus d’informations.

Si vous envisagez d’utiliser la migration, vous devez également installer le `Microsoft.EntityFrameworkCore.Tools` package.

Il convient de laisser le package NuGet de EF6 (EntityFramework) est installé, comme EF Core et EF6 peuvent être utilisé côte à côte dans la même application. Toutefois, si vous n’êtes pas l’intention d’utiliser des EF6 dans les zones de votre application, puis désinstaller le package vous permettent d’exercer des erreurs de compilation sur les segments de code nécessitant une attention particulière.

## <a name="swap-namespaces"></a>Remplacez les espaces de noms

La plupart des API que vous utilisez dans EF6 se trouvent dans le `System.Data.Entity` espace de noms (et les sous-espaces de noms). La première modification de code est disponible pour le remplacement à la `Microsoft.EntityFrameworkCore` espace de noms. Vous commencent généralement par votre fichier de code de contexte dérivée et puis élaborer à partir de là, traite les erreurs de compilation lorsqu’elles se produisent.

## <a name="context-configuration-connection-etc"></a>Configuration du contexte de (connexion etc..)

Comme décrit dans [Vérifiez EF travail principal sera lié pour votre Application](ensure-requirements.md), EF Core a moins magic autour de détection pour se connecter à la base de données. Vous devez remplacer le `OnConfiguring` méthode sur votre contexte dérivée et utilisez l’API de fournisseur de base de données spécifique pour le programme d’installation de la connexion à la base de données.

La plupart des applications EF6 stocker la chaîne de connexion dans les applications `App/Web.config` fichier. Dans EF Core, vous lisez cette chaîne de connexion à l’aide de la `ConfigurationManager` API. Vous devrez peut-être ajouter une référence à la `System.Configuration` assembly framework pour être en mesure d’utiliser cette API.

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

## <a name="update-your-code"></a>Mettre à jour votre code.

À ce stade, il est une question d’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement auront un impact.

## <a name="existing-migrations"></a>Migrations existantes

Il n’est pas vraiment possible permettent de déplacer des migrations EF6 existantes vers EF Core.

Si possible, il est préférable de supposer que toutes les migrations précédentes à partir de EF6 ont été appliquées à la base de données et de puis démarrer le schéma de la migration à partir de ce point à l’aide de EF Core. Pour ce faire, vous devez utiliser le `Add-Migration` commande pour ajouter une migration, une fois que le modèle est déplacée vers EF Core. Vous pouvez supprimer puis de tout le code à partir de la `Up` et `Down` méthodes de la migration du modèle généré automatiquement. Les migrations suivantes compare au modèle lorsque par la migration initiale a été structurée.

## <a name="test-the-port"></a>Le port de test

Le simple fait que votre application est compilé, ne signifie pas qu’il est correctement déplacée vers EF Core. Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont affectées votre application.
