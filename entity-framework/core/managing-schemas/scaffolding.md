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
# <a name="reverse-engineering"></a><span data-ttu-id="68703-102">Ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="68703-102">Reverse Engineering</span></span>

<span data-ttu-id="68703-103">L’ingénierie à rebours est le processus de génération de modèles automatique classes de type d’entité et une classe DbContext basée sur un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="68703-104">Elle peut être effectuée à l’aide de la `Scaffold-DbContext` commande des outils Entity Framework Core Package Manager Console (PMC) ou le `dotnet ef dbcontext scaffold` commande des outils d’Interface de ligne de commande (CLI) .NET.</span><span class="sxs-lookup"><span data-stu-id="68703-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="68703-105">Installation de</span><span class="sxs-lookup"><span data-stu-id="68703-105">Installing</span></span>

<span data-ttu-id="68703-106">Avant de l’ingénierie à rebours, vous devrez installer soit le [outils PMC](xref:core/miscellaneous/cli/powershell) (Visual Studio uniquement) ou le [outils CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="68703-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="68703-107">Consultez les liens pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="68703-107">See links for details.</span></span>

<span data-ttu-id="68703-108">Vous devez également installer approprié [fournisseur de base de données](xref:core/providers/index) pour le schéma de base de données que vous voulez rétroconcevoir.</span><span class="sxs-lookup"><span data-stu-id="68703-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="68703-109">Chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="68703-109">Connection string</span></span>

<span data-ttu-id="68703-110">Le premier argument de la commande est une chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="68703-111">Les outils utilisera cette chaîne de connexion pour lire le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="68703-112">Comment mettre entre guillemets et séquence d’échappement de la chaîne de connexion dépend le shell vous utilisez pour exécuter la commande.</span><span class="sxs-lookup"><span data-stu-id="68703-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="68703-113">Reportez-vous à la documentation de l’interpréteur de commandes pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="68703-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="68703-114">Par exemple, PowerShell vous oblige à échapper le `$` de caractères, mais pas `\`.</span><span class="sxs-lookup"><span data-stu-id="68703-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="68703-115">Configuration et les Secrets de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="68703-115">Configuration and User Secrets</span></span>

<span data-ttu-id="68703-116">Si vous avez un projet ASP.NET Core, vous pouvez utiliser le `Name=<connection-string>` syntaxe qui lit la chaîne de connexion de configuration.</span><span class="sxs-lookup"><span data-stu-id="68703-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="68703-117">Cela fonctionne bien avec le [outil Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) de séparer votre mot de passe de base de données à partir de votre code base.</span><span class="sxs-lookup"><span data-stu-id="68703-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="68703-118">Nom du fournisseur</span><span class="sxs-lookup"><span data-stu-id="68703-118">Provider name</span></span>

<span data-ttu-id="68703-119">Le deuxième argument est le nom du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="68703-119">The second argument is the provider name.</span></span> <span data-ttu-id="68703-120">Le nom du fournisseur est généralement le même que le nom du fournisseur NuGet package.</span><span class="sxs-lookup"><span data-stu-id="68703-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="68703-121">Spécification de tables</span><span class="sxs-lookup"><span data-stu-id="68703-121">Specifying tables</span></span>

<span data-ttu-id="68703-122">Toutes les tables dans le schéma de base de données sont intégrées dans les types d’entités par défaut.</span><span class="sxs-lookup"><span data-stu-id="68703-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="68703-123">Vous pouvez limiter les tables sont inversées conçu en spécifiant des schémas et tables.</span><span class="sxs-lookup"><span data-stu-id="68703-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="68703-124">Le `-Schemas` paramètre dans PMC et les `--schema` option dans l’interface CLI peut être utilisée pour inclure toutes les tables dans un schéma.</span><span class="sxs-lookup"><span data-stu-id="68703-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="68703-125">`-Tables` (PMC) et `--table` (CLI) peut être utilisé pour inclure des tables spécifiques.</span><span class="sxs-lookup"><span data-stu-id="68703-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="68703-126">Pour inclure plusieurs tables dans PMC, utilisez un tableau.</span><span class="sxs-lookup"><span data-stu-id="68703-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="68703-127">Pour inclure plusieurs tables dans l’interface CLI, spécifiez l’option plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="68703-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="68703-128">Conservation des noms</span><span class="sxs-lookup"><span data-stu-id="68703-128">Preserving names</span></span>

<span data-ttu-id="68703-129">Noms de table et de colonne sont fixes pour mieux refléter les conventions de nommage .NET pour les types et les propriétés par défaut.</span><span class="sxs-lookup"><span data-stu-id="68703-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="68703-130">En spécifiant le `-UseDatabaseNames` basculer dans PMC ou `--use-database-names` option dans l’interface CLI va désactiver ce comportement en conservant les noms de base de données d’origine autant que possible.</span><span class="sxs-lookup"><span data-stu-id="68703-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="68703-131">Identificateurs de .NET non valides doit toujours être corrigés et synthétisées noms comme propriétés de navigation seront toujours conforme aux conventions d’affectation de noms .NET.</span><span class="sxs-lookup"><span data-stu-id="68703-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="68703-132">API Fluent ou des Annotations de données</span><span class="sxs-lookup"><span data-stu-id="68703-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="68703-133">Types d’entité sont configurés à l’aide de l’API Fluent par défaut.</span><span class="sxs-lookup"><span data-stu-id="68703-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="68703-134">Spécifiez `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) à utiliser à la place des annotations de données lorsque cela est possible.</span><span class="sxs-lookup"><span data-stu-id="68703-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="68703-135">Par exemple, à l’aide de l’API Fluent sera structurer this.</span><span class="sxs-lookup"><span data-stu-id="68703-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="68703-136">Lors de l’utilisation d’Annotations de données sera structurer cela.</span><span class="sxs-lookup"><span data-stu-id="68703-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="68703-137">Nom de DbContext</span><span class="sxs-lookup"><span data-stu-id="68703-137">DbContext name</span></span>

<span data-ttu-id="68703-138">Le nom de la classe DbContext structuré sera le nom de la base de données avec le suffixe *contexte* par défaut.</span><span class="sxs-lookup"><span data-stu-id="68703-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="68703-139">Pour spécifier une autre, utilisez `-Context` dans PMC et `--context` dans l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="68703-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="68703-140">Espaces de noms et de répertoires</span><span class="sxs-lookup"><span data-stu-id="68703-140">Directories and namespaces</span></span>

<span data-ttu-id="68703-141">Les classes d’entité et une classe DbContext sont généré automatiquement dans le répertoire du projet racine et utilisent l’espace de noms par défaut du projet.</span><span class="sxs-lookup"><span data-stu-id="68703-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="68703-142">Vous pouvez spécifier le répertoire où les classes sont structurés à l’aide de `-OutputDir` (PMC) ou `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="68703-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="68703-143">L’espace de noms sera l’espace de noms racine ainsi que les noms de tous les sous-répertoires sous le répertoire du projet racine.</span><span class="sxs-lookup"><span data-stu-id="68703-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="68703-144">Vous pouvez également utiliser `-ContextDir` (PMC) et `--context-dir` (CLI) pour générer automatiquement la classe DbContext dans un répertoire séparé à partir des classes de type d’entité.</span><span class="sxs-lookup"><span data-stu-id="68703-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="68703-145">Son fonctionnement</span><span class="sxs-lookup"><span data-stu-id="68703-145">How it works</span></span>

<span data-ttu-id="68703-146">L’ingénierie à rebours démarre en lisant le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="68703-147">Il lit les informations sur les tables, colonnes, contraintes et index.</span><span class="sxs-lookup"><span data-stu-id="68703-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="68703-148">Ensuite, il utilise les informations de schéma pour créer un modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="68703-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="68703-149">Tables sont utilisées pour créer des types d’entité ; colonnes sont utilisées pour créer des propriétés ; et les clés étrangères sont utilisés pour créer des relations.</span><span class="sxs-lookup"><span data-stu-id="68703-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="68703-150">Enfin, le modèle est utilisé pour générer le code.</span><span class="sxs-lookup"><span data-stu-id="68703-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="68703-151">Correspondante entity type données de classes et API Fluent annotations sont structurées afin de recréer le même modèle à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="68703-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="68703-152">Ce qui ne fonctionne pas</span><span class="sxs-lookup"><span data-stu-id="68703-152">What doesn't work</span></span>

<span data-ttu-id="68703-153">Pas toutes les informations sur un modèle peuvent être représentées à l’aide d’un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="68703-154">Par exemple, les informations sur **hiérarchies d’héritage**, **types détenus**, et **fractionnement de table** ne sont pas présents dans le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="68703-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="68703-155">Pour cette raison, ces constructions seront jamais être l’ingénierie inverse.</span><span class="sxs-lookup"><span data-stu-id="68703-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="68703-156">En outre, **certains types de colonne** ne peut pas être pris en charge par le fournisseur EF Core.</span><span class="sxs-lookup"><span data-stu-id="68703-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="68703-157">Ces colonnes ne seront pas incluses dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="68703-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="68703-158">EF Core requiert la clé de chaque type d’entité.</span><span class="sxs-lookup"><span data-stu-id="68703-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="68703-159">Toutefois, les tables, ne sont pas requis pour spécifier une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="68703-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="68703-160">**Tables sans clé primaire** sont actuellement pas l’ingénierie inverse.</span><span class="sxs-lookup"><span data-stu-id="68703-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="68703-161">Vous pouvez définir **jetons d’accès concurrentiel** dans un modèle EF Core pour empêcher les deux utilisateurs de la mise à jour de la même entité en même temps.</span><span class="sxs-lookup"><span data-stu-id="68703-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="68703-162">Certaines bases de données ont un type spécial pour représenter ce type de colonne (par exemple, rowversion dans SQL Server) dans ce cas, nous pouvons inverser concevoir ces informations ; Toutefois, les autres jetons d’accès concurrentiel ne sera pas être l’ingénierie inverse.</span><span class="sxs-lookup"><span data-stu-id="68703-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="68703-163">Personnalisation du modèle</span><span class="sxs-lookup"><span data-stu-id="68703-163">Customizing the model</span></span>

<span data-ttu-id="68703-164">Le code généré par EF Core est votre code.</span><span class="sxs-lookup"><span data-stu-id="68703-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="68703-165">N’hésitez pas à le modifier.</span><span class="sxs-lookup"><span data-stu-id="68703-165">Feel free to change it.</span></span> <span data-ttu-id="68703-166">Il sera regénéré uniquement si vous rétroconcevez le même modèle à nouveau.</span><span class="sxs-lookup"><span data-stu-id="68703-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="68703-167">Représente le code structuré *un* modèle qui peut être utilisé pour accéder à la base de données, mais il n’est certainement pas le *uniquement* modèle qui peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="68703-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="68703-168">Personnaliser les classes de type d’entité et de la classe DbContext pour répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="68703-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="68703-169">Par exemple, vous pouvez choisir Renommer des types et des propriétés, introduire des hiérarchies d’héritage ou fractionner une table dans plusieurs entités.</span><span class="sxs-lookup"><span data-stu-id="68703-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="68703-170">Vous pouvez également supprimer des index non uniques, séquences inutilisés propriétés de navigation, les propriétés scalaires facultatives et des noms de contraintes à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="68703-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="68703-171">Vous pouvez également ajouter d’autres constructeurs, méthodes, propriétés, etc.</span><span class="sxs-lookup"><span data-stu-id="68703-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="68703-172">à l’aide d’une autre classe partielle dans un fichier distinct.</span><span class="sxs-lookup"><span data-stu-id="68703-172">using another partial class in a separate file.</span></span> <span data-ttu-id="68703-173">Cette approche fonctionne même lorsque vous avez l’intention de rétroconcevoir le modèle à nouveau.</span><span class="sxs-lookup"><span data-stu-id="68703-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="68703-174">La mise à jour le modèle</span><span class="sxs-lookup"><span data-stu-id="68703-174">Updating the model</span></span>

<span data-ttu-id="68703-175">Après avoir apporté des modifications à la base de données, vous devrez peut-être mettre à jour votre modèle EF Core pour refléter ces modifications.</span><span class="sxs-lookup"><span data-stu-id="68703-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="68703-176">Si les modifications de base de données sont simples, il peut être plus facile de simplement effectuer manuellement les modifications dans votre modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="68703-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="68703-177">Par exemple, renommer une table ou une colonne, suppression d’une colonne ou la mise à jour d’un type de colonne est des modifications simples à effectuer dans le code.</span><span class="sxs-lookup"><span data-stu-id="68703-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="68703-178">Changements plus importants, toutefois, ne sont pas en tant que marque facile manuellement.</span><span class="sxs-lookup"><span data-stu-id="68703-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="68703-179">Un flux de travail courant consiste à inverser concevoir le modèle à partir de la base de données à l’aide de `-Force` (PMC) ou `--force` (CLI) pour remplacer le modèle existant avec une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="68703-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="68703-180">Une autre fonctionnalité souvent demandée est la possibilité de mettre à jour le modèle à partir de la base de données tout en conservant la personnalisation, telles que les changements de noms, les hiérarchies de types, etc. Utiliser le problème [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) pour suivre la progression de cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="68703-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="68703-181">Si vous effectuez la rétroconception du modèle à partir de la base de données à nouveau, toutes les modifications apportées aux fichiers seront perdues.</span><span class="sxs-lookup"><span data-stu-id="68703-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
