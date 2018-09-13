---
title: Console du Gestionnaire de package (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490848"
---
<a name="ef-core-package-manager-console-tools"></a>Outils de la Console Gestionnaire de Package EF Core
=====================================
Les outils Entity Framework Core Package Manager Console (PMC) est exécuté à l’intérieur de Visual Studio à l’aide de NuGet [Console du Gestionnaire de Package][2].
Ces outils fonctionnent avec les projets .NET Framework et .NET Core.

> [!TIP]
> N’utilisez ne pas Visual Studio ? Le [outils de ligne de commande de EF Core] [ 1] sont multiplateformes et d’exécution à l’intérieur d’une invite de commandes.

<a name="installing-the-tools"></a>Installation des outils
--------------------
Installer les outils de Console du Gestionnaire de Package EF Core en installant le package NuGet de Microsoft.EntityFrameworkCore.Tools.
Vous pouvez l’installer en exécutant la commande suivante à l’intérieur de [Console du Gestionnaire de Package][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Si tout fonctionne correctement, vous devez être en mesure d’exécuter cette commande :

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Si votre projet de démarrage cible .NET Standard, [un framework pris en charge de ciblage croisé] [ 3] avant d’utiliser les outils.

> [!IMPORTANT]
> Si vous utilisez **Windows universel** ou **Xamarin**, déplacer votre code EF vers une bibliothèque de classes .NET Standard et [un framework pris en charge de ciblage croisé] [ 3] avant d’utiliser les outils. Spécifier la bibliothèque de classes en tant que projet de démarrage.

<a name="using-the-tools"></a>L’utilisation des outils
---------------
Chaque fois que vous appelez une commande, deux projets sont impliqués :

Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés). Le projet cible par défaut est le **projet par défaut** sélectionné dans la Console du Gestionnaire de Package, mais peut également être spécifié à l’aide de-paramètre du projet.

Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet. Les valeurs par défaut une **définir comme projet de démarrage** dans l’Explorateur de solutions. Il peut également être spécifié en utilisant le paramètre - StartupProject.

Paramètres communs :

|                           |                             |
|:--------------------------|:----------------------------|
| -Contexte \<chaîne >        | DbContext à utiliser.       |
| -Projet \<chaîne >        | Le projet à utiliser.         |
| -StartupProject \<chaîne > | Le projet de démarrage à utiliser. |
| -Verbose                  | Afficher la sortie détaillée.        |

Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.

> [!TIP]
> Les paramètres de contexte, le projet et StartupProject prend en charge d’extension de l’onglet.

> [!TIP]
> Définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter pour spécifier l’environnement ASP.NET Core.

<a name="commands"></a>Commandes
--------

### <a name="add-migration"></a>Add-Migration

Ajoute une nouvelle migration.

Paramètres :

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Name*** \<chaîne >             | Le nom de la migration.                                                                                       |
| <nobr>-OutputDir \<chaîne ></nobr> | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire de projet. La valeur par défaut est « Migrations ». |

> [!NOTE]
> Paramètres dans **gras** sont nécessaires et celles dans *italique* sont positionnels.

### <a name="drop-database"></a>DROP Database

Supprime la base de données.

Paramètres :

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Afficher la base de données serait supprimée, mais ne la supprimez. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtient des informations sur un type DbContext.

### <a name="remove-migration"></a>Remove-Migration

Supprime la dernière migration.

Paramètres :

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Rétablir la migration s’il a été appliqué à la base de données. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Permet de générer automatiquement un types DbContext et d’entité pour une base de données.

Paramètres :

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connexion*** \<chaîne ></nobr> | La chaîne de connexion à la base de données.                                                           |
| ***-Fournisseur*** \<chaîne >                | Le fournisseur à utiliser. (par exemple, Microsoft.EntityFrameworkCore.SqlServer)                      |
| -OutputDir \<chaîne >                     | Répertoire à placer les fichiers dans. Chemins d’accès sont relatif au répertoire de projet.                      |
| -ContextDir \<chaîne >                    | Le répertoire de placer le fichier de DbContext dans. Chemins d’accès sont relatif au répertoire de projet.             |
| -Contexte \<chaîne >                       | Le nom de la classe DbContext pour générer.                                                           |
| -Schémas \<String [] >                     | Les schémas des tables pour générer des types d’entité.                                              |
| -Tables \<String [] >                      | Les tables pour générer des types d’entité.                                                         |
| -DataAnnotations                         | Utilisez des attributs pour configurer le modèle (le cas échéant). Si omis, uniquement l’API fluent est utilisé. |
| -UseDatabaseNames                        | Utilisez des noms de table et de colonne directement à partir de la base de données.                                           |
| -Force                                   | Remplacer les fichiers existants.                                                                        |

### <a name="script-migration"></a>Migration de script

Génère un script SQL à partir de migrations.

Paramètres :

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<chaîne > | La migration de départ. La valeur par défaut est 0 (la base de données initiale).      |
| *-* \<Chaîne >   | La migration de fin. Valeur par défaut est la dernière migration.              |
| -Idempotent       | Générer un script qui peut être utilisé sur toute migration d’une base de données. |
| -Sortie \<chaîne > | Fichier dans lequel écrire le résultat.                                   |

> [!TIP]
> To, From, et les paramètres de sortie prend en charge d’extension de l’onglet.

### <a name="update-database"></a>Mise à jour la base de données

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<chaîne ></nobr> | La migration de la cible. Si '0', toutes les migrations seront annulées. Valeur par défaut est la dernière migration. |

> [!TIP]
> Le paramètre de Migration prend en charge d’extension de l’onglet.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
