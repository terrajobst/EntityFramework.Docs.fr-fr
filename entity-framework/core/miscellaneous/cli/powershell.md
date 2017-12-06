---
title: Package Manager Console (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a>Outils de la Console Gestionnaire de Package EF Core
=====================================
Les outils EF Core Package Manager Console (PMC) est exécuté à l’intérieur de Visual Studio à l’aide de NuGet [Package Manager Console][2].
Ces outils fonctionnent avec les projets .NET Framework et .NET Core.

> [!TIP]
> N’utilisez ne pas Visual Studio ? Le [EF principaux outils de ligne de commande] [ 1] sont inter-plateformes et s’exécutent à l’intérieur d’une invite de commandes.

<a name="installing-the-tools"></a>Installation des outils
--------------------
Installez les outils de la Console Gestionnaire de Package de base EF en installant le package NuGet de Microsoft.EntityFrameworkCore.Tools.
Vous pouvez l’installer en exécutant la commande suivante à l’intérieur de [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Si tout fonctionne correctement, il se peut que vous devez être en mesure d’exécuter cette commande :

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Si votre projet de démarrage cible .NET Standard, [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils.

> [!IMPORTANT]
> Si vous utilisez **Windows universelles** ou **Xamarin**, déplacez votre code EF à une bibliothèque de classes .NET Standard et [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils. Spécifiez la bibliothèque de classes en tant que projet de démarrage.

<a name="using-the-tools"></a>L’utilisation des outils
---------------
Chaque fois que vous appelez une commande, il existe deux projets concernés :

Le projet cible est où les fichiers sont ajoutés (ou dans certains cas supprimés). Le projet cible par défaut est la **projet par défaut** sélectionné dans la Console du Gestionnaire de Package, mais peut également être spécifié à l’aide de-paramètre du projet.

Le projet de démarrage est celui émulé par les outils lors de l’exécution de code de votre projet. La valeur par défaut une **définir comme projet de démarrage** dans l’Explorateur de solutions. Il peut également être spécifié à l’aide du paramètre - StartupProject.

Paramètres communs :

|                           |                             |
| ------------------------- | --------------------------- |
| -Contexte \<chaîne >        | DbContext à utiliser.       |
| -Projet \<chaîne >        | Le projet à utiliser.         |
| -StartupProject \<chaîne > | Le projet de démarrage à utiliser. |
| -Verbose                  | Afficher la sortie des commentaires.        |

Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.

> [!TIP]
> Les paramètres de contexte, du projet et StartupProject prend en charge le développement par tabulation.

> [!TIP]
> Définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter pour spécifier l’environnement ASP.NET Core.

<a name="commands"></a>Commandes
--------

### <a name="add-migration"></a>Ajouter la Migration

Ajoute une nouvelle migration.

Paramètres :

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| ***-Name*** \<chaîne >              | Le nom de la migration.                                                      |
| <nobr>-OutputDir \<chaîne ></nobr>  | Le répertoire (et espace de noms secondaire) à utiliser. Chemins d’accès sont relatif au répertoire du projet. La valeur par défaut est « Migration ». |

> [!NOTE]
> Paramètres de **gras** sont requis et celles dans *italique* sont positionnels.

### <a name="drop-database"></a>Déplacer la base de données

Supprime la base de données.

Paramètres :

|          |                                                          |
| -------- | -------------------------------------------------------- |
| -WhatIf  | Afficher la base de données qui seront supprimés, mais ne pas supprimer. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtient des informations sur un type de DbContext.

### <a name="remove-migration"></a>Remove-Migration

Supprime la dernière migration.

Paramètres :

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| -Force | Ne pas vérifier si la migration a été appliquée à la base de données. |

### <a name="scaffold-dbcontext"></a>Une vue de structure-DbContext

Structures un types DbContext et l’entité pour une base de données.

Paramètres :

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <nobr>***-Connexion*** \<chaîne ></nobr> | La chaîne de connexion à la base de données.                                    |
| ***-Fournisseur*** \<chaîne >                | Le fournisseur à utiliser. (Par exemple : Microsoft.EntityFrameworkCore.SqlServer)       |
| -OutputDir \<chaîne >                     | Répertoire à placer les fichiers dans. Chemins d’accès sont relatif au répertoire du projet. |
| -Contexte \<chaîne >                       | Le nom de la DbContext à générer.                                    |
| -Schémas \<String [] >                     | Les schémas des tables pour générer des types d’entités.                       |
| -Tables \<String [] >                      | Les tables pour générer des types d’entités.                                  |
| -DataAnnotations                         | Utilisez des attributs pour configurer le modèle (le cas échéant). Si omis, uniquement l’API fluent est utilisé. |
| -UseDatabaseNames                        | Utilisez des noms de table et de colonne directement à partir de la base de données.                    |
| -Force                                   | Remplacer les fichiers existants.                                                 |

### <a name="script-migration"></a>Migration de script

Génère un script SQL à partir de la migration.

Paramètres :

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| *-From* \<chaîne > | La migration de départ. La valeur par défaut est 0 (base de données initiale).      |
| *-* \<Chaîne >   | La fin de la migration. Valeur par défaut est la dernière migration.              |
| -Idempotent       | Générer un script qui peut être utilisé sur toute migration, une base de données. |
| -Sortie \<chaîne > | Le fichier dans lequel écrire le résultat à.                                   |

> [!TIP]
> To, From, et les paramètres de sortie prend en charge le développement par tabulation.

### <a name="update-database"></a>Base de données de mise à jour

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <nobr>*-Migration* \<chaîne ></nobr> | La migration de cible. Si 0, toutes les migrations vont être annulées. Valeur par défaut est la dernière migration. |

> [!TIP]
> Le paramètre de la Migration prend en charge le développement par tabulation.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
