---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489607"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="e0fb1-102">Outils de ligne de commande du .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="e0fb1-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="e0fb1-103">Les outils de ligne de commande de .NET Entity Framework Core sont une extension multiplateforme **dotnet** commande, qui fait partie de la [du SDK .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="e0fb1-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="e0fb1-104">Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] au lieu de cela car ils fournissent une expérience plus intégrée.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="e0fb1-105">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="e0fb1-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="e0fb1-106">Le SDK .NET Core 2.1.300 de version et la plus récente inclut **dotnet ef** commandes qui sont compatibles avec EF Core 2.0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="e0fb1-107">Par conséquent, si vous utilisez des versions récentes du SDK .NET Core et le runtime EF Core, aucune installation n’est requise et vous pouvez ignorer le reste de cette section.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="e0fb1-108">En revanche, le **dotnet ef** outil de relation contenant-contenu dans la version du SDK .NET Core 2.1.300 et les versions ultérieures n’est pas compatible avec la version d’EF Core 1.0 et 1.1.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="e0fb1-109">Avant que vous pouvez travailler avec un projet qui utilise les versions antérieures d’EF Core sur un ordinateur disposant du SDK .NET Core 2.1.300 ou version ultérieure, vous devez également installer la version 2.1.200 ou une version antérieure du Kit de développement et de configurer l’application pour utiliser cette ancienne version en modifiant son  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) fichier.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="e0fb1-110">Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="e0fb1-111">Vous pouvez ensuite poursuivre l’instruction 2161DS ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="e0fb1-112">Pour les versions précédentes du SDK .NET Core, vous pouvez installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="e0fb1-113">Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="e0fb1-114">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="e0fb1-115">Le projet résultant doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="e0fb1-116">Une référence de package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="e0fb1-117">Si vous l’avez fait tout bien, il se peut que vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="e0fb1-118">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="e0fb1-118">Using the tools</span></span>
---------------
<span data-ttu-id="e0fb1-119">Chaque fois que vous appelez une commande, deux projets sont impliqués :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="e0fb1-120">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="e0fb1-121">Le projet cible par défaut pour le projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **`--project`** </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="e0fb1-122">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="e0fb1-123">Également par défaut est le projet dans le répertoire actif, elle peut être modifié à l’aide de la **`--startup-project`** option.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="e0fb1-124">Par exemple, la mise à jour de la base de données de votre application web avec EF Core est installé dans un autre projet se présenterait comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’applications web)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="e0fb1-125">Options courantes :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="e0fb1-126">Afficher la sortie JSON.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-126">Show JSON output.</span></span>           |
| <span data-ttu-id="e0fb1-127">-c</span><span class="sxs-lookup"><span data-stu-id="e0fb1-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="e0fb1-128">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="e0fb1-129">-p</span><span class="sxs-lookup"><span data-stu-id="e0fb1-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="e0fb1-130">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-130">The project to use.</span></span>         |
| <span data-ttu-id="e0fb1-131">-s</span><span class="sxs-lookup"><span data-stu-id="e0fb1-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="e0fb1-132">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="e0fb1-133">Le framework cible.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="e0fb1-134">Configuration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="e0fb1-135">Le runtime à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-135">The runtime to use.</span></span>         |
| <span data-ttu-id="e0fb1-136">-h</span><span class="sxs-lookup"><span data-stu-id="e0fb1-136">-h</span></span> | `--help`                           | <span data-ttu-id="e0fb1-137">Afficher les informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-137">Show help information.</span></span>      |
| <span data-ttu-id="e0fb1-138">-v</span><span class="sxs-lookup"><span data-stu-id="e0fb1-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="e0fb1-139">Afficher la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="e0fb1-140">Ne pas mettre en couleur sortie.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="e0fb1-141">Préfixe avec le niveau de sortie.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="e0fb1-142">Pour spécifier l’environnement ASP.NET Core, définissez le **ASPNETCORE_ENVIRONMENT** variable d’environnement avant l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="e0fb1-143">Commandes</span><span class="sxs-lookup"><span data-stu-id="e0fb1-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="e0fb1-144">liste de base de données ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-144">dotnet ef database drop</span></span>

<span data-ttu-id="e0fb1-145">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-145">Drops the database.</span></span>

<span data-ttu-id="e0fb1-146">Options :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="e0fb1-147">-f</span><span class="sxs-lookup"><span data-stu-id="e0fb1-147">-f</span></span> | `--force`   | <span data-ttu-id="e0fb1-148">Ne pas confirmer.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="e0fb1-149">Afficher la base de données serait supprimée, mais ne la supprimez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="e0fb1-150">mise à jour de la base de données DotNet ef</span><span class="sxs-lookup"><span data-stu-id="e0fb1-150">dotnet ef database update</span></span>

<span data-ttu-id="e0fb1-151">Met à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="e0fb1-152">Arguments :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="e0fb1-153">La migration de la cible.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-153">The target migration.</span></span> <span data-ttu-id="e0fb1-154">Si 0, toutes les migrations seront annulées.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="e0fb1-155">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="e0fb1-156">informations de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="e0fb1-157">Obtient des informations sur un type DbContext.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="e0fb1-158">liste de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="e0fb1-159">Répertorie les types de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="e0fb1-160">structure de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="e0fb1-161">Permet de générer automatiquement un types DbContext et d’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="e0fb1-162">Arguments :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="e0fb1-163">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="e0fb1-164">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-164">The provider to use.</span></span> <span data-ttu-id="e0fb1-165">(par exemple, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="e0fb1-166">Options :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0fb1-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="e0fb1-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="e0fb1-168">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="e0fb1-169">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="e0fb1-170">-c</span><span class="sxs-lookup"><span data-stu-id="e0fb1-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="e0fb1-171">Le nom de la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="e0fb1-172">Le répertoire de placer le fichier de DbContext dans.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="e0fb1-173">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="e0fb1-174">-f</span><span class="sxs-lookup"><span data-stu-id="e0fb1-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="e0fb1-175">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="e0fb1-176">-o</span><span class="sxs-lookup"><span data-stu-id="e0fb1-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="e0fb1-177">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-177">The directory to put files in.</span></span> <span data-ttu-id="e0fb1-178">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="e0fb1-179">Les schémas des tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="e0fb1-180">-t</span><span class="sxs-lookup"><span data-stu-id="e0fb1-180">-t</span></span>              | <span data-ttu-id="e0fb1-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="e0fb1-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="e0fb1-182">Les tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="e0fb1-183">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="e0fb1-184">ajouter des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-184">dotnet ef migrations add</span></span>

<span data-ttu-id="e0fb1-185">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-185">Adds a new migration.</span></span>

<span data-ttu-id="e0fb1-186">Arguments :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="e0fb1-187">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-187">The name of the migration.</span></span> |

<span data-ttu-id="e0fb1-188">Options :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e0fb1-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="e0fb1-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="e0fb1-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="e0fb1-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="e0fb1-191">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="e0fb1-192">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="e0fb1-193">La valeur par défaut est « Migrations ».</span><span class="sxs-lookup"><span data-stu-id="e0fb1-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="e0fb1-194">liste de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="e0fb1-194">dotnet ef migrations list</span></span>

<span data-ttu-id="e0fb1-195">Répertorie les migrations disponibles.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="e0fb1-196">supprimer des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="e0fb1-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="e0fb1-197">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-197">Removes the last migration.</span></span>

<span data-ttu-id="e0fb1-198">Options :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="e0fb1-199">-f</span><span class="sxs-lookup"><span data-stu-id="e0fb1-199">-f</span></span> | `--force` | <span data-ttu-id="e0fb1-200">Rétablir la migration s’il a été appliqué à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="e0fb1-201">script de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="e0fb1-201">dotnet ef migrations script</span></span>

<span data-ttu-id="e0fb1-202">Génère un script SQL à partir de migrations.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="e0fb1-203">Arguments :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="e0fb1-204">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-204">The starting migration.</span></span> <span data-ttu-id="e0fb1-205">La valeur par défaut est 0 (la base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="e0fb1-206">La migration de fin.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-206">The ending migration.</span></span> <span data-ttu-id="e0fb1-207">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="e0fb1-208">Options :</span><span class="sxs-lookup"><span data-stu-id="e0fb1-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="e0fb1-209">-o</span><span class="sxs-lookup"><span data-stu-id="e0fb1-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="e0fb1-210">Fichier dans lequel écrire le résultat.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="e0fb1-211">-i</span><span class="sxs-lookup"><span data-stu-id="e0fb1-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="e0fb1-212">Générer un script qui peut être utilisé sur toute migration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
