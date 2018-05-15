---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="75980-102">Outils de ligne de commande du .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="75980-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="75980-103">Les outils de ligne de commande Entity Framework Core .NET sont une extension de l’inter-plateformes **dotnet** commande, qui fait partie de la [le Kit de développement .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="75980-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="75980-104">Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] à la place car ils fournissent une expérience intégrée.</span><span class="sxs-lookup"><span data-stu-id="75980-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="75980-105">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="75980-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="75980-106">Le .NET Core SDK version 2.1.300 et inclut plus récente **dotnet ef** commandes qui sont compatibles avec EF Core 2.0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="75980-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="75980-107">Par conséquent, si vous utilisez des versions récentes de .NET Core SDK et le runtime EF Core, aucune installation n’est requise et vous pouvez ignorer le reste de cette section.</span><span class="sxs-lookup"><span data-stu-id="75980-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="75980-108">En revanche, le **dotnet ef** outil contenu dans la version du Kit de développement .NET Core 2.1.300 et les versions ultérieures n’est pas compatible avec la version EF Core 1.0 et 1.1.</span><span class="sxs-lookup"><span data-stu-id="75980-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="75980-109">Avant que vous pouvez travailler avec un projet qui utilise ces versions antérieures d’EF principal sur un ordinateur qui a le Kit de développement logiciel .NET Core 2.1.300 ou version plus récente, vous devez également installer la version 2.1.200 ou antérieure du Kit de développement et de configurer l’application pour utiliser cette version antérieure en modifiant son  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) fichier.</span><span class="sxs-lookup"><span data-stu-id="75980-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="75980-110">Ce fichier est normalement inclus dans le répertoire de solution (une au-dessus du projet).</span><span class="sxs-lookup"><span data-stu-id="75980-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="75980-111">Vous pouvez ensuite poursuivre l’instruction 2161DS ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="75980-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="75980-112">Pour les versions précédentes du Kit de développement .NET Core, vous pouvez installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :</span><span class="sxs-lookup"><span data-stu-id="75980-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="75980-113">Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="75980-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="75980-114">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="75980-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="75980-115">Le projet résultant doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="75980-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="75980-116">Une référence au package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, ce qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="75980-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="75980-117">Si vous avez tout à droite, vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="75980-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="75980-118">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="75980-118">Using the tools</span></span>
---------------
<span data-ttu-id="75980-119">Chaque fois que vous appelez une commande, il existe deux projets concernés :</span><span class="sxs-lookup"><span data-stu-id="75980-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="75980-120">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="75980-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="75980-121">Le projet cible par défaut du projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="75980-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="75980-122">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="75980-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="75980-123">Également par défaut est le projet dans le répertoire actif, il peut être modifié à l’aide de la **--projet de démarrage** option.</span><span class="sxs-lookup"><span data-stu-id="75980-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="75980-124">Par exemple, la mise à jour de la base de données de votre application web qui a un cœur EF installé dans un autre projet se présente comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’application web)</span><span class="sxs-lookup"><span data-stu-id="75980-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="75980-125">Options courantes :</span><span class="sxs-lookup"><span data-stu-id="75980-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="75980-126">--json</span><span class="sxs-lookup"><span data-stu-id="75980-126">--json</span></span>                           | <span data-ttu-id="75980-127">Afficher la sortie JSON.</span><span class="sxs-lookup"><span data-stu-id="75980-127">Show JSON output.</span></span>           |
| <span data-ttu-id="75980-128">-c</span><span class="sxs-lookup"><span data-stu-id="75980-128">-c</span></span> | <span data-ttu-id="75980-129">--contexte \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="75980-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="75980-130">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="75980-131">-p</span><span class="sxs-lookup"><span data-stu-id="75980-131">-p</span></span> | <span data-ttu-id="75980-132">--projet \<projet ></span><span class="sxs-lookup"><span data-stu-id="75980-132">--project \<PROJECT></span></span>             | <span data-ttu-id="75980-133">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-133">The project to use.</span></span>         |
| <span data-ttu-id="75980-134">-s</span><span class="sxs-lookup"><span data-stu-id="75980-134">-s</span></span> | <span data-ttu-id="75980-135">--projet de démarrage \<projet ></span><span class="sxs-lookup"><span data-stu-id="75980-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="75980-136">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="75980-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="75980-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="75980-138">Le framework cible.</span><span class="sxs-lookup"><span data-stu-id="75980-138">The target framework.</span></span>       |
|    | <span data-ttu-id="75980-139">--configuration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="75980-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="75980-140">Configuration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="75980-141">--runtime \<identificateur ></span><span class="sxs-lookup"><span data-stu-id="75980-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="75980-142">Le runtime à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-142">The runtime to use.</span></span>         |
| <span data-ttu-id="75980-143">-h</span><span class="sxs-lookup"><span data-stu-id="75980-143">-h</span></span> | <span data-ttu-id="75980-144">--aide</span><span class="sxs-lookup"><span data-stu-id="75980-144">--help</span></span>                           | <span data-ttu-id="75980-145">Afficher les informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="75980-145">Show help information.</span></span>      |
| <span data-ttu-id="75980-146">-v</span><span class="sxs-lookup"><span data-stu-id="75980-146">-v</span></span> | <span data-ttu-id="75980-147">--détaillé</span><span class="sxs-lookup"><span data-stu-id="75980-147">--verbose</span></span>                        | <span data-ttu-id="75980-148">Afficher la sortie des commentaires.</span><span class="sxs-lookup"><span data-stu-id="75980-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="75980-149">--Aucune-couleur</span><span class="sxs-lookup"><span data-stu-id="75980-149">--no-color</span></span>                       | <span data-ttu-id="75980-150">Ne pas redéfinir la sortie.</span><span class="sxs-lookup"><span data-stu-id="75980-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="75980-151">--sortie de préfixe</span><span class="sxs-lookup"><span data-stu-id="75980-151">--prefix-output</span></span>                  | <span data-ttu-id="75980-152">Préfixe dont le niveau de sortie.</span><span class="sxs-lookup"><span data-stu-id="75980-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="75980-153">Pour spécifier l’environnement ASP.NET Core, définissez la **ASPNETCORE_ENVIRONMENT** variable d’environnement avant d’exécuter.</span><span class="sxs-lookup"><span data-stu-id="75980-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="75980-154">Commandes</span><span class="sxs-lookup"><span data-stu-id="75980-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="75980-155">suppression de base de données ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-155">dotnet ef database drop</span></span>

<span data-ttu-id="75980-156">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-156">Drops the database.</span></span>

<span data-ttu-id="75980-157">Options :</span><span class="sxs-lookup"><span data-stu-id="75980-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="75980-158">-f</span><span class="sxs-lookup"><span data-stu-id="75980-158">-f</span></span> | <span data-ttu-id="75980-159">--forcer</span><span class="sxs-lookup"><span data-stu-id="75980-159">--force</span></span>   | <span data-ttu-id="75980-160">Ne pas confirmer.</span><span class="sxs-lookup"><span data-stu-id="75980-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="75980-161">--exécution</span><span class="sxs-lookup"><span data-stu-id="75980-161">--dry-run</span></span> | <span data-ttu-id="75980-162">Afficher la base de données qui seront supprimés, mais ne pas supprimer.</span><span class="sxs-lookup"><span data-stu-id="75980-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="75980-163">mise à jour de la base de données DotNet ef</span><span class="sxs-lookup"><span data-stu-id="75980-163">dotnet ef database update</span></span>

<span data-ttu-id="75980-164">Met à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="75980-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="75980-165">Arguments :</span><span class="sxs-lookup"><span data-stu-id="75980-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="75980-166">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-166">\<MIGRATION></span></span> | <span data-ttu-id="75980-167">La migration de cible.</span><span class="sxs-lookup"><span data-stu-id="75980-167">The target migration.</span></span> <span data-ttu-id="75980-168">Si 0, toutes les migrations vont être annulées.</span><span class="sxs-lookup"><span data-stu-id="75980-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="75980-169">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="75980-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="75980-170">informations de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="75980-171">Obtient des informations sur un type de DbContext.</span><span class="sxs-lookup"><span data-stu-id="75980-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="75980-172">liste de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="75980-173">Répertorie les types DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="75980-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="75980-174">structure de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="75980-175">Structures un types DbContext et l’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="75980-176">Arguments :</span><span class="sxs-lookup"><span data-stu-id="75980-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="75980-177">\<CONNEXION &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-177">\<CONNECTION></span></span> | <span data-ttu-id="75980-178">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="75980-179">\<FOURNISSEUR &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-179">\<PROVIDER></span></span>   | <span data-ttu-id="75980-180">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-180">The provider to use.</span></span> <span data-ttu-id="75980-181">(Par ex.</span><span class="sxs-lookup"><span data-stu-id="75980-181">(E.g.</span></span> <span data-ttu-id="75980-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="75980-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="75980-183">Options :</span><span class="sxs-lookup"><span data-stu-id="75980-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="75980-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="75980-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="75980-185">--annotations de données</span><span class="sxs-lookup"><span data-stu-id="75980-185">--data-annotations</span></span>                      | <span data-ttu-id="75980-186">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="75980-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="75980-187">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="75980-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="75980-188">-c</span><span class="sxs-lookup"><span data-stu-id="75980-188">-c</span></span>              | <span data-ttu-id="75980-189">--contexte \<nom ></span><span class="sxs-lookup"><span data-stu-id="75980-189">--context \<NAME></span></span>                       | <span data-ttu-id="75980-190">Le nom de la DbContext.</span><span class="sxs-lookup"><span data-stu-id="75980-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="75980-191">--contexte-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="75980-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="75980-192">Répertoire à placer dans DbContext.</span><span class="sxs-lookup"><span data-stu-id="75980-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="75980-193">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="75980-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="75980-194">-f</span><span class="sxs-lookup"><span data-stu-id="75980-194">-f</span></span>              | <span data-ttu-id="75980-195">--forcer</span><span class="sxs-lookup"><span data-stu-id="75980-195">--force</span></span>                                 | <span data-ttu-id="75980-196">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="75980-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="75980-197">-o</span><span class="sxs-lookup"><span data-stu-id="75980-197">-o</span></span>              | <span data-ttu-id="75980-198">--sortie-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="75980-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="75980-199">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="75980-199">The directory to put files in.</span></span> <span data-ttu-id="75980-200">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="75980-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="75980-201"><nobr>--schéma \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="75980-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="75980-202">Les schémas des tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="75980-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="75980-203">-t</span><span class="sxs-lookup"><span data-stu-id="75980-203">-t</span></span>              | <span data-ttu-id="75980-204">--table \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="75980-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="75980-205">Les tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="75980-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="75980-206">--noms de base de données d’utilisation</span><span class="sxs-lookup"><span data-stu-id="75980-206">--use-database-names</span></span>                    | <span data-ttu-id="75980-207">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="75980-208">ajouter des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-208">dotnet ef migrations add</span></span>

<span data-ttu-id="75980-209">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="75980-209">Adds a new migration.</span></span>

<span data-ttu-id="75980-210">Arguments :</span><span class="sxs-lookup"><span data-stu-id="75980-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="75980-211">\<NOM &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-211">\<NAME></span></span> | <span data-ttu-id="75980-212">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="75980-212">The name of the migration.</span></span> |

<span data-ttu-id="75980-213">Options :</span><span class="sxs-lookup"><span data-stu-id="75980-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="75980-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="75980-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="75980-215"><nobr>--sortie-dir \<chemin d’accès ></nobr></span><span class="sxs-lookup"><span data-stu-id="75980-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="75980-216">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="75980-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="75980-217">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="75980-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="75980-218">La valeur par défaut est « Migration ».</span><span class="sxs-lookup"><span data-stu-id="75980-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="75980-219">liste de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-219">dotnet ef migrations list</span></span>

<span data-ttu-id="75980-220">Répertorie les migrations disponibles.</span><span class="sxs-lookup"><span data-stu-id="75980-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="75980-221">supprimer des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="75980-222">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="75980-222">Removes the last migration.</span></span>

<span data-ttu-id="75980-223">Options :</span><span class="sxs-lookup"><span data-stu-id="75980-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="75980-224">-f</span><span class="sxs-lookup"><span data-stu-id="75980-224">-f</span></span> | <span data-ttu-id="75980-225">--forcer</span><span class="sxs-lookup"><span data-stu-id="75980-225">--force</span></span> | <span data-ttu-id="75980-226">Rétablir la migration si elle a été appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="75980-227">script de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="75980-227">dotnet ef migrations script</span></span>

<span data-ttu-id="75980-228">Génère un script SQL à partir de la migration.</span><span class="sxs-lookup"><span data-stu-id="75980-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="75980-229">Arguments :</span><span class="sxs-lookup"><span data-stu-id="75980-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="75980-230">\<À PARTIR DE &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-230">\<FROM></span></span> | <span data-ttu-id="75980-231">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="75980-231">The starting migration.</span></span> <span data-ttu-id="75980-232">La valeur par défaut est 0 (base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="75980-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="75980-233">\<POUR &GT;</span><span class="sxs-lookup"><span data-stu-id="75980-233">\<TO></span></span>   | <span data-ttu-id="75980-234">La fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="75980-234">The ending migration.</span></span> <span data-ttu-id="75980-235">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="75980-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="75980-236">Options :</span><span class="sxs-lookup"><span data-stu-id="75980-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="75980-237">-o</span><span class="sxs-lookup"><span data-stu-id="75980-237">-o</span></span> | <span data-ttu-id="75980-238">--sortie \<fichier ></span><span class="sxs-lookup"><span data-stu-id="75980-238">--output \<FILE></span></span> | <span data-ttu-id="75980-239">Le fichier dans lequel écrire le résultat à.</span><span class="sxs-lookup"><span data-stu-id="75980-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="75980-240">-i</span><span class="sxs-lookup"><span data-stu-id="75980-240">-i</span></span> | <span data-ttu-id="75980-241">--idempotent</span><span class="sxs-lookup"><span data-stu-id="75980-241">--idempotent</span></span>     | <span data-ttu-id="75980-242">Générer un script qui peut être utilisé sur toute migration, une base de données.</span><span class="sxs-lookup"><span data-stu-id="75980-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
