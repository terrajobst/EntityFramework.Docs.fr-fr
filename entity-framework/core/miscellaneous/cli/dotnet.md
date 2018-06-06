---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754494"
---
<a name="ef-core-net-command-line-tools"></a>Outils de ligne de commande du .NET Core EF
===============================
Les outils de ligne de commande Entity Framework Core .NET sont une extension de l’inter-plateformes **dotnet** commande, qui fait partie de la [le Kit de développement .NET Core][2].

> [!TIP]
> Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] à la place car ils fournissent une expérience intégrée.

<a name="installing-the-tools"></a>Installation des outils
--------------------
> [!NOTE]
> Le .NET Core SDK version 2.1.300 et inclut plus récente **dotnet ef** commandes qui sont compatibles avec EF Core 2.0 et versions ultérieures. Par conséquent, si vous utilisez des versions récentes de .NET Core SDK et le runtime EF Core, aucune installation n’est requise et vous pouvez ignorer le reste de cette section.
>
> En revanche, le **dotnet ef** outil contenu dans la version du Kit de développement .NET Core 2.1.300 et les versions ultérieures n’est pas compatible avec la version EF Core 1.0 et 1.1. Avant que vous pouvez travailler avec un projet qui utilise ces versions antérieures d’EF principal sur un ordinateur qui a le Kit de développement logiciel .NET Core 2.1.300 ou version plus récente, vous devez également installer la version 2.1.200 ou antérieure du Kit de développement et de configurer l’application pour utiliser cette version antérieure en modifiant son  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) fichier. Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet). Vous pouvez ensuite poursuivre l’instruction 2161DS ci-dessous.

Pour les versions précédentes du Kit de développement .NET Core, vous pouvez installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :

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
> Une référence au package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, ce qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.

Si vous avez tout à droite, vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>L’utilisation des outils
---------------
Chaque fois que vous appelez une commande, il existe deux projets concernés :

Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés). Le projet cible par défaut du projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.

Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet. Également par défaut est le projet dans le répertoire actif, il peut être modifié à l’aide de la **--projet de démarrage** option.

> [!NOTE]
> Par exemple, la mise à jour de la base de données de votre application web qui a un cœur EF installé dans un autre projet se présente comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’application web)

Options courantes :

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Afficher la sortie JSON.           |
| -c | --contexte \<DBCONTEXT >           | DbContext à utiliser.       |
| -p | --projet \<projet >             | Le projet à utiliser.         |
| -s | --projet de démarrage \<projet >     | Le projet de démarrage à utiliser. |
|    | --framework \<FRAMEWORK >         | Le framework cible.       |
|    | --configuration \<CONFIGURATION > | Configuration à utiliser.   |
|    | --runtime \<identificateur >          | Le runtime à utiliser.         |
| -h | --aide                           | Afficher les informations d’aide.      |
| -v | --détaillé                        | Afficher la sortie des commentaires.        |
|    | --Aucune-couleur                       | Ne pas redéfinir la sortie.      |
|    | --sortie de préfixe                  | Préfixe dont le niveau de sortie.   |


> [!TIP]
> Pour spécifier l’environnement ASP.NET Core, définissez la **ASPNETCORE_ENVIRONMENT** variable d’environnement avant d’exécuter.

<a name="commands"></a>Commandes
--------

### <a name="dotnet-ef-database-drop"></a>suppression de base de données ef dotnet

Supprime la base de données.

Options :

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --forcer   | Ne pas confirmer.                                           |
|    | --exécution | Afficher la base de données qui seront supprimés, mais ne pas supprimer. |

### <a name="dotnet-ef-database-update"></a>mise à jour de la base de données DotNet ef

Met à jour la base de données pour une migration spécifiée.

Arguments :

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRATION &GT; | La migration de cible. Si 0, toutes les migrations vont être annulées. Valeur par défaut est la dernière migration. |

### <a name="dotnet-ef-dbcontext-info"></a>informations de dbcontext ef dotnet

Obtient des informations sur un type de DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>liste de dbcontext ef dotnet

Répertorie les types DbContext disponibles.

### <a name="dotnet-ef-dbcontext-scaffold"></a>structure de dbcontext ef dotnet

Structures un types DbContext et l’entité pour une base de données.

Arguments :

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<CONNEXION &GT; | La chaîne de connexion à la base de données.                                      |
| \<FOURNISSEUR &GT;   | Le fournisseur à utiliser. (par exemple, Microsoft.EntityFrameworkCore.SqlServer) |

Options :

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --annotations de données                      | Utilisez des attributs pour configurer le modèle (le cas échéant). Si omis, uniquement l’API fluent est utilisé. |
| -c              | --contexte \<nom >                       | Le nom de la DbContext.                                                                       |
|                 | --contexte-dir \<chemin d’accès >                   | Répertoire à placer dans DbContext. Chemins d’accès sont relatif au répertoire du projet.             |
| -f              | --forcer                                 | Remplacer les fichiers existants.                                                                        |
| -o              | --sortie-dir \<chemin d’accès >                    | Répertoire à placer les fichiers dans. Chemins d’accès sont relatif au répertoire du projet.                      |
|                 | <nobr>--schéma \<SCHEMA_NAME >...</nobr> | Les schémas des tables pour générer des types d’entités.                                              |
| -t              | --table \<TABLE_NAME >...                | Les tables pour générer des types d’entités.                                                         |
|                 | --noms de base de données d’utilisation                    | Utilisez des noms de table et de colonne directement à partir de la base de données.                                           |

### <a name="dotnet-ef-migrations-add"></a>ajouter des migrations d’ef dotnet

Ajoute une nouvelle migration.

Arguments :

|         |                            |
|:--------|:---------------------------|
| \<NOM &GT; | Le nom de la migration. |

Options :

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--sortie-dir \<chemin d’accès ></nobr> | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire du projet. La valeur par défaut est « Migration ». |

### <a name="dotnet-ef-migrations-list"></a>liste de migrations ef dotnet

Répertorie les migrations disponibles.

### <a name="dotnet-ef-migrations-remove"></a>supprimer des migrations d’ef dotnet

Supprime la dernière migration.

Options :

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --forcer | Rétablir la migration si elle a été appliquée à la base de données. |

### <a name="dotnet-ef-migrations-script"></a>script de migrations ef dotnet

Génère un script SQL à partir de la migration.

Arguments :

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<À PARTIR DE &GT; | La migration de départ. La valeur par défaut est 0 (base de données initiale). |
| \<POUR &GT;   | La fin de la migration. Valeur par défaut est la dernière migration.         |

Options :

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --sortie \<fichier > | Le fichier dans lequel écrire le résultat à.                                   |
| -i | --idempotent     | Générer un script qui peut être utilisé sur toute migration, une base de données. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
