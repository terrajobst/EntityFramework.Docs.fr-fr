---
title: Rétroconception - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688678"
---
# <a name="reverse-engineering"></a>Ingénierie à rebours

L’ingénierie à rebours est le processus de génération de modèles automatique classes de type d’entité et une classe DbContext basée sur un schéma de base de données. Elle peut être effectuée à l’aide de la `Scaffold-DbContext` commande des outils Entity Framework Core Package Manager Console (PMC) ou le `dotnet ef dbcontext scaffold` commande des outils d’Interface de ligne de commande (CLI) .NET.

## <a name="installing"></a>Installation de

Avant de l’ingénierie à rebours, vous devrez installer soit le [outils PMC](xref:core/miscellaneous/cli/powershell) (Visual Studio uniquement) ou le [outils CLI](xref:core/miscellaneous/cli/dotnet). Consultez les liens pour plus d’informations.

Vous devez également installer approprié [fournisseur de base de données](xref:core/providers/index) pour le schéma de base de données que vous voulez rétroconcevoir.

## <a name="connection-string"></a>Chaîne de connexion

Le premier argument de la commande est une chaîne de connexion à la base de données. Les outils utilisera cette chaîne de connexion pour lire le schéma de base de données.

Comment mettre entre guillemets et séquence d’échappement de la chaîne de connexion dépend le shell vous utilisez pour exécuter la commande. Reportez-vous à la documentation de l’interpréteur de commandes pour plus de détails. Par exemple, PowerShell vous oblige à échapper le `$` de caractères, mais pas `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuration et les Secrets de l’utilisateur

Si vous avez un projet ASP.NET Core, vous pouvez utiliser le `Name=<connection-string>` syntaxe qui lit la chaîne de connexion de configuration.

Cela fonctionne bien avec le [outil Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) de séparer votre mot de passe de base de données à partir de votre code base.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nom du fournisseur

Le deuxième argument est le nom du fournisseur. Le nom du fournisseur est généralement le même que le nom du fournisseur NuGet package.

## <a name="specifying-tables"></a>Spécification de tables

Toutes les tables dans le schéma de base de données sont intégrées dans les types d’entités par défaut. Vous pouvez limiter les tables sont inversées conçu en spécifiant des schémas et tables.

Le `-Schemas` paramètre dans PMC et les `--schema` option dans l’interface CLI peut être utilisée pour inclure toutes les tables dans un schéma.

`-Tables` (PMC) et `--table` (CLI) peut être utilisé pour inclure des tables spécifiques.

Pour inclure plusieurs tables dans PMC, utilisez un tableau.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Pour inclure plusieurs tables dans l’interface CLI, spécifiez l’option plusieurs fois.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Conservation des noms

Noms de table et de colonne sont fixes pour mieux refléter les conventions de nommage .NET pour les types et les propriétés par défaut. En spécifiant le `-UseDatabaseNames` basculer dans PMC ou `--use-database-names` option dans l’interface CLI va désactiver ce comportement en conservant les noms de base de données d’origine autant que possible. Identificateurs de .NET non valides doit toujours être corrigés et synthétisées noms comme propriétés de navigation seront toujours conforme aux conventions d’affectation de noms .NET.

## <a name="fluent-api-or-data-annotations"></a>API Fluent ou des Annotations de données

Types d’entité sont configurés à l’aide de l’API Fluent par défaut. Spécifiez `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) à utiliser à la place des annotations de données lorsque cela est possible.

Par exemple, à l’aide de l’API Fluent sera structurer this.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Lors de l’utilisation d’Annotations de données sera structurer cela.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nom de DbContext

Le nom de la classe DbContext structuré sera le nom de la base de données avec le suffixe *contexte* par défaut. Pour spécifier une autre, utilisez `-Context` dans PMC et `--context` dans l’interface CLI.

## <a name="directories-and-namespaces"></a>Espaces de noms et de répertoires

Les classes d’entité et une classe DbContext sont généré automatiquement dans le répertoire du projet racine et utilisent l’espace de noms par défaut du projet. Vous pouvez spécifier le répertoire où les classes sont structurés à l’aide de `-OutputDir` (PMC) ou `--output-dir` (CLI). L’espace de noms sera l’espace de noms racine ainsi que les noms de tous les sous-répertoires sous le répertoire du projet racine.

Vous pouvez également utiliser `-ContextDir` (PMC) et `--context-dir` (CLI) pour générer automatiquement la classe DbContext dans un répertoire séparé à partir des classes de type d’entité.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Son fonctionnement

L’ingénierie à rebours démarre en lisant le schéma de base de données. Il lit les informations sur les tables, colonnes, contraintes et index.

Ensuite, il utilise les informations de schéma pour créer un modèle EF Core. Tables sont utilisées pour créer des types d’entité ; colonnes sont utilisées pour créer des propriétés ; et les clés étrangères sont utilisés pour créer des relations.

Enfin, le modèle est utilisé pour générer le code. Correspondante entity type données de classes et API Fluent annotations sont structurées afin de recréer le même modèle à partir de votre application.

## <a name="what-doesnt-work"></a>Ce qui ne fonctionne pas

Pas toutes les informations sur un modèle peuvent être représentées à l’aide d’un schéma de base de données. Par exemple, les informations sur **hiérarchies d’héritage**, **types détenus**, et **fractionnement de table** ne sont pas présents dans le schéma de base de données. Pour cette raison, ces constructions seront jamais être l’ingénierie inverse.

En outre, **certains types de colonne** ne peut pas être pris en charge par le fournisseur EF Core. Ces colonnes ne seront pas incluses dans le modèle.

EF Core requiert la clé de chaque type d’entité. Toutefois, les tables, ne sont pas requis pour spécifier une clé primaire. **Tables sans clé primaire** sont actuellement pas l’ingénierie inverse.

Vous pouvez définir **jetons d’accès concurrentiel** dans un modèle EF Core pour empêcher les deux utilisateurs de la mise à jour de la même entité en même temps. Certaines bases de données ont un type spécial pour représenter ce type de colonne (par exemple, rowversion dans SQL Server) dans ce cas, nous pouvons inverser concevoir ces informations ; Toutefois, les autres jetons d’accès concurrentiel ne sera pas être l’ingénierie inverse.

## <a name="customizing-the-model"></a>Personnalisation du modèle

Le code généré par EF Core est votre code. N’hésitez pas à le modifier. Il sera regénéré uniquement si vous rétroconcevez le même modèle à nouveau. Représente le code structuré *un* modèle qui peut être utilisé pour accéder à la base de données, mais il n’est certainement pas le *uniquement* modèle qui peut être utilisé.

Personnaliser les classes de type d’entité et de la classe DbContext pour répondre à vos besoins. Par exemple, vous pouvez choisir Renommer des types et des propriétés, introduire des hiérarchies d’héritage ou fractionner une table dans plusieurs entités. Vous pouvez également supprimer des index non uniques, séquences inutilisés propriétés de navigation, les propriétés scalaires facultatives et des noms de contraintes à partir du modèle.

Vous pouvez également ajouter d’autres constructeurs, méthodes, propriétés, etc. à l’aide d’une autre classe partielle dans un fichier distinct. Cette approche fonctionne même lorsque vous avez l’intention de rétroconcevoir le modèle à nouveau.

## <a name="updating-the-model"></a>La mise à jour le modèle

Après avoir apporté des modifications à la base de données, vous devrez peut-être mettre à jour votre modèle EF Core pour refléter ces modifications. Si les modifications de base de données sont simples, il peut être plus facile de simplement effectuer manuellement les modifications dans votre modèle EF Core. Par exemple, renommer une table ou une colonne, suppression d’une colonne ou la mise à jour d’un type de colonne est des modifications simples à effectuer dans le code.

Changements plus importants, toutefois, ne sont pas en tant que marque facile manuellement. Un flux de travail courant consiste à inverser concevoir le modèle à partir de la base de données à l’aide de `-Force` (PMC) ou `--force` (CLI) pour remplacer le modèle existant avec une mise à jour.

Une autre fonctionnalité souvent demandée est la possibilité de mettre à jour le modèle à partir de la base de données tout en conservant la personnalisation, telles que les changements de noms, les hiérarchies de types, etc. Utiliser le problème [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) pour suivre la progression de cette fonctionnalité.

> [!WARNING]
> Si vous effectuez la rétroconception du modèle à partir de la base de données à nouveau, toutes les modifications apportées aux fichiers seront perdues.
