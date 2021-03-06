---
title: Ingénierie à rebours-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416762"
---
# <a name="reverse-engineering"></a>Rétroconception

L’ingénierie à rebours est le processus de génération de modèles automatique des classes de type d’entité et une classe DbContext basée sur un schéma de base de données. Il peut être effectué à l’aide de la commande `Scaffold-DbContext` des outils de la console du gestionnaire de package EF Core (PMC) ou de la commande `dotnet ef dbcontext scaffold` des outils de l’interface de ligne de commande (CLI) .NET.

## <a name="installing"></a>Installation

Avant l’ingénierie à rebours, vous devez installer les [Outils PMC](xref:core/miscellaneous/cli/powershell) (Visual Studio uniquement) ou les [Outils CLI](xref:core/miscellaneous/cli/dotnet). Pour plus d’informations, consultez les liens.

Vous devez également installer un [fournisseur de base de données](xref:core/providers/index) approprié pour le schéma de base de données que vous souhaitez rétroconcevoir.

## <a name="connection-string"></a>Chaîne de connexion

Le premier argument de la commande est une chaîne de connexion à la base de données. Les outils utilisent cette chaîne de connexion pour lire le schéma de la base de données.

La façon dont vous utilisez les guillemets et les séquences d’échappement de la chaîne de connexion dépend du shell que vous utilisez pour exécuter la commande. Reportez-vous à la documentation de votre shell pour obtenir des informations spécifiques. Par exemple, PowerShell vous oblige à échapper le caractère `$`, mais pas `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuration et secrets de l’utilisateur

Si vous avez un projet ASP.NET Core, vous pouvez utiliser la syntaxe `Name=<connection-string>` pour lire la chaîne de connexion à partir de la configuration.

Cela fonctionne bien avec l' [outil secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) pour conserver le mot de passe de votre base de données distinct de votre code base.

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nom du fournisseur

Le deuxième argument est le nom du fournisseur. Le nom du fournisseur est généralement identique au nom du package NuGet du fournisseur.

## <a name="specifying-tables"></a>Spécification de tables

Toutes les tables du schéma de base de données sont rétroconçues dans les types d’entités par défaut. Vous pouvez limiter les tables qui sont rétroconçues en spécifiant des schémas et des tables.

Le paramètre `-Schemas` dans PMC et l’option `--schema` de l’interface CLI peuvent être utilisés pour inclure chaque table dans un schéma.

`-Tables` (PMC) et `--table` (CLI) peuvent être utilisés pour inclure des tables spécifiques.

Pour inclure plusieurs tables dans PMC, utilisez un tableau.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Pour inclure plusieurs tables dans l’interface CLI, spécifiez l’option plusieurs fois.

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Préservation des noms

Les noms de table et de colonne sont fixes pour mieux correspondre aux conventions de nommage .NET pour les types et les propriétés par défaut. La spécification du commutateur `-UseDatabaseNames` dans PMC ou de l’option `--use-database-names` dans l’interface CLI désactivera ce comportement en conservant autant que possible les noms de bases de données originaux. Les identificateurs .NET non valides seront toujours fixes et les noms synthétisés, tels que les propriétés de navigation, seront toujours conformes aux conventions d’affectation de noms .NET.

## <a name="fluent-api-or-data-annotations"></a>API Fluent ou annotations de données

Par défaut, les types d’entités sont configurés à l’aide de l’API Fluent. Spécifiez `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) pour utiliser à la place des annotations de données lorsque cela est possible.

Par exemple, l’utilisation de l’API Fluent entraîne l’échafaudage suivant :

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Lorsque vous utilisez des annotations de données, vous générez une structure :

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nom de DbContext

Le nom de classe DbContext généré par génération de modèles automatique sera le nom de la base de données suffixée *par défaut* . Pour spécifier un autre, utilisez `-Context` dans PMC et `--context` dans l’interface CLI.

## <a name="directories-and-namespaces"></a>Répertoires et espaces de noms

Les classes d’entité et une classe DbContext sont intégrées au répertoire racine du projet et utilisent l’espace de noms par défaut du projet. Vous pouvez spécifier le répertoire dans lequel les classes sont échafaudées à l’aide de `-OutputDir` (PMC) ou `--output-dir` (CLI). L’espace de noms sera l’espace de noms racine, ainsi que les noms de tous les sous-répertoires sous le répertoire racine du projet.

Vous pouvez également utiliser `-ContextDir` (PMC) et `--context-dir` (CLI) pour générer une structure de la classe DbContext dans un répertoire distinct des classes de type d’entité.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Fonctionnement

L’ingénierie à rebours commence par lire le schéma de base de données. Il lit les informations sur les tables, les colonnes, les contraintes et les index.

Ensuite, il utilise les informations de schéma pour créer un modèle de EF Core. Les tables sont utilisées pour créer des types d’entité ; les colonnes sont utilisées pour créer des propriétés. et les clés étrangères sont utilisées pour créer des relations.

Enfin, le modèle est utilisé pour générer le code. Les classes de type d’entité, l’API Fluent et les annotations de données correspondantes sont échafaudées afin de recréer le même modèle à partir de votre application.

## <a name="limitations"></a>Limites

* Tout ce qui concerne un modèle peut être représenté à l’aide d’un schéma de base de données. Par exemple, les informations sur les [**hiérarchies d’héritage**](../modeling/inheritance.md), les [**types détenus**](../modeling/owned-entities.md)et le [**fractionnement de table**](../modeling/table-splitting.md) ne sont pas présentes dans le schéma de la base de données. Pour cette raison, ces constructions ne feront jamais l’effet d’une rétroconception.
* En outre, **certains types de colonne** peuvent ne pas être pris en charge par le fournisseur EF Core. Ces colonnes ne sont pas incluses dans le modèle.
* Vous pouvez définir des [**jetons d’accès concurrentiel**](../modeling/concurrency.md), dans un modèle EF Core pour empêcher deux utilisateurs de mettre à jour la même entité en même temps. Certaines bases de données ont un type spécial pour représenter ce type de colonne (par exemple, rowversion dans SQL Server), auquel cas nous pouvons rétroconcevoir ces informations. Toutefois, les autres jetons d’accès concurrentiel ne feront pas l’être par rétroconception.
* [La C# fonctionnalité de type référence Nullable 8](/dotnet/csharp/tutorials/nullable-reference-types) n’est actuellement pas prise en charge dans l’ingénierie à C# rebours : EF Core génère toujours du code qui suppose que la fonctionnalité est désactivée. Par exemple, les colonnes de texte Nullable seront échafaudées en tant que propriété de type `string`, et non pas `string?`, avec l’API Fluent ou les annotations de données utilisées pour configurer si une propriété est requise ou non. Vous pouvez modifier le code de génération de modèles automatique et C# les remplacer par des annotations de possibilité de valeur null. La prise en charge de la génération de modèles automatique pour les types de référence Nullable est suivie par le problème [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).

## <a name="customizing-the-model"></a>Personnalisation du modèle

Le code généré par EF Core est votre code. N’hésitez pas à la modifier. Elle sera régénérée uniquement si vous retrouvez à nouveau le même modèle. Le code de génération de modèles automatique représente *un* modèle qui peut être utilisé pour accéder à la base de données, mais ce n’est certainement pas le *seul* modèle qui peut être utilisé.

Personnalisez les classes de type d’entité et la classe DbContext en fonction de vos besoins. Par exemple, vous pouvez choisir de renommer des types et des propriétés, d’introduire des hiérarchies d’héritage ou de fractionner une table en plusieurs entités. Vous pouvez également supprimer des index non uniques, des séquences inutilisées et des propriétés de navigation, des propriétés scalaires facultatives et des noms de contrainte à partir du modèle.

Vous pouvez également ajouter des constructeurs, des méthodes, des propriétés, etc. supplémentaires. utilisation d’une autre classe partielle dans un fichier séparé. Cette approche fonctionne même lorsque vous envisagez de réactiver le modèle.

## <a name="updating-the-model"></a>Mise à jour du modèle

Après avoir apporté des modifications à la base de données, vous devrez peut-être mettre à jour votre modèle de EF Core pour refléter ces modifications. Si les modifications de la base de données sont simples, il peut être plus facile d’apporter manuellement les modifications à votre modèle de EF Core. Par exemple, le fait de renommer une table ou une colonne, de supprimer une colonne ou de mettre à jour le type d’une colonne est une modification triviale dans le code.

Toutefois, les modifications les plus importantes ne sont pas aussi faciles à effectuer manuellement. Un flux de travail courant consiste à reconstituer à nouveau le modèle de la base de données à l’aide d' `-Force` (PMC) ou d' `--force` (CLI) pour remplacer le modèle existant par un modèle mis à jour.

Une autre fonctionnalité couramment demandée est la possibilité de mettre à jour le modèle à partir de la base de données tout en préservant la personnalisation, comme les renommages, les hiérarchies de types, etc. Utilisez [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) de problème pour suivre la progression de cette fonctionnalité.

> [!WARNING]
> Si vous rérétroconcevez le modèle à partir de la base de données, toutes les modifications que vous avez apportées aux fichiers seront perdues.
