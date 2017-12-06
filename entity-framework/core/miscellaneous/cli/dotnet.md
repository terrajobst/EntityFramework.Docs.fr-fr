---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="845fe-102">Outils de ligne de commande du .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="845fe-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="845fe-103">Les outils de ligne de commande Entity Framework Core .NET sont une extension de l’inter-plateformes **dotnet** commande, qui fait partie de la [le Kit de développement .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="845fe-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="845fe-104">Si vous utilisez Visual Studio, nous vous recommandons de [les outils PMC] [ 1] à la place car ils fournissent une expérience intégrée.</span><span class="sxs-lookup"><span data-stu-id="845fe-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="845fe-105">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="845fe-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="845fe-106">Installer les outils de ligne de commande EF Core .NET à l’aide de ces étapes :</span><span class="sxs-lookup"><span data-stu-id="845fe-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="845fe-107">Modifiez le fichier projet et ajoutez Microsoft.EntityFrameworkCore.Tools.DotNet comme un élément DotNetCliToolReference (voir ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="845fe-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="845fe-108">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="845fe-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="845fe-109">Le projet résultant doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="845fe-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="845fe-110">Une référence au package avec `PrivateAssets="All"` signifie qu’il n’est pas exposé à des projets qui font référence à ce projet, ce qui est particulièrement utile pour les packages qui sont utilisés en général uniquement pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="845fe-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="845fe-111">Si vous avez tout à droite, vous devez être en mesure d’exécuter correctement la commande suivante dans une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="845fe-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="845fe-112">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="845fe-112">Using the tools</span></span>
---------------
<span data-ttu-id="845fe-113">Chaque fois que vous appelez une commande, il existe deux projets concernés :</span><span class="sxs-lookup"><span data-stu-id="845fe-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="845fe-114">Le projet cible est où les fichiers sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="845fe-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="845fe-115">Le projet cible par défaut du projet dans le répertoire actif, mais peut être modifié à l’aide de la <nobr> **--projet** </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="845fe-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="845fe-116">Le projet de démarrage est celui émulé par les outils lors de l’exécution de code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="845fe-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="845fe-117">Également par défaut est le projet dans le répertoire actif, il peut être modifié à l’aide de la **--projet de démarrage** option.</span><span class="sxs-lookup"><span data-stu-id="845fe-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="845fe-118">Options courantes :</span><span class="sxs-lookup"><span data-stu-id="845fe-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="845fe-119">--json</span><span class="sxs-lookup"><span data-stu-id="845fe-119">--json</span></span>                           | <span data-ttu-id="845fe-120">Afficher la sortie JSON.</span><span class="sxs-lookup"><span data-stu-id="845fe-120">Show JSON output.</span></span>           |
| <span data-ttu-id="845fe-121">-c</span><span class="sxs-lookup"><span data-stu-id="845fe-121">-c</span></span> | <span data-ttu-id="845fe-122">--contexte \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="845fe-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="845fe-123">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="845fe-124">-P</span><span class="sxs-lookup"><span data-stu-id="845fe-124">-p</span></span> | <span data-ttu-id="845fe-125">--projet \<projet ></span><span class="sxs-lookup"><span data-stu-id="845fe-125">--project \<PROJECT></span></span>             | <span data-ttu-id="845fe-126">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-126">The project to use.</span></span>         |
| <span data-ttu-id="845fe-127">-s</span><span class="sxs-lookup"><span data-stu-id="845fe-127">-s</span></span> | <span data-ttu-id="845fe-128">--projet de démarrage \<projet ></span><span class="sxs-lookup"><span data-stu-id="845fe-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="845fe-129">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="845fe-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="845fe-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="845fe-131">Le framework cible.</span><span class="sxs-lookup"><span data-stu-id="845fe-131">The target framework.</span></span>       |
|    | <span data-ttu-id="845fe-132">--configuration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="845fe-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="845fe-133">Configuration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="845fe-134">--runtime \<identificateur ></span><span class="sxs-lookup"><span data-stu-id="845fe-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="845fe-135">Le runtime à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-135">The runtime to use.</span></span>         |
| <span data-ttu-id="845fe-136">-h</span><span class="sxs-lookup"><span data-stu-id="845fe-136">-h</span></span> | <span data-ttu-id="845fe-137">--aide</span><span class="sxs-lookup"><span data-stu-id="845fe-137">--help</span></span>                           | <span data-ttu-id="845fe-138">Afficher les informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="845fe-138">Show help information.</span></span>      |
| <span data-ttu-id="845fe-139">-v</span><span class="sxs-lookup"><span data-stu-id="845fe-139">-v</span></span> | <span data-ttu-id="845fe-140">--détaillé</span><span class="sxs-lookup"><span data-stu-id="845fe-140">--verbose</span></span>                        | <span data-ttu-id="845fe-141">Afficher la sortie des commentaires.</span><span class="sxs-lookup"><span data-stu-id="845fe-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="845fe-142">--Aucune-couleur</span><span class="sxs-lookup"><span data-stu-id="845fe-142">--no-color</span></span>                       | <span data-ttu-id="845fe-143">Ne pas redéfinir la sortie.</span><span class="sxs-lookup"><span data-stu-id="845fe-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="845fe-144">--sortie de préfixe</span><span class="sxs-lookup"><span data-stu-id="845fe-144">--prefix-output</span></span>                  | <span data-ttu-id="845fe-145">Préfixe dont le niveau de sortie.</span><span class="sxs-lookup"><span data-stu-id="845fe-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="845fe-146">Pour spécifier l’environnement ASP.NET Core, définissez la **ASPNETCORE_ENVIRONMENT** variable d’environnement avant d’exécuter.</span><span class="sxs-lookup"><span data-stu-id="845fe-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="845fe-147">Commandes</span><span class="sxs-lookup"><span data-stu-id="845fe-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="845fe-148">suppression de base de données ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-148">dotnet ef database drop</span></span>

<span data-ttu-id="845fe-149">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-149">Drops the database.</span></span>

<span data-ttu-id="845fe-150">Options :</span><span class="sxs-lookup"><span data-stu-id="845fe-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="845fe-151">-f</span><span class="sxs-lookup"><span data-stu-id="845fe-151">-f</span></span> | <span data-ttu-id="845fe-152">--forcer</span><span class="sxs-lookup"><span data-stu-id="845fe-152">--force</span></span>   | <span data-ttu-id="845fe-153">Ne pas confirmer.</span><span class="sxs-lookup"><span data-stu-id="845fe-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="845fe-154">--exécution</span><span class="sxs-lookup"><span data-stu-id="845fe-154">--dry-run</span></span> | <span data-ttu-id="845fe-155">Afficher la base de données qui seront supprimés, mais ne pas supprimer.</span><span class="sxs-lookup"><span data-stu-id="845fe-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="845fe-156">mise à jour de la base de données DotNet ef</span><span class="sxs-lookup"><span data-stu-id="845fe-156">dotnet ef database update</span></span>

<span data-ttu-id="845fe-157">Met à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="845fe-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="845fe-158">Arguments :</span><span class="sxs-lookup"><span data-stu-id="845fe-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="845fe-159">\<MIGRATION ></span><span class="sxs-lookup"><span data-stu-id="845fe-159">\<MIGRATION></span></span> | <span data-ttu-id="845fe-160">La migration de cible.</span><span class="sxs-lookup"><span data-stu-id="845fe-160">The target migration.</span></span> <span data-ttu-id="845fe-161">Si 0, toutes les migrations vont être annulées.</span><span class="sxs-lookup"><span data-stu-id="845fe-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="845fe-162">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="845fe-163">informations de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="845fe-164">Obtient des informations sur un type de DbContext.</span><span class="sxs-lookup"><span data-stu-id="845fe-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="845fe-165">liste de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="845fe-166">Répertorie les types DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="845fe-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="845fe-167">structure de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="845fe-168">Structures un types DbContext et l’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="845fe-169">Arguments :</span><span class="sxs-lookup"><span data-stu-id="845fe-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="845fe-170">\<CONNEXION ></span><span class="sxs-lookup"><span data-stu-id="845fe-170">\<CONNECTION></span></span> | <span data-ttu-id="845fe-171">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="845fe-172">\<FOURNISSEUR ></span><span class="sxs-lookup"><span data-stu-id="845fe-172">\<PROVIDER></span></span>   | <span data-ttu-id="845fe-173">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-173">The provider to use.</span></span> <span data-ttu-id="845fe-174">(Par exemple :</span><span class="sxs-lookup"><span data-stu-id="845fe-174">(E.g.</span></span> <span data-ttu-id="845fe-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="845fe-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="845fe-176">Options :</span><span class="sxs-lookup"><span data-stu-id="845fe-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="845fe-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="845fe-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="845fe-178">--annotations de données</span><span class="sxs-lookup"><span data-stu-id="845fe-178">--data-annotations</span></span>                | <span data-ttu-id="845fe-179">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="845fe-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="845fe-180">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="845fe-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="845fe-181">-c</span><span class="sxs-lookup"><span data-stu-id="845fe-181">-c</span></span>        |       <span data-ttu-id="845fe-182">--contexte \<nom ></span><span class="sxs-lookup"><span data-stu-id="845fe-182">--context \<NAME></span></span>                 | <span data-ttu-id="845fe-183">Le nom de la DbContext.</span><span class="sxs-lookup"><span data-stu-id="845fe-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="845fe-184">-f</span><span class="sxs-lookup"><span data-stu-id="845fe-184">-f</span></span>        |       <span data-ttu-id="845fe-185">--forcer</span><span class="sxs-lookup"><span data-stu-id="845fe-185">--force</span></span>                           | <span data-ttu-id="845fe-186">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="845fe-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="845fe-187">-o</span><span class="sxs-lookup"><span data-stu-id="845fe-187">-o</span></span>        |       <span data-ttu-id="845fe-188">--sortie-dir \<chemin d’accès ></span><span class="sxs-lookup"><span data-stu-id="845fe-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="845fe-189">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="845fe-189">The directory to put files in.</span></span> <span data-ttu-id="845fe-190">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="845fe-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="845fe-191"><nobr>--schéma \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="845fe-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="845fe-192">Les schémas des tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="845fe-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="845fe-193">-t</span><span class="sxs-lookup"><span data-stu-id="845fe-193">-t</span></span>        |       <span data-ttu-id="845fe-194">--table \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="845fe-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="845fe-195">Les tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="845fe-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="845fe-196">--noms de base de données d’utilisation</span><span class="sxs-lookup"><span data-stu-id="845fe-196">--use-database-names</span></span>              | <span data-ttu-id="845fe-197">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="845fe-198">ajouter des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-198">dotnet ef migrations add</span></span>

<span data-ttu-id="845fe-199">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-199">Adds a new migration.</span></span>

<span data-ttu-id="845fe-200">Arguments :</span><span class="sxs-lookup"><span data-stu-id="845fe-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="845fe-201">\<NOM ></span><span class="sxs-lookup"><span data-stu-id="845fe-201">\<NAME></span></span> | <span data-ttu-id="845fe-202">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-202">The name of the migration.</span></span> |

<span data-ttu-id="845fe-203">Options :</span><span class="sxs-lookup"><span data-stu-id="845fe-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="845fe-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="845fe-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="845fe-205"><nobr>--sortie-dir \<chemin d’accès ></nobr></span><span class="sxs-lookup"><span data-stu-id="845fe-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="845fe-206">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="845fe-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="845fe-207">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="845fe-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="845fe-208">La valeur par défaut est « Migration ».</span><span class="sxs-lookup"><span data-stu-id="845fe-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="845fe-209">liste de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-209">dotnet ef migrations list</span></span>

<span data-ttu-id="845fe-210">Répertorie les migrations disponibles.</span><span class="sxs-lookup"><span data-stu-id="845fe-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="845fe-211">supprimer des migrations d’ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="845fe-212">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-212">Removes the last migration.</span></span>

<span data-ttu-id="845fe-213">Options :</span><span class="sxs-lookup"><span data-stu-id="845fe-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="845fe-214">-f</span><span class="sxs-lookup"><span data-stu-id="845fe-214">-f</span></span> | <span data-ttu-id="845fe-215">--forcer</span><span class="sxs-lookup"><span data-stu-id="845fe-215">--force</span></span> | <span data-ttu-id="845fe-216">Ne pas vérifier si la migration a été appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="845fe-217">script de migrations ef dotnet</span><span class="sxs-lookup"><span data-stu-id="845fe-217">dotnet ef migrations script</span></span>

<span data-ttu-id="845fe-218">Génère un script SQL à partir de la migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="845fe-219">Arguments :</span><span class="sxs-lookup"><span data-stu-id="845fe-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="845fe-220">\<À PARTIR DE ></span><span class="sxs-lookup"><span data-stu-id="845fe-220">\<FROM></span></span> | <span data-ttu-id="845fe-221">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="845fe-221">The starting migration.</span></span> <span data-ttu-id="845fe-222">La valeur par défaut est 0 (base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="845fe-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="845fe-223">\<POUR ></span><span class="sxs-lookup"><span data-stu-id="845fe-223">\<TO></span></span>   | <span data-ttu-id="845fe-224">La fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-224">The ending migration.</span></span> <span data-ttu-id="845fe-225">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="845fe-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="845fe-226">Options :</span><span class="sxs-lookup"><span data-stu-id="845fe-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="845fe-227">-o</span><span class="sxs-lookup"><span data-stu-id="845fe-227">-o</span></span> | <span data-ttu-id="845fe-228">--sortie \<fichier ></span><span class="sxs-lookup"><span data-stu-id="845fe-228">--output \<FILE></span></span> | <span data-ttu-id="845fe-229">Le fichier dans lequel écrire le résultat à.</span><span class="sxs-lookup"><span data-stu-id="845fe-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="845fe-230">-i</span><span class="sxs-lookup"><span data-stu-id="845fe-230">-i</span></span> | <span data-ttu-id="845fe-231">--idempotent</span><span class="sxs-lookup"><span data-stu-id="845fe-231">--idempotent</span></span>     | <span data-ttu-id="845fe-232">Générer un script qui peut être utilisé sur toute migration, une base de données.</span><span class="sxs-lookup"><span data-stu-id="845fe-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
