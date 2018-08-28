---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995295"
---
<a name="ef-core-net-command-line-tools"></a>Outils de ligne de commande du .NET Core EF
===============================
Les outils de ligne de commande de .NET Entity Framework Core sont une extension multiplateforme **dotnet** commande, qui fait partie de la [du SDK .NET Core][2].

> [!TIP]
> Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] au lieu de cela car ils fournissent une expérience plus intégrée.

<a name="installing-the-tools"></a>Installation des outils
--------------------
> [!NOTE]
> Le SDK .NET Core 2.1.300 de version et la plus récente inclut **dotnet ef** commandes qui sont compatibles avec EF Core 2.0 et versions ultérieures. Par conséquent, si vous utilisez des versions récentes du SDK .NET Core et le runtime EF Core, aucune installation n’est requise et vous pouvez ignorer le reste de cette section.
>
> En revanche, le **dotnet ef** outil de relation contenant-contenu dans la version du SDK .NET Core 2.1.300 et les versions ultérieures n’est pas compatible avec la version d’EF Core 1.0 et 1.1. Avant que vous pouvez travailler avec un projet qui utilise les versions antérieures d’EF Core sur un ordinateur disposant du SDK .NET Core 2.1.300 ou version ultérieure, vous devez également installer la version 2.1.200 ou une version antérieure du Kit de développement et de configurer l’application pour utiliser cette ancienne version en modifiant son  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) fichier. Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet). Vous pouvez ensuite poursuivre l’instruction 2161DS ci-dessous.

Pour les versions précédentes du SDK .NET Core, vous pouvez installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :

1. Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)
2. Exécutez les commandes suivantes :

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Le projet résultant doit ressembler à ceci :

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Une référence de package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.

Si vous l’avez fait tout bien, il se peut que vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>L’utilisation des outils
---------------
Chaque fois que vous appelez une commande, deux projets sont impliqués :

Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés). Le projet cible par défaut pour le projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.

Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet. Également par défaut est le projet dans le répertoire actif, elle peut être modifié à l’aide de la **--projet de démarrage** option.

> [!NOTE]
> Par exemple, la mise à jour de la base de données de votre application web avec EF Core est installé dans un autre projet se présenterait comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’applications web)

Options courantes :

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Afficher la sortie JSON.           |
| -c | --contexte \<DBCONTEXT >           | DbContext à utiliser.       |
| -p | --projet \<projet >             | Le projet à utiliser.         |
| -s | --projet de démarrage \<projet >     | Le projet de démarrage à utiliser. |
|    | --framework \<FRAMEWORK >         | Le framework cible.       |
|    | --configuration \<CONFIGURATION > | Configuration à utiliser.   |
|    | --exécution \<identificateur >          | Le runtime à utiliser.         |
| -h | --aide                           | Afficher les informations d’aide.      |
| -v | --verbose                        | Afficher la sortie détaillée.        |
|    | --Aucune couleur                       | Ne pas mettre en couleur sortie.      |
|    | --sortie de préfixe                  | Préfixe avec le niveau de sortie.   |


> [!TIP]
> Pour spécifier l’environnement ASP.NET Core, définissez le **ASPNETCORE_ENVIRONMENT** variable d’environnement avant l’exécution.

<a name="commands"></a>Commandes
--------

### <a name="dotnet-ef-database-drop"></a>liste de base de données ef dotnet

Supprime la base de données.

Options :

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --force   | Ne pas confirmer.                                           |
|    | --dry-run | Afficher la base de données serait supprimée, mais ne la supprimez. |

### <a name="dotnet-ef-database-update"></a>mise à jour de la base de données DotNet ef

Met à jour la base de données pour une migration spécifiée.

Arguments :

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRATION &GT; | La migration de la cible. Si 0, toutes les migrations seront annulées. Valeur par défaut est la dernière migration. |

### <a name="dotnet-ef-dbcontext-info"></a>informations de dbcontext ef dotnet

Obtient des informations sur un type DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>liste de dbcontext ef dotnet

Répertorie les types de DbContext disponibles.

### <a name="dotnet-ef-dbcontext-scaffold"></a>structure de dbcontext ef dotnet

Permet de générer automatiquement un types DbContext et d’entité pour une base de données.

Arguments :

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<CONNEXION &GT; | La chaîne de connexion à la base de données.                                      |
| \<FOURNISSEUR &GT;   | Le fournisseur à utiliser. (par exemple, Microsoft.EntityFrameworkCore.SqlServer) |

Options :

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --annotations de données                      | Utilisez des attributs pour configurer le modèle (le cas échéant). Si omis, uniquement l’API fluent est utilisé. |
| -c              | --contexte \<nom >                       | Le nom de la classe DbContext.                                                                       |
|                 | --contexte-dir \<chemin d’accès >                   | Le répertoire de placer le fichier de DbContext dans. Chemins d’accès sont relatif au répertoire de projet.             |
| -f              | --force                                 | Remplacer les fichiers existants.                                                                        |
| -o              | --sortie-dir \<chemin d’accès >                    | Répertoire à placer les fichiers dans. Chemins d’accès sont relatif au répertoire de projet.                      |
|                 | <nobr>--schéma \<SCHEMA_NAME >...</nobr> | Les schémas des tables pour générer des types d’entité.                                              |
| -t              | --table \<nom_table >...                | Les tables pour générer des types d’entité.                                                         |
|                 | ---base de données-utiliser des noms                    | Utilisez des noms de table et de colonne directement à partir de la base de données.                                           |

### <a name="dotnet-ef-migrations-add"></a>ajouter des migrations d’ef dotnet

Ajoute une nouvelle migration.

Arguments :

|         |                            |
|:--------|:---------------------------|
| \<NOM &GT; | Le nom de la migration. |

Options :

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--sortie-dir \<chemin d’accès ></nobr> | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire de projet. La valeur par défaut est « Migrations ». |

### <a name="dotnet-ef-migrations-list"></a>liste de dotnet ef migrations

Répertorie les migrations disponibles.

### <a name="dotnet-ef-migrations-remove"></a>supprimer des migrations d’ef dotnet

Supprime la dernière migration.

Options :

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --force | Rétablir la migration s’il a été appliqué à la base de données. |

### <a name="dotnet-ef-migrations-script"></a>script de dotnet ef migrations

Génère un script SQL à partir de migrations.

Arguments :

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<À PARTIR DE &GT; | La migration de départ. La valeur par défaut est 0 (la base de données initiale). |
| \<POUR &GT;   | La migration de fin. Valeur par défaut est la dernière migration.         |

Options :

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --sortie \<fichier > | Fichier dans lequel écrire le résultat.                                   |
| -i | --idempotent     | Générer un script qui peut être utilisé sur toute migration d’une base de données. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
