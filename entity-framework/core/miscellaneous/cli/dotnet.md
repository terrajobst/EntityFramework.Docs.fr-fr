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
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="30bba-102">Outils de ligne de commande du .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="30bba-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="30bba-103">Les outils de ligne de commande de .NET Entity Framework Core sont une extension multiplateforme **dotnet** commande, qui fait partie de la [du SDK .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="30bba-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="30bba-104">Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] au lieu de cela car ils fournissent une expérience plus intégrée.</span><span class="sxs-lookup"><span data-stu-id="30bba-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="30bba-105">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="30bba-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="30bba-106">Le SDK .NET Core 2.1.300 de version et la plus récente inclut **dotnet ef** commandes qui sont compatibles avec EF Core 2.0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="30bba-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="30bba-107">Par conséquent, si vous utilisez des versions récentes du SDK .NET Core et le runtime EF Core, aucune installation n’est requise et vous pouvez ignorer le reste de cette section.</span><span class="sxs-lookup"><span data-stu-id="30bba-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="30bba-108">En revanche, le **dotnet ef** outil de relation contenant-contenu dans la version du SDK .NET Core 2.1.300 et les versions ultérieures n’est pas compatible avec la version d’EF Core 1.0 et 1.1.</span><span class="sxs-lookup"><span data-stu-id="30bba-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="30bba-109">Avant que vous pouvez travailler avec un projet qui utilise les versions antérieures d’EF Core sur un ordinateur disposant du SDK .NET Core 2.1.300 ou version ultérieure, vous devez également installer la version 2.1.200 ou une version antérieure du Kit de développement et de configurer l’application pour utiliser cette ancienne version en modifiant son  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) fichier.</span><span class="sxs-lookup"><span data-stu-id="30bba-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="30bba-110">Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet).</span><span class="sxs-lookup"><span data-stu-id="30bba-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="30bba-111">Vous pouvez ensuite poursuivre l’instruction 2161DS ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="30bba-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="30bba-112">Pour les versions précédentes du SDK .NET Core, vous pouvez installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :</span><span class="sxs-lookup"><span data-stu-id="30bba-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="30bba-113">Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="30bba-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="30bba-114">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="30bba-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="30bba-115">Le projet résultant doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="30bba-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="30bba-116">Une référence de package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="30bba-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="30bba-117">Si vous l’avez fait tout bien, il se peut que vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="30bba-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="30bba-118">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="30bba-118">Using the tools</span></span>
---------------
<span data-ttu-id="30bba-119">Chaque fois que vous appelez une commande, deux projets sont impliqués :</span><span class="sxs-lookup"><span data-stu-id="30bba-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="30bba-120">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="30bba-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="30bba-121">Le projet cible par défaut pour le projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="30bba-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="30bba-122">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="30bba-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="30bba-123">Également par défaut est le projet dans le répertoire actif, elle peut être modifié à l’aide de la **--projet de démarrage** option.</span><span class="sxs-lookup"><span data-stu-id="30bba-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="30bba-124">Par exemple, la mise à jour de la base de données de votre application web avec EF Core est installé dans un autre projet se présenterait comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’applications web)</span><span class="sxs-lookup"><span data-stu-id="30bba-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="30bba-125">Options courantes :</span><span class="sxs-lookup"><span data-stu-id="30bba-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="30bba-126">--json</span><span class="sxs-lookup"><span data-stu-id="30bba-126">--json</span></span>                           | <span data-ttu-id="30bba-127">Afficher la sortie JSON.</span><span class="sxs-lookup"><span data-stu-id="30bba-127">Show JSON output.</span></span>           |
| <span data-ttu-id="30bba-128">-c</span><span class="sxs-lookup"><span data-stu-id="30bba-128">-c</span></span> | <span data-ttu-id="30bba-129">--contexte \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="30bba-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="30bba-130">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="30bba-131">-p</span><span class="sxs-lookup"><span data-stu-id="30bba-131">-p</span></span> | <span data-ttu-id="30bba-132">--projet \<projet ></span><span class="sxs-lookup"><span data-stu-id="30bba-132">--project \<PROJECT></span></span>             | <span data-ttu-id="30bba-133">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-133">The project to use.</span></span>         |
| <span data-ttu-id="30bba-134">-s</span><span class="sxs-lookup"><span data-stu-id="30bba-134">-s</span></span> | <span data-ttu-id="30bba-135">--projet de démarrage \<projet ></span><span class="sxs-lookup"><span data-stu-id="30bba-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="30bba-136">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="30bba-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="30bba-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="30bba-138">Le framework cible.</span><span class="sxs-lookup"><span data-stu-id="30bba-138">The target framework.</span></span>       |
|    | <span data-ttu-id="30bba-139">--configuration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="30bba-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="30bba-140">Configuration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="30bba-141">--exécution \<identificateur ></span><span class="sxs-lookup"><span data-stu-id="30bba-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="30bba-142">Le runtime à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-142">The runtime to use.</span></span>         |
| <span data-ttu-id="30bba-143">-h</span><span class="sxs-lookup"><span data-stu-id="30bba-143">-h</span></span> | <span data-ttu-id="30bba-144">--aide</span><span class="sxs-lookup"><span data-stu-id="30bba-144">--help</span></span>                           | <span data-ttu-id="30bba-145">Afficher les informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="30bba-145">Show help information.</span></span>      |
| <span data-ttu-id="30bba-146">-v</span><span class="sxs-lookup"><span data-stu-id="30bba-146">-v</span></span> | <span data-ttu-id="30bba-147">--verbose</span><span class="sxs-lookup"><span data-stu-id="30bba-147">--verbose</span></span>                        | <span data-ttu-id="30bba-148">Afficher la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="30bba-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="30bba-149">--Aucune couleur</span><span class="sxs-lookup"><span data-stu-id="30bba-149">--no-color</span></span>                       | <span data-ttu-id="30bba-150">Ne pas mettre en couleur sortie.</span><span class="sxs-lookup"><span data-stu-id="30bba-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="30bba-151">--sortie de préfixe</span><span class="sxs-lookup"><span data-stu-id="30bba-151">--prefix-output</span></span>                  | <span data-ttu-id="30bba-152">Préfixe avec le niveau de sortie.</span><span class="sxs-lookup"><span data-stu-id="30bba-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="30bba-153">Pour spécifier l’environnement ASP.NET Core, définissez le **ASPNETCORE_ENVIRONMENT** variable d’environnement avant l’exécution.</span><span class="sxs-lookup"><span data-stu-id="30bba-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="30bba-154">Commandes</span><span class="sxs-lookup"><span data-stu-id="30bba-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="30bba-155">liste de base de données ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-155">dotnet ef database drop</span></span>

<span data-ttu-id="30bba-156">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-156">Drops the database.</span></span>

<span data-ttu-id="30bba-157">Options :</span><span class="sxs-lookup"><span data-stu-id="30bba-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="30bba-158">-f</span><span class="sxs-lookup"><span data-stu-id="30bba-158">-f</span></span> | <span data-ttu-id="30bba-159">--force</span><span class="sxs-lookup"><span data-stu-id="30bba-159">--force</span></span>   | <span data-ttu-id="30bba-160">Ne pas confirmer.</span><span class="sxs-lookup"><span data-stu-id="30bba-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="30bba-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="30bba-161">--dry-run</span></span> | <span data-ttu-id="30bba-162">Afficher la base de données serait supprimée, mais ne la supprimez.</span><span class="sxs-lookup"><span data-stu-id="30bba-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="30bba-163">mise à jour de la base de données DotNet ef</span><span class="sxs-lookup"><span data-stu-id="30bba-163">dotnet ef database update</span></span>

<span data-ttu-id="30bba-164">Met à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="30bba-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="30bba-165">Arguments :</span><span class="sxs-lookup"><span data-stu-id="30bba-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="30bba-166">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-166">\<MIGRATION></span></span> | <span data-ttu-id="30bba-167">La migration de la cible.</span><span class="sxs-lookup"><span data-stu-id="30bba-167">The target migration.</span></span> <span data-ttu-id="30bba-168">Si 0, toutes les migrations seront annulées.</span><span class="sxs-lookup"><span data-stu-id="30bba-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="30bba-169">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="30bba-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="30bba-170">informations de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="30bba-171">Obtient des informations sur un type DbContext.</span><span class="sxs-lookup"><span data-stu-id="30bba-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="30bba-172">liste de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="30bba-173">Répertorie les types de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="30bba-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="30bba-174">structure de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="30bba-175">Permet de générer automatiquement un types DbContext et d’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="30bba-176">Arguments :</span><span class="sxs-lookup"><span data-stu-id="30bba-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="30bba-177">\<CONNEXION &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-177">\<CONNECTION></span></span> | <span data-ttu-id="30bba-178">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="30bba-179">\<FOURNISSEUR &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-179">\<PROVIDER></span></span>   | <span data-ttu-id="30bba-180">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-180">The provider to use.</span></span> <span data-ttu-id="30bba-181">(par exemple, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="30bba-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="30bba-182">Options :</span><span class="sxs-lookup"><span data-stu-id="30bba-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="30bba-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="30bba-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="30bba-184">--annotations de données</span><span class="sxs-lookup"><span data-stu-id="30bba-184">--data-annotations</span></span>                      | <span data-ttu-id="30bba-185">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="30bba-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="30bba-186">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="30bba-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="30bba-187">-c</span><span class="sxs-lookup"><span data-stu-id="30bba-187">-c</span></span>              | <span data-ttu-id="30bba-188">--contexte \<nom ></span><span class="sxs-lookup"><span data-stu-id="30bba-188">--context \<NAME></span></span>                       | <span data-ttu-id="30bba-189">Le nom de la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="30bba-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="30bba-190">--contexte-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="30bba-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="30bba-191">Le répertoire de placer le fichier de DbContext dans.</span><span class="sxs-lookup"><span data-stu-id="30bba-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="30bba-192">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="30bba-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="30bba-193">-f</span><span class="sxs-lookup"><span data-stu-id="30bba-193">-f</span></span>              | <span data-ttu-id="30bba-194">--force</span><span class="sxs-lookup"><span data-stu-id="30bba-194">--force</span></span>                                 | <span data-ttu-id="30bba-195">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="30bba-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="30bba-196">-o</span><span class="sxs-lookup"><span data-stu-id="30bba-196">-o</span></span>              | <span data-ttu-id="30bba-197">--sortie-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="30bba-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="30bba-198">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="30bba-198">The directory to put files in.</span></span> <span data-ttu-id="30bba-199">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="30bba-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="30bba-200"><nobr>--schéma \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="30bba-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="30bba-201">Les schémas des tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="30bba-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="30bba-202">-t</span><span class="sxs-lookup"><span data-stu-id="30bba-202">-t</span></span>              | <span data-ttu-id="30bba-203">--table \<nom_table >...</span><span class="sxs-lookup"><span data-stu-id="30bba-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="30bba-204">Les tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="30bba-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="30bba-205">---base de données-utiliser des noms</span><span class="sxs-lookup"><span data-stu-id="30bba-205">--use-database-names</span></span>                    | <span data-ttu-id="30bba-206">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="30bba-207">ajouter des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-207">dotnet ef migrations add</span></span>

<span data-ttu-id="30bba-208">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="30bba-208">Adds a new migration.</span></span>

<span data-ttu-id="30bba-209">Arguments :</span><span class="sxs-lookup"><span data-stu-id="30bba-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="30bba-210">\<NOM &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-210">\<NAME></span></span> | <span data-ttu-id="30bba-211">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="30bba-211">The name of the migration.</span></span> |

<span data-ttu-id="30bba-212">Options :</span><span class="sxs-lookup"><span data-stu-id="30bba-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="30bba-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="30bba-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="30bba-214"><nobr>--sortie-dir \<chemin d’accès ></nobr></span><span class="sxs-lookup"><span data-stu-id="30bba-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="30bba-215">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="30bba-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="30bba-216">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="30bba-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="30bba-217">La valeur par défaut est « Migrations ».</span><span class="sxs-lookup"><span data-stu-id="30bba-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="30bba-218">liste de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="30bba-218">dotnet ef migrations list</span></span>

<span data-ttu-id="30bba-219">Répertorie les migrations disponibles.</span><span class="sxs-lookup"><span data-stu-id="30bba-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="30bba-220">supprimer des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="30bba-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="30bba-221">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="30bba-221">Removes the last migration.</span></span>

<span data-ttu-id="30bba-222">Options :</span><span class="sxs-lookup"><span data-stu-id="30bba-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="30bba-223">-f</span><span class="sxs-lookup"><span data-stu-id="30bba-223">-f</span></span> | <span data-ttu-id="30bba-224">--force</span><span class="sxs-lookup"><span data-stu-id="30bba-224">--force</span></span> | <span data-ttu-id="30bba-225">Rétablir la migration s’il a été appliqué à la base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="30bba-226">script de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="30bba-226">dotnet ef migrations script</span></span>

<span data-ttu-id="30bba-227">Génère un script SQL à partir de migrations.</span><span class="sxs-lookup"><span data-stu-id="30bba-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="30bba-228">Arguments :</span><span class="sxs-lookup"><span data-stu-id="30bba-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="30bba-229">\<À PARTIR DE &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-229">\<FROM></span></span> | <span data-ttu-id="30bba-230">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="30bba-230">The starting migration.</span></span> <span data-ttu-id="30bba-231">La valeur par défaut est 0 (la base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="30bba-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="30bba-232">\<POUR &GT;</span><span class="sxs-lookup"><span data-stu-id="30bba-232">\<TO></span></span>   | <span data-ttu-id="30bba-233">La migration de fin.</span><span class="sxs-lookup"><span data-stu-id="30bba-233">The ending migration.</span></span> <span data-ttu-id="30bba-234">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="30bba-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="30bba-235">Options :</span><span class="sxs-lookup"><span data-stu-id="30bba-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="30bba-236">-o</span><span class="sxs-lookup"><span data-stu-id="30bba-236">-o</span></span> | <span data-ttu-id="30bba-237">--sortie \<fichier ></span><span class="sxs-lookup"><span data-stu-id="30bba-237">--output \<FILE></span></span> | <span data-ttu-id="30bba-238">Fichier dans lequel écrire le résultat.</span><span class="sxs-lookup"><span data-stu-id="30bba-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="30bba-239">-i</span><span class="sxs-lookup"><span data-stu-id="30bba-239">-i</span></span> | <span data-ttu-id="30bba-240">--idempotent</span><span class="sxs-lookup"><span data-stu-id="30bba-240">--idempotent</span></span>     | <span data-ttu-id="30bba-241">Générer un script qui peut être utilisé sur toute migration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="30bba-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
