---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="b0923-102">Outils de ligne de commande du .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="b0923-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="b0923-103">Les outils de ligne de commande Entity Framework Core .NET sont une extension de l’inter-plateformes **dotnet** commande, qui fait partie de la [le Kit de développement .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="b0923-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="b0923-104">Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] à la place car ils fournissent une expérience intégrée.</span><span class="sxs-lookup"><span data-stu-id="b0923-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="b0923-105">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="b0923-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="b0923-106">Installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :</span><span class="sxs-lookup"><span data-stu-id="b0923-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="b0923-107">Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="b0923-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="b0923-108">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0923-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="b0923-109">Le projet résultant doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="b0923-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="b0923-110">Une référence au package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, ce qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="b0923-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="b0923-111">Si vous avez tout à droite, vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="b0923-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="b0923-112">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="b0923-112">Using the tools</span></span>
---------------
<span data-ttu-id="b0923-113">Chaque fois que vous appelez une commande, il existe deux projets concernés :</span><span class="sxs-lookup"><span data-stu-id="b0923-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="b0923-114">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="b0923-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="b0923-115">Le projet cible par défaut du projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="b0923-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="b0923-116">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="b0923-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="b0923-117">Également par défaut est le projet dans le répertoire actif, il peut être modifié à l’aide de la **--projet de démarrage** option.</span><span class="sxs-lookup"><span data-stu-id="b0923-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="b0923-118">Par exemple, la mise à jour de la base de données de votre application web qui a un cœur EF installé dans un autre projet se présente comme suit : `dotnet ef database update --project {project-path}` (à partir de votre répertoire d’application web)</span><span class="sxs-lookup"><span data-stu-id="b0923-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="b0923-119">Options courantes :</span><span class="sxs-lookup"><span data-stu-id="b0923-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="b0923-120">--json</span><span class="sxs-lookup"><span data-stu-id="b0923-120">--json</span></span>                           | <span data-ttu-id="b0923-121">Afficher la sortie JSON.</span><span class="sxs-lookup"><span data-stu-id="b0923-121">Show JSON output.</span></span>           |
| <span data-ttu-id="b0923-122">-c</span><span class="sxs-lookup"><span data-stu-id="b0923-122">-c</span></span> | <span data-ttu-id="b0923-123">--contexte \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="b0923-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="b0923-124">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="b0923-125">-p</span><span class="sxs-lookup"><span data-stu-id="b0923-125">-p</span></span> | <span data-ttu-id="b0923-126">--projet \<projet ></span><span class="sxs-lookup"><span data-stu-id="b0923-126">--project \<PROJECT></span></span>             | <span data-ttu-id="b0923-127">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-127">The project to use.</span></span>         |
| <span data-ttu-id="b0923-128">-s</span><span class="sxs-lookup"><span data-stu-id="b0923-128">-s</span></span> | <span data-ttu-id="b0923-129">--projet de démarrage \<projet ></span><span class="sxs-lookup"><span data-stu-id="b0923-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="b0923-130">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="b0923-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="b0923-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="b0923-132">Le framework cible.</span><span class="sxs-lookup"><span data-stu-id="b0923-132">The target framework.</span></span>       |
|    | <span data-ttu-id="b0923-133">--configuration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="b0923-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="b0923-134">Configuration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="b0923-135">--runtime \<identificateur ></span><span class="sxs-lookup"><span data-stu-id="b0923-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="b0923-136">Le runtime à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-136">The runtime to use.</span></span>         |
| <span data-ttu-id="b0923-137">-h</span><span class="sxs-lookup"><span data-stu-id="b0923-137">-h</span></span> | <span data-ttu-id="b0923-138">--aide</span><span class="sxs-lookup"><span data-stu-id="b0923-138">--help</span></span>                           | <span data-ttu-id="b0923-139">Afficher les informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="b0923-139">Show help information.</span></span>      |
| <span data-ttu-id="b0923-140">-v</span><span class="sxs-lookup"><span data-stu-id="b0923-140">-v</span></span> | <span data-ttu-id="b0923-141">--détaillé</span><span class="sxs-lookup"><span data-stu-id="b0923-141">--verbose</span></span>                        | <span data-ttu-id="b0923-142">Afficher la sortie des commentaires.</span><span class="sxs-lookup"><span data-stu-id="b0923-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="b0923-143">--Aucune-couleur</span><span class="sxs-lookup"><span data-stu-id="b0923-143">--no-color</span></span>                       | <span data-ttu-id="b0923-144">Ne pas redéfinir la sortie.</span><span class="sxs-lookup"><span data-stu-id="b0923-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="b0923-145">--sortie de préfixe</span><span class="sxs-lookup"><span data-stu-id="b0923-145">--prefix-output</span></span>                  | <span data-ttu-id="b0923-146">Préfixe dont le niveau de sortie.</span><span class="sxs-lookup"><span data-stu-id="b0923-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="b0923-147">Pour spécifier l’environnement ASP.NET Core, définissez la **ASPNETCORE_ENVIRONMENT** variable d’environnement avant d’exécuter.</span><span class="sxs-lookup"><span data-stu-id="b0923-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="b0923-148">Commandes</span><span class="sxs-lookup"><span data-stu-id="b0923-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="b0923-149">suppression de base de données ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-149">dotnet ef database drop</span></span>

<span data-ttu-id="b0923-150">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-150">Drops the database.</span></span>

<span data-ttu-id="b0923-151">Options :</span><span class="sxs-lookup"><span data-stu-id="b0923-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="b0923-152">-f</span><span class="sxs-lookup"><span data-stu-id="b0923-152">-f</span></span> | <span data-ttu-id="b0923-153">--forcer</span><span class="sxs-lookup"><span data-stu-id="b0923-153">--force</span></span>   | <span data-ttu-id="b0923-154">Ne pas confirmer.</span><span class="sxs-lookup"><span data-stu-id="b0923-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="b0923-155">--exécution</span><span class="sxs-lookup"><span data-stu-id="b0923-155">--dry-run</span></span> | <span data-ttu-id="b0923-156">Afficher la base de données qui seront supprimés, mais ne pas supprimer.</span><span class="sxs-lookup"><span data-stu-id="b0923-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="b0923-157">mise à jour de la base de données DotNet ef</span><span class="sxs-lookup"><span data-stu-id="b0923-157">dotnet ef database update</span></span>

<span data-ttu-id="b0923-158">Met à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b0923-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="b0923-159">Arguments :</span><span class="sxs-lookup"><span data-stu-id="b0923-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0923-160">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-160">\<MIGRATION></span></span> | <span data-ttu-id="b0923-161">La migration de cible.</span><span class="sxs-lookup"><span data-stu-id="b0923-161">The target migration.</span></span> <span data-ttu-id="b0923-162">Si 0, toutes les migrations vont être annulées.</span><span class="sxs-lookup"><span data-stu-id="b0923-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="b0923-163">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="b0923-164">informations de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="b0923-165">Obtient des informations sur un type de DbContext.</span><span class="sxs-lookup"><span data-stu-id="b0923-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="b0923-166">liste de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="b0923-167">Répertorie les types DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="b0923-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="b0923-168">structure de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="b0923-169">Structures un types DbContext et l’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="b0923-170">Arguments :</span><span class="sxs-lookup"><span data-stu-id="b0923-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="b0923-171">\<CONNEXION &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-171">\<CONNECTION></span></span> | <span data-ttu-id="b0923-172">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="b0923-173">\<FOURNISSEUR &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-173">\<PROVIDER></span></span>   | <span data-ttu-id="b0923-174">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-174">The provider to use.</span></span> <span data-ttu-id="b0923-175">(Par ex.</span><span class="sxs-lookup"><span data-stu-id="b0923-175">(E.g.</span></span> <span data-ttu-id="b0923-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="b0923-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="b0923-177">Options :</span><span class="sxs-lookup"><span data-stu-id="b0923-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0923-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="b0923-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="b0923-179">--annotations de données</span><span class="sxs-lookup"><span data-stu-id="b0923-179">--data-annotations</span></span>                      | <span data-ttu-id="b0923-180">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="b0923-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="b0923-181">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b0923-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="b0923-182">-c</span><span class="sxs-lookup"><span data-stu-id="b0923-182">-c</span></span>              | <span data-ttu-id="b0923-183">--contexte \<nom ></span><span class="sxs-lookup"><span data-stu-id="b0923-183">--context \<NAME></span></span>                       | <span data-ttu-id="b0923-184">Le nom de la DbContext.</span><span class="sxs-lookup"><span data-stu-id="b0923-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="b0923-185">--contexte-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="b0923-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="b0923-186">Répertoire à placer dans DbContext.</span><span class="sxs-lookup"><span data-stu-id="b0923-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="b0923-187">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="b0923-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="b0923-188">-f</span><span class="sxs-lookup"><span data-stu-id="b0923-188">-f</span></span>              | <span data-ttu-id="b0923-189">--forcer</span><span class="sxs-lookup"><span data-stu-id="b0923-189">--force</span></span>                                 | <span data-ttu-id="b0923-190">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="b0923-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="b0923-191">-o</span><span class="sxs-lookup"><span data-stu-id="b0923-191">-o</span></span>              | <span data-ttu-id="b0923-192">--sortie-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="b0923-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="b0923-193">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="b0923-193">The directory to put files in.</span></span> <span data-ttu-id="b0923-194">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="b0923-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="b0923-195"><nobr>--schéma \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="b0923-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="b0923-196">Les schémas des tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="b0923-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="b0923-197">-t</span><span class="sxs-lookup"><span data-stu-id="b0923-197">-t</span></span>              | <span data-ttu-id="b0923-198">--table \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="b0923-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="b0923-199">Les tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="b0923-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="b0923-200">--noms de base de données d’utilisation</span><span class="sxs-lookup"><span data-stu-id="b0923-200">--use-database-names</span></span>                    | <span data-ttu-id="b0923-201">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="b0923-202">ajouter des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-202">dotnet ef migrations add</span></span>

<span data-ttu-id="b0923-203">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-203">Adds a new migration.</span></span>

<span data-ttu-id="b0923-204">Arguments :</span><span class="sxs-lookup"><span data-stu-id="b0923-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="b0923-205">\<NOM &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-205">\<NAME></span></span> | <span data-ttu-id="b0923-206">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-206">The name of the migration.</span></span> |

<span data-ttu-id="b0923-207">Options :</span><span class="sxs-lookup"><span data-stu-id="b0923-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0923-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="b0923-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="b0923-209"><nobr>--sortie-dir \<chemin d’accès ></nobr></span><span class="sxs-lookup"><span data-stu-id="b0923-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="b0923-210">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b0923-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="b0923-211">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="b0923-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="b0923-212">La valeur par défaut est « Migration ».</span><span class="sxs-lookup"><span data-stu-id="b0923-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="b0923-213">liste de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-213">dotnet ef migrations list</span></span>

<span data-ttu-id="b0923-214">Répertorie les migrations disponibles.</span><span class="sxs-lookup"><span data-stu-id="b0923-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="b0923-215">supprimer des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="b0923-216">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-216">Removes the last migration.</span></span>

<span data-ttu-id="b0923-217">Options :</span><span class="sxs-lookup"><span data-stu-id="b0923-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="b0923-218">-f</span><span class="sxs-lookup"><span data-stu-id="b0923-218">-f</span></span> | <span data-ttu-id="b0923-219">--forcer</span><span class="sxs-lookup"><span data-stu-id="b0923-219">--force</span></span> | <span data-ttu-id="b0923-220">Rétablir la migration si elle a été appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="b0923-221">script de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0923-221">dotnet ef migrations script</span></span>

<span data-ttu-id="b0923-222">Génère un script SQL à partir de la migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="b0923-223">Arguments :</span><span class="sxs-lookup"><span data-stu-id="b0923-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="b0923-224">\<À PARTIR DE &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-224">\<FROM></span></span> | <span data-ttu-id="b0923-225">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="b0923-225">The starting migration.</span></span> <span data-ttu-id="b0923-226">La valeur par défaut est 0 (base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="b0923-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="b0923-227">\<POUR &GT;</span><span class="sxs-lookup"><span data-stu-id="b0923-227">\<TO></span></span>   | <span data-ttu-id="b0923-228">La fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-228">The ending migration.</span></span> <span data-ttu-id="b0923-229">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="b0923-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="b0923-230">Options :</span><span class="sxs-lookup"><span data-stu-id="b0923-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="b0923-231">-o</span><span class="sxs-lookup"><span data-stu-id="b0923-231">-o</span></span> | <span data-ttu-id="b0923-232">--sortie \<fichier ></span><span class="sxs-lookup"><span data-stu-id="b0923-232">--output \<FILE></span></span> | <span data-ttu-id="b0923-233">Le fichier dans lequel écrire le résultat à.</span><span class="sxs-lookup"><span data-stu-id="b0923-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="b0923-234">-i</span><span class="sxs-lookup"><span data-stu-id="b0923-234">-i</span></span> | <span data-ttu-id="b0923-235">--idempotent</span><span class="sxs-lookup"><span data-stu-id="b0923-235">--idempotent</span></span>     | <span data-ttu-id="b0923-236">Générer un script qui peut être utilisé sur toute migration, une base de données.</span><span class="sxs-lookup"><span data-stu-id="b0923-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
