---
title: Package Manager Console (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: aacf8c8564a3966db6202c9ff1c1c02a19a10814
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="062f6-102">Outils de la Console Gestionnaire de Package EF Core</span><span class="sxs-lookup"><span data-stu-id="062f6-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="062f6-103">Les outils EF Core Package Manager Console (PMC) est exécuté à l’intérieur de Visual Studio à l’aide de NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="062f6-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="062f6-104">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="062f6-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="062f6-105">N’utilisez ne pas Visual Studio ?</span><span class="sxs-lookup"><span data-stu-id="062f6-105">Not using Visual Studio?</span></span> <span data-ttu-id="062f6-106">Le [EF principaux outils de ligne de commande] [ 1] sont inter-plateformes et s’exécutent à l’intérieur d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="062f6-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="062f6-107">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="062f6-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="062f6-108">Installez les outils de la Console Gestionnaire de Package de base EF en installant le package NuGet de Microsoft.EntityFrameworkCore.Tools.</span><span class="sxs-lookup"><span data-stu-id="062f6-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="062f6-109">Vous pouvez l’installer en exécutant la commande suivante à l’intérieur de [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="062f6-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="062f6-110">Si tout fonctionne correctement, il se peut que vous devez être en mesure d’exécuter cette commande :</span><span class="sxs-lookup"><span data-stu-id="062f6-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="062f6-111">Si votre projet de démarrage cible .NET Standard, [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="062f6-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="062f6-112">Si vous utilisez **Windows universelles** ou **Xamarin**, déplacez votre code EF à une bibliothèque de classes .NET Standard et [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="062f6-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="062f6-113">Spécifiez la bibliothèque de classes en tant que projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="062f6-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="062f6-114">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="062f6-114">Using the tools</span></span>
---------------
<span data-ttu-id="062f6-115">Chaque fois que vous appelez une commande, il existe deux projets concernés :</span><span class="sxs-lookup"><span data-stu-id="062f6-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="062f6-116">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="062f6-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="062f6-117">Le projet cible par défaut est la **projet par défaut** sélectionné dans la Console du Gestionnaire de Package, mais peut également être spécifié à l’aide de-paramètre du projet.</span><span class="sxs-lookup"><span data-stu-id="062f6-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="062f6-118">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="062f6-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="062f6-119">La valeur par défaut une **définir comme projet de démarrage** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="062f6-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="062f6-120">Il peut également être spécifié à l’aide du paramètre - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="062f6-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="062f6-121">Paramètres communs :</span><span class="sxs-lookup"><span data-stu-id="062f6-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="062f6-122">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="062f6-122">-Context \<String></span></span>        | <span data-ttu-id="062f6-123">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="062f6-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="062f6-124">-Projet \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="062f6-124">-Project \<String></span></span>        | <span data-ttu-id="062f6-125">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="062f6-125">The project to use.</span></span>         |
| <span data-ttu-id="062f6-126">-StartupProject \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="062f6-126">-StartupProject \<String></span></span> | <span data-ttu-id="062f6-127">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="062f6-127">The startup project to use.</span></span> |
| <span data-ttu-id="062f6-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="062f6-128">-Verbose</span></span>                  | <span data-ttu-id="062f6-129">Afficher la sortie des commentaires.</span><span class="sxs-lookup"><span data-stu-id="062f6-129">Show verbose output.</span></span>        |

<span data-ttu-id="062f6-130">Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.</span><span class="sxs-lookup"><span data-stu-id="062f6-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="062f6-131">Les paramètres de contexte, du projet et StartupProject prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="062f6-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="062f6-132">Définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter pour spécifier l’environnement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="062f6-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="062f6-133">Commandes</span><span class="sxs-lookup"><span data-stu-id="062f6-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="062f6-134">Ajouter la Migration</span><span class="sxs-lookup"><span data-stu-id="062f6-134">Add-Migration</span></span>

<span data-ttu-id="062f6-135">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-135">Adds a new migration.</span></span>

<span data-ttu-id="062f6-136">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="062f6-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="062f6-137">***-Name*** \<String></span><span class="sxs-lookup"><span data-stu-id="062f6-137">***-Name*** \<String></span></span>             | <span data-ttu-id="062f6-138">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="062f6-139"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="062f6-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="062f6-140">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="062f6-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="062f6-141">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="062f6-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="062f6-142">La valeur par défaut est « Migration ».</span><span class="sxs-lookup"><span data-stu-id="062f6-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="062f6-143">Paramètres de **gras** sont requis et celles dans *italique* sont positionnels.</span><span class="sxs-lookup"><span data-stu-id="062f6-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="062f6-144">Déplacer la base de données</span><span class="sxs-lookup"><span data-stu-id="062f6-144">Drop-Database</span></span>

<span data-ttu-id="062f6-145">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-145">Drops the database.</span></span>

<span data-ttu-id="062f6-146">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="062f6-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="062f6-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="062f6-147">-WhatIf</span></span> | <span data-ttu-id="062f6-148">Afficher la base de données qui seront supprimés, mais ne pas supprimer.</span><span class="sxs-lookup"><span data-stu-id="062f6-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="062f6-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="062f6-149">Get-DbContext</span></span>

<span data-ttu-id="062f6-150">Obtient des informations sur un type de DbContext.</span><span class="sxs-lookup"><span data-stu-id="062f6-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="062f6-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="062f6-151">Remove-Migration</span></span>

<span data-ttu-id="062f6-152">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-152">Removes the last migration.</span></span>

<span data-ttu-id="062f6-153">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="062f6-153">Parameters:</span></span>

|        |                                                                       |
|:-------|:----------------------------------------------------------------------|
| <span data-ttu-id="062f6-154">-Force</span><span class="sxs-lookup"><span data-stu-id="062f6-154">-Force</span></span> | <span data-ttu-id="062f6-155">Ne pas vérifier si la migration a été appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="062f6-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="062f6-156">Scaffold-DbContext</span></span>

<span data-ttu-id="062f6-157">Structures un types DbContext et l’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="062f6-158">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="062f6-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="062f6-159"><nobr>***-Connection*** \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="062f6-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="062f6-160">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="062f6-161">***-Provider*** \<String></span><span class="sxs-lookup"><span data-stu-id="062f6-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="062f6-162">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="062f6-162">The provider to use.</span></span> <span data-ttu-id="062f6-163">(Par exemple)</span><span class="sxs-lookup"><span data-stu-id="062f6-163">(E.g.</span></span> <span data-ttu-id="062f6-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="062f6-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="062f6-165">-OutputDir \<String></span><span class="sxs-lookup"><span data-stu-id="062f6-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="062f6-166">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="062f6-166">The directory to put files in.</span></span> <span data-ttu-id="062f6-167">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="062f6-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="062f6-168">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="062f6-168">-Context \<String></span></span>                       | <span data-ttu-id="062f6-169">Le nom de la DbContext à générer.</span><span class="sxs-lookup"><span data-stu-id="062f6-169">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="062f6-170">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="062f6-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="062f6-171">Les schémas des tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="062f6-171">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="062f6-172">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="062f6-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="062f6-173">Les tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="062f6-173">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="062f6-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="062f6-174">-DataAnnotations</span></span>                         | <span data-ttu-id="062f6-175">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="062f6-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="062f6-176">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="062f6-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="062f6-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="062f6-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="062f6-178">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-178">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="062f6-179">-Force</span><span class="sxs-lookup"><span data-stu-id="062f6-179">-Force</span></span>                                   | <span data-ttu-id="062f6-180">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="062f6-180">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="062f6-181">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="062f6-181">Script-Migration</span></span>

<span data-ttu-id="062f6-182">Génère un script SQL à partir de la migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="062f6-183">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="062f6-183">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="062f6-184">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="062f6-184">*-From* \<String></span></span> | <span data-ttu-id="062f6-185">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="062f6-185">The starting migration.</span></span> <span data-ttu-id="062f6-186">La valeur par défaut est 0 (base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="062f6-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="062f6-187">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="062f6-187">*-To* \<String></span></span>   | <span data-ttu-id="062f6-188">La fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-188">The ending migration.</span></span> <span data-ttu-id="062f6-189">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="062f6-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="062f6-190">-Idempotent</span></span>       | <span data-ttu-id="062f6-191">Générer un script qui peut être utilisé sur toute migration, une base de données.</span><span class="sxs-lookup"><span data-stu-id="062f6-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="062f6-192">-Sortie \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="062f6-192">-Output \<String></span></span> | <span data-ttu-id="062f6-193">Le fichier dans lequel écrire le résultat à.</span><span class="sxs-lookup"><span data-stu-id="062f6-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="062f6-194">To, From, et les paramètres de sortie prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="062f6-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="062f6-195">Update-Database</span><span class="sxs-lookup"><span data-stu-id="062f6-195">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="062f6-196"><nobr>*-Migration* \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="062f6-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="062f6-197">La migration de cible.</span><span class="sxs-lookup"><span data-stu-id="062f6-197">The target migration.</span></span> <span data-ttu-id="062f6-198">Si 0, toutes les migrations vont être annulées.</span><span class="sxs-lookup"><span data-stu-id="062f6-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="062f6-199">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="062f6-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="062f6-200">Le paramètre de la Migration prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="062f6-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
