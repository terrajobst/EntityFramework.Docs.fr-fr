---
title: Portage depuis EF6 vers EF Core - portage d’un modèle basé sur le Code
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997047"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portage d’un modèle basé sur le Code EF6 vers EF Core

Si vous avez lu les mises en garde et vous êtes prêt à un port, voici quelques conseils pour vous aider à commencer.

## <a name="install-ef-core-nuget-packages"></a>Installer les packages NuGet de EF Core

Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de base de données que vous souhaitez utiliser. Par exemple, lors du ciblage de SQL Server, que vous installeriez `Microsoft.EntityFrameworkCore.SqlServer`. Consultez [les fournisseurs de base de données](../../core/providers/index.md) pour plus d’informations.

Si vous envisagez d’utiliser des migrations, vous devez également installer le `Microsoft.EntityFrameworkCore.Tools` package.

Il est possible de laisser le package NuGet d’EF6 (Entity Framework) est installé, comme EF Core et EF6 peuvent être utilisé côté à côte dans la même application. Toutefois, si vous n’êtes pas l’intention d’utiliser EF6 dans les zones de votre application, puis la désinstallation du package vous permettent d’exercer des erreurs de compilation sur les éléments de code nécessitant votre attention.

## <a name="swap-namespaces"></a>Remplacez les espaces de noms

La plupart des API que vous utilisez dans EF6 se trouvent dans le `System.Data.Entity` espace de noms (et liés sous-espaces de noms). La première modification de code est disponible pour le remplacement à la `Microsoft.EntityFrameworkCore` espace de noms. Vous commence généralement par votre fichier de code de contexte dérivé et passez ensuite à partir de là, ciblant des erreurs de compilation lorsqu’elles se produisent.

## <a name="context-configuration-connection-etc"></a>Configuration de contexte (connexion etc.).

Comme décrit dans [vous assurer de EF Core sera travail pour votre Application](ensure-requirements.md), EF Core offre moins magic autour de détection pour se connecter à la base de données. Vous devrez remplacer le `OnConfiguring` méthode sur votre contexte dérivé et utiliser l’API spécifique de fournisseur de base de données pour configurer la connexion à la base de données.

La plupart des applications EF6 stocker la chaîne de connexion dans les applications `App/Web.config` fichier. Dans EF Core, vous lisez cette chaîne de connexion à l’aide de la `ConfigurationManager` API. Vous devrez peut-être ajouter une référence à la `System.Configuration` assembly .NET framework à être en mesure d’utiliser cette API.

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

À ce stade, il est une question de l’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement aura un impact sur vous.

## <a name="existing-migrations"></a>Migrations existantes

Il n’est pas vraiment possible permettent de déplacer des migrations EF6 existantes vers EF Core.

Si possible, il est préférable de supposer que toutes les migrations précédentes d’EF6 ont été appliquées à la base de données et migration du schéma de celle de début pointez à l’aide d’EF Core. Pour ce faire, vous utiliseriez le `Add-Migration` commande pour ajouter une migration une fois que le modèle est déplacée vers EF Core. Puis supprimer tout le code à partir de la `Up` et `Down` méthodes de la migration du modèle généré automatiquement. Des migrations compare au modèle lors de la migration initiale a été généré automatiquement.

## <a name="test-the-port"></a>Le port de test

Parce que votre application est compilée, ne signifie pas qu’il est correctement déplacée vers EF Core. Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont défavorablement affectées de votre application.
