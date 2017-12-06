---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a>Outils de ligne de commande du .NET Core EF
===============================
Les outils de ligne de commande Entity Framework Core .NET sont une extension de l’inter-plateformes **dotnet** commande, qui fait partie de la [le Kit de développement .NET Core][2].

> [!TIP]
> Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] à la place car ils fournissent une expérience intégrée.

<a name="installing-the-tools"></a>Installation des outils
--------------------
Installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :

1. Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)
2. Exécutez les commandes suivantes :

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

Le projet cible est où les fichiers sont ajoutés (ou dans certains cas supprimés). Le projet cible par défaut du projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.

Le projet de démarrage est celui émulé par les outils lors de l’exécution de code de votre projet. Également par défaut est le projet dans le répertoire actif, il peut être modifié à l’aide de la **--projet de démarrage** option.

Options courantes :

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | --json                           | Afficher la sortie JSON.           |
| -c | --contexte \<DBCONTEXT >           | DbContext à utiliser.       |
| -P | --projet \<projet >             | Le projet à utiliser.         |
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
| -- | --------- | -------------------------------------------------------- |
| -f | --forcer   | Ne pas confirmer.                                           |
|    | --exécution | Afficher la base de données qui seront supprimés, mais ne pas supprimer. |

### <a name="dotnet-ef-database-update"></a>mise à jour de la base de données DotNet ef

Met à jour la base de données pour une migration spécifiée.

Arguments :

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| \<MIGRATION > | La migration de cible. Si 0, toutes les migrations vont être annulées. Valeur par défaut est la dernière migration. |

### <a name="dotnet-ef-dbcontext-info"></a>informations de dbcontext ef dotnet

Obtient des informations sur un type de DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>liste de dbcontext ef dotnet

Répertorie les types DbContext disponibles.

### <a name="dotnet-ef-dbcontext-scaffold"></a>structure de dbcontext ef dotnet

Structures un types DbContext et l’entité pour une base de données.

Arguments :

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| \<CONNEXION > | La chaîne de connexion à la base de données.                              |
| \<FOURNISSEUR >   | Le fournisseur à utiliser. (Par exemple : Microsoft.EntityFrameworkCore.SqlServer) |

Options :

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <nobr>-d</nobr> |       --annotations de données                | Utilisez des attributs pour configurer le modèle (le cas échéant). Si omis, uniquement l’API fluent est utilisé. |
|       -c        |       --contexte \<nom >                 | Le nom de la DbContext.                               |
|       -f        |       --forcer                           | Remplacer les fichiers existants.                                |
|       -o        |       --sortie-dir \<chemin d’accès >              | Répertoire à placer les fichiers dans. Chemins d’accès sont relatif au répertoire du projet. |
|                 | <nobr>--schéma \<SCHEMA_NAME >...</nobr> | Les schémas des tables pour générer des types d’entités.      |
|       -t        |       --table \<TABLE_NAME >...          | Les tables pour générer des types d’entités.                 |
|                 |       --noms de base de données d’utilisation              | Utilisez des noms de table et de colonne directement à partir de la base de données.   |

### <a name="dotnet-ef-migrations-add"></a>ajouter des migrations d’ef dotnet

Ajoute une nouvelle migration.

Arguments :

|         |                            |
| ------- | -------------------------- |
| \<NOM > | Le nom de la migration. |

Options :

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <nobr>-o</nobr> | <nobr>--sortie-dir \<chemin d’accès ></nobr> | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire du projet. La valeur par défaut est « Migration ». |

### <a name="dotnet-ef-migrations-list"></a>liste de migrations ef dotnet

Répertorie les migrations disponibles.

### <a name="dotnet-ef-migrations-remove"></a>supprimer des migrations d’ef dotnet

Supprime la dernière migration.

Options :

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| -f | --forcer | Ne pas vérifier si la migration a été appliquée à la base de données. |

### <a name="dotnet-ef-migrations-script"></a>script de migrations ef dotnet

Génère un script SQL à partir de la migration.

Arguments :

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| \<À PARTIR DE > | La migration de départ. La valeur par défaut est 0 (base de données initiale). |
| \<POUR >   | La fin de la migration. Valeur par défaut est la dernière migration.         |

Options :

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| -o | --sortie \<fichier > | Le fichier dans lequel écrire le résultat à.                                   |
| -i | --idempotent     | Générer un script qui peut être utilisé sur toute migration, une base de données. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
