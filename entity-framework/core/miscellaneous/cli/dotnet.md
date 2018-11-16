---
title: EF Core références relatives aux outils (CLI) .NET - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 959785c7b10ca668f3691106f62076d538978c03
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688665"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Référence des outils Entity Framework Core - interface CLI .NET

Les outils de l’interface de ligne de commande (CLI) pour Entity Framework Core effectuer des tâches de développement au moment du design. Par exemple, ils créent [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), appliquer des migrations et générer du code pour un modèle basé sur une base de données existante. Les commandes sont une extension multiplateforme [dotnet](/dotnet/core/tools) commande, qui fait partie de la [du SDK .NET Core](https://www.microsoft.com/net/core). Ces outils fonctionnent avec les projets .NET Core.

Si vous utilisez Visual Studio, nous vous recommandons du [outils de la Console du Gestionnaire de Package](powershell.md) à la place :
* Elles fonctionnent automatiquement avec le projet sélectionné dans le **Console du Gestionnaire de Package** sans nécessiter de basculer manuellement les répertoires.
* Elles ouvrent automatiquement les fichiers générés par une commande une fois la commande est terminée.

## <a name="installing-the-tools"></a>Installation des outils

La procédure d’installation dépend de la version et le type de projet :

* ASP.NET Core 2.1 et versions ultérieures
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Installer actuel [du SDK .NET Core](https://www.microsoft.com/net/download/core). Le kit SDK doit être installé même si vous disposez de la dernière version de Visual Studio 2017.

  C’est tout ce qui est nécessaire pour ASP.NET Core 2.1 +, car le `Microsoft.EntityFrameworkCore.Design` package est inclus dans le [Microsoft.AspNetCore.App métapackage](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (pas ASP.NET Core)

Le `dotnet ef` commandes sont inclus dans le SDK .NET Core, mais pour activer les commandes, vous devez installer le `Microsoft.EntityFrameworkCore.Design` package.

* Installer actuel [du SDK .NET Core](https://www.microsoft.com/net/download/core). Le kit SDK doit être installé même si vous disposez de la dernière version de Visual Studio 2017.

* Installer la dernière stable `Microsoft.EntityFrameworkCore.Design` package.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Installer le SDK .NET Core version 2.1.200. Versions ultérieures ne sont pas compatibles avec les outils CLI pour EF Core 1.0 et 1.1.

* Configurer l’application pour utiliser le 2.1.200 SDK version en modifiant son [global.json](/dotnet/core/tools/global-json) fichier. Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet).

* Modifiez le fichier projet et ajoutez `Microsoft.EntityFrameworkCore.Tools.DotNet` comme un `DotNetCliToolReference` élément. Spécifier la dernière version 1.x, par exemple : 1.1.6. Consultez l’exemple de fichier de projet à la fin de cette section.

* Installez la dernière version 1.x de le `Microsoft.EntityFrameworkCore.Design` du package, par exemple :

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Avec les deux références de package ajoutés, le fichier projet ressemble à ceci :

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Une référence de package avec `PrivateAssets="All"` n’est pas exposé à des projets qui font référence à ce projet. Cette restriction est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.

### <a name="verify-installation"></a>Vérifier l’installation

Exécutez les commandes suivantes pour vérifier que les outils CLI EF Core sont correctement installés :

  ``` Console
  dotnet restore
  dotnet ef
  ```

La sortie de la commande identifie la version des outils en cours d’utilisation :

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>L’utilisation des outils

Avant d’utiliser les outils, vous devrez peut-être créer un projet de démarrage ou de définir l’environnement.

### <a name="target-project-and-startup-project"></a>Projet cible et le projet de démarrage

Les commandes font référence à un *projet* et un *projet de démarrage*.

* Le *projet* est également connu sous le *projet cible* , car il est où les commandes ajoutent ou supprimer des fichiers. Par défaut, le projet dans le répertoire actif est le projet cible. Vous pouvez spécifier un autre projet comme projet cible à l’aide de la <nobr> `--project` </nobr> option.

* Le *projet de démarrage* est celui que les outils de générer et exécuter. Les outils ont exécuter du code de l’application au moment du design pour obtenir des informations sur le projet, telles que la chaîne de connexion de base de données et la configuration du modèle. Par défaut, le projet dans le répertoire actif est le projet de démarrage. Vous pouvez spécifier un autre projet comme projet de démarrage à l’aide de la <nobr> `--startup-project` </nobr> option.

Le projet de démarrage et le projet cible sont souvent le même projet. Un scénario classique dans lequel ils sont des projets distincts est cas suivants :

* Les classes d’entité et de contexte EF Core sont dans une bibliothèque de classes .NET Core.
* Une application console .NET Core ou une application web fait référence à la bibliothèque de classes.

Il est également possible de [mettre le code de migrations dans une bibliothèque de classes distincte à partir du contexte EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Autres frameworks cibles

Les outils CLI fonctionnent avec les projets .NET Core et .NET Framework. Les applications qui ont le modèle EF Core dans une bibliothèque de classes .NET Standard peut-être pas un projet de .NET Framework ou le .NET Core. Par exemple, cela est vrai pour les applications Xamarin et de la plateforme Windows universelle. Dans ce cas, vous pouvez créer un projet d’application console .NET Core dont seul but est d’agir en tant que projet de démarrage pour les outils. Le projet peut être un projet factice sans code réel &mdash; il est uniquement nécessaire pour fournir une cible pour les outils.

Pourquoi est-un projet factice requis ? Comme mentionné précédemment, les outils ont exécuter du code de l’application au moment du design. Pour ce faire, ils doivent utiliser le runtime .NET Core. Lorsque le modèle EF Core est dans un projet qui cible .NET Core ou .NET Framework, les outils EF Core emprunt le runtime à partir du projet. Ils ne le sauront pas si le modèle EF Core se trouve dans une bibliothèque de classes .NET Standard. .NET Standard n’est pas une implémentation réelle de .NET ; Il est une spécification d’un ensemble d’API implémentations .NET doivent prendre en charge. Par conséquent, .NET Standard n’est pas suffisant pour les outils EF Core exécuter du code d’application. Le projet factice que vous créez à utiliser en tant que projet de démarrage fournit une plateforme cible concrète dans lequel les outils peuvent charger la bibliothèque de classes .NET Standard.

### <a name="aspnet-core-environment"></a>Environnement ASP.NET Core

Pour spécifier l’environnement pour les projets ASP.NET Core, définissez le **ASPNETCORE_ENVIRONMENT** variable d’environnement avant d’exécuter des commandes.

## <a name="common-options"></a>Options courantes

|                   | Option                            | Description                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Afficher la sortie JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | Le `DbContext` classe à utiliser. Nom de classe complet avec des espaces de noms ou uniquement.  Si cette option est omise, EF Core trouve la classe de contexte. S’il existe plusieurs classes de contexte, cette option est requise.                                            |
| `-p`              | `--project <PROJECT>`             | Chemin d’accès relatif au dossier du projet du projet cible.  Valeur par défaut est le dossier actif.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Chemin d’accès relatif au dossier du projet du projet de démarrage. Valeur par défaut est le dossier actif.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | Le [Moniker du Framework cible](/dotnet/standard/frameworks#supported-target-framework-versions) pour le [framework cible](/dotnet/standard/frameworks).  À utiliser lorsque le fichier projet spécifie plusieurs frameworks cibles, et que vous souhaitez sélectionner un d’eux. |
|                   | `--configuration <CONFIGURATION>` | La configuration de build, par exemple : `Debug` ou `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | L’identificateur du runtime cible à restaurer les packages. Pour connaître les identificateurs de runtime, consultez le [catalogue des identificateurs de runtime](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Afficher les informations d’aide.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Afficher la sortie détaillée.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Ne pas mettre en couleur sortie.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Préfixe avec le niveau de sortie.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>liste de base de données ef dotnet

Supprime la base de données.

Options :

|                   | Option                   | Description                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Ne pas confirmer.                                           |
|                   | <nobr>`--dry-run`</nobr> | Afficher la base de données serait supprimée, mais ne la supprimez. |

## <a name="dotnet-ef-database-update"></a>mise à jour de la base de données DotNet ef

Met à jour la base de données pour la dernière migration ou pour une migration spécifiée.

Arguments :

| Argument      | Description                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | La migration de la cible. Migrations peuvent être identifiées par nom ou par ID. La valeur 0 est un cas spécial signifie *avant la première migration* et oblige toutes les migrations à rétablir. Si aucune migration n’est spécifiée, la commande par défaut est la dernière migration. |

Les exemples suivants mettre à jour la base de données pour une migration spécifiée. La première utilise le nom de la migration et la seconde utilise l’ID de la migration :

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informations de dbcontext ef dotnet

Obtient des informations sur un `DbContext` type.

## <a name="dotnet-ef-dbcontext-list"></a>liste de dbcontext ef dotnet

Listes disponibles `DbContext` types.

## <a name="dotnet-ef-dbcontext-scaffold"></a>structure de dbcontext ef dotnet

Génère du code pour un `DbContext` et types d’entité pour une base de données. Dans l’ordre de cette commande Générer un type d’entité, la table de base de données doit avoir une clé primaire.

Arguments :

| Argument       | Description                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | La chaîne de connexion à la base de données. Pour les projets ASP.NET Core 2.x, la valeur peut être *nom =\<nom de chaîne de connexion >*. Dans ce cas, le nom est fourni à partir des sources de configuration qui sont configurées pour le projet. |
| `<PROVIDER>`   | Le fournisseur à utiliser. En général, c’est le nom du package NuGet, par exemple : `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Options :

|                 | Option                                   | Description                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Utilisez des attributs pour configurer le modèle (le cas échéant). Si cette option est omise, uniquement l’API fluent est utilisé.                                                                |
| `-c`            | `--context <NAME>`                       | Le nom de la `DbContext` classe à générer.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | Le répertoire de placer le `DbContext` fichier de classe dans. Chemins d’accès sont relatif au répertoire de projet. Espaces de noms sont dérivés les noms de dossiers.                                 |
| `-f`            | `--force`                                | Remplacer les fichiers existants.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | Répertoire à placer les fichiers de classe d’entité dans. Chemins d’accès sont relatif au répertoire de projet.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Les schémas des tables pour générer des types d’entité. Pour spécifier plusieurs schémas, répétez `--schema` pour chacun d'entre eux. Si cette option est omise, tous les schémas sont inclus.          |
| `-t`            | `--table <TABLE_NAME>`...                | Les tables pour générer des types d’entité. Pour spécifier plusieurs tables, répétez `-t` ou `--table` pour chacun d'entre eux. Si cette option est omise, toutes les tables sont inclus.                |
|                 | `--use-database-names`                   | Utiliser des noms de table et colonne exactement telles qu’elles apparaissent dans la base de données. Si cette option est omise, les noms de base de données sont modifiés pour mieux se conformer aux conventions de style de nom C#. |

L’exemple suivant structure tous les schémas et les tables et place les nouveaux fichiers dans le *modèles* dossier.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

L’exemple suivant structure que les tables sélectionnées et crée le contexte dans un dossier distinct avec un nom spécifié :

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>ajouter des migrations d’ef dotnet

Ajoute une nouvelle migration.

Arguments :

| Argument | Description                |
|:---------|:---------------------------|
| `<NAME>` | Le nom de la migration. |

Options :

|                   | Option                             | Description                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire de projet. La valeur par défaut est « Migrations ». |

## <a name="dotnet-ef-migrations-list"></a>liste de dotnet ef migrations

Répertorie les migrations disponibles.

## <a name="dotnet-ef-migrations-remove"></a>supprimer des migrations d’ef dotnet

Supprime la dernière migration (annule les modifications de code qui ont été effectuées pour la migration).

Options :

|                   | Option    | Description                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Rétablir la migration (annuler les modifications qui ont été appliquées à la base de données). |

## <a name="dotnet-ef-migrations-script"></a>script de dotnet ef migrations

Génère un script SQL à partir de migrations.

Arguments :

| Argument | Description                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | La migration de départ. Migrations peuvent être identifiées par nom ou par ID. La valeur 0 est un cas spécial signifie *avant la première migration*. La valeur par défaut est 0. |
| `<TO>`   | La migration de fin. Valeur par défaut est la dernière migration.                                                                                                         |

Options :

|                   | Option            | Description                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Fichier dans lequel écrire le script.                                   |
| `-i`              | `--idempotent`    | Générer un script qui peut être utilisé sur toute migration d’une base de données. |

L’exemple suivant crée un script pour la migration de InitialCreate :

```console
dotnet ef migrations script 0 InitialCreate
```

L’exemple suivant crée un script pour toutes les migrations après la migration InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Migrations](xref:core/managing-schemas/migrations/index)
* [Ingénierie à rebours](xref:core/managing-schemas/scaffolding)
