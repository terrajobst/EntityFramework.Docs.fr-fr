---
title: Console du Gestionnaire de package (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490848"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="6c800-102">Outils de la Console Gestionnaire de Package EF Core</span><span class="sxs-lookup"><span data-stu-id="6c800-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="6c800-103">Les outils Entity Framework Core Package Manager Console (PMC) est exécuté à l’intérieur de Visual Studio à l’aide de NuGet [Console du Gestionnaire de Package][2].</span><span class="sxs-lookup"><span data-stu-id="6c800-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="6c800-104">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c800-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="6c800-105">N’utilisez ne pas Visual Studio ?</span><span class="sxs-lookup"><span data-stu-id="6c800-105">Not using Visual Studio?</span></span> <span data-ttu-id="6c800-106">Le [outils de ligne de commande de EF Core] [ 1] sont multiplateformes et d’exécution à l’intérieur d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="6c800-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="6c800-107">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="6c800-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="6c800-108">Installer les outils de Console du Gestionnaire de Package EF Core en installant le package NuGet de Microsoft.EntityFrameworkCore.Tools.</span><span class="sxs-lookup"><span data-stu-id="6c800-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="6c800-109">Vous pouvez l’installer en exécutant la commande suivante à l’intérieur de [Console du Gestionnaire de Package][2].</span><span class="sxs-lookup"><span data-stu-id="6c800-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="6c800-110">Si tout fonctionne correctement, vous devez être en mesure d’exécuter cette commande :</span><span class="sxs-lookup"><span data-stu-id="6c800-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="6c800-111">Si votre projet de démarrage cible .NET Standard, [un framework pris en charge de ciblage croisé] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="6c800-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c800-112">Si vous utilisez **Windows universel** ou **Xamarin**, déplacer votre code EF vers une bibliothèque de classes .NET Standard et [un framework pris en charge de ciblage croisé] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="6c800-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="6c800-113">Spécifier la bibliothèque de classes en tant que projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="6c800-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="6c800-114">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="6c800-114">Using the tools</span></span>
---------------
<span data-ttu-id="6c800-115">Chaque fois que vous appelez une commande, deux projets sont impliqués :</span><span class="sxs-lookup"><span data-stu-id="6c800-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="6c800-116">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="6c800-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="6c800-117">Le projet cible par défaut est le **projet par défaut** sélectionné dans la Console du Gestionnaire de Package, mais peut également être spécifié à l’aide de-paramètre du projet.</span><span class="sxs-lookup"><span data-stu-id="6c800-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="6c800-118">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="6c800-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="6c800-119">Les valeurs par défaut une **définir comme projet de démarrage** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="6c800-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="6c800-120">Il peut également être spécifié en utilisant le paramètre - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="6c800-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="6c800-121">Paramètres communs :</span><span class="sxs-lookup"><span data-stu-id="6c800-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="6c800-122">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-122">-Context \<String></span></span>        | <span data-ttu-id="6c800-123">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6c800-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="6c800-124">-Projet \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-124">-Project \<String></span></span>        | <span data-ttu-id="6c800-125">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6c800-125">The project to use.</span></span>         |
| <span data-ttu-id="6c800-126">-StartupProject \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-126">-StartupProject \<String></span></span> | <span data-ttu-id="6c800-127">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6c800-127">The startup project to use.</span></span> |
| <span data-ttu-id="6c800-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="6c800-128">-Verbose</span></span>                  | <span data-ttu-id="6c800-129">Afficher la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="6c800-129">Show verbose output.</span></span>        |

<span data-ttu-id="6c800-130">Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.</span><span class="sxs-lookup"><span data-stu-id="6c800-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="6c800-131">Les paramètres de contexte, le projet et StartupProject prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="6c800-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="6c800-132">Définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter pour spécifier l’environnement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c800-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="6c800-133">Commandes</span><span class="sxs-lookup"><span data-stu-id="6c800-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="6c800-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="6c800-134">Add-Migration</span></span>

<span data-ttu-id="6c800-135">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="6c800-135">Adds a new migration.</span></span>

<span data-ttu-id="6c800-136">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="6c800-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c800-137">***-Name*** \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-137">***-Name*** \<String></span></span>             | <span data-ttu-id="6c800-138">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="6c800-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="6c800-139"><nobr>-OutputDir \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="6c800-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="6c800-140">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6c800-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="6c800-141">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="6c800-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="6c800-142">La valeur par défaut est « Migrations ».</span><span class="sxs-lookup"><span data-stu-id="6c800-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="6c800-143">Paramètres dans **gras** sont nécessaires et celles dans *italique* sont positionnels.</span><span class="sxs-lookup"><span data-stu-id="6c800-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="6c800-144">DROP Database</span><span class="sxs-lookup"><span data-stu-id="6c800-144">Drop-Database</span></span>

<span data-ttu-id="6c800-145">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-145">Drops the database.</span></span>

<span data-ttu-id="6c800-146">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="6c800-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="6c800-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="6c800-147">-WhatIf</span></span> | <span data-ttu-id="6c800-148">Afficher la base de données serait supprimée, mais ne la supprimez.</span><span class="sxs-lookup"><span data-stu-id="6c800-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="6c800-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="6c800-149">Get-DbContext</span></span>

<span data-ttu-id="6c800-150">Obtient des informations sur un type DbContext.</span><span class="sxs-lookup"><span data-stu-id="6c800-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="6c800-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="6c800-151">Remove-Migration</span></span>

<span data-ttu-id="6c800-152">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="6c800-152">Removes the last migration.</span></span>

<span data-ttu-id="6c800-153">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="6c800-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="6c800-154">-Force</span><span class="sxs-lookup"><span data-stu-id="6c800-154">-Force</span></span> | <span data-ttu-id="6c800-155">Rétablir la migration s’il a été appliqué à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="6c800-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="6c800-156">Scaffold-DbContext</span></span>

<span data-ttu-id="6c800-157">Permet de générer automatiquement un types DbContext et d’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="6c800-158">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="6c800-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c800-159"><nobr>***-Connexion*** \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="6c800-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="6c800-160">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="6c800-161">***-Fournisseur*** \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="6c800-162">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6c800-162">The provider to use.</span></span> <span data-ttu-id="6c800-163">(par exemple, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="6c800-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="6c800-164">-OutputDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="6c800-165">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="6c800-165">The directory to put files in.</span></span> <span data-ttu-id="6c800-166">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="6c800-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="6c800-167">-ContextDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="6c800-168">Le répertoire de placer le fichier de DbContext dans.</span><span class="sxs-lookup"><span data-stu-id="6c800-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="6c800-169">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="6c800-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="6c800-170">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-170">-Context \<String></span></span>                       | <span data-ttu-id="6c800-171">Le nom de la classe DbContext pour générer.</span><span class="sxs-lookup"><span data-stu-id="6c800-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="6c800-172">-Schémas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6c800-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="6c800-173">Les schémas des tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="6c800-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="6c800-174">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6c800-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="6c800-175">Les tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="6c800-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="6c800-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="6c800-176">-DataAnnotations</span></span>                         | <span data-ttu-id="6c800-177">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="6c800-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="6c800-178">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="6c800-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="6c800-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="6c800-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="6c800-180">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="6c800-181">-Force</span><span class="sxs-lookup"><span data-stu-id="6c800-181">-Force</span></span>                                   | <span data-ttu-id="6c800-182">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="6c800-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="6c800-183">Migration de script</span><span class="sxs-lookup"><span data-stu-id="6c800-183">Script-Migration</span></span>

<span data-ttu-id="6c800-184">Génère un script SQL à partir de migrations.</span><span class="sxs-lookup"><span data-stu-id="6c800-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="6c800-185">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="6c800-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="6c800-186">*-From* \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-186">*-From* \<String></span></span> | <span data-ttu-id="6c800-187">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="6c800-187">The starting migration.</span></span> <span data-ttu-id="6c800-188">La valeur par défaut est 0 (la base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="6c800-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="6c800-189">*-* \<Chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-189">*-To* \<String></span></span>   | <span data-ttu-id="6c800-190">La migration de fin.</span><span class="sxs-lookup"><span data-stu-id="6c800-190">The ending migration.</span></span> <span data-ttu-id="6c800-191">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="6c800-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="6c800-192">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="6c800-192">-Idempotent</span></span>       | <span data-ttu-id="6c800-193">Générer un script qui peut être utilisé sur toute migration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="6c800-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="6c800-194">-Sortie \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="6c800-194">-Output \<String></span></span> | <span data-ttu-id="6c800-195">Fichier dans lequel écrire le résultat.</span><span class="sxs-lookup"><span data-stu-id="6c800-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="6c800-196">To, From, et les paramètres de sortie prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="6c800-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="6c800-197">Mise à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="6c800-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="6c800-198"><nobr>*-Migration* \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="6c800-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="6c800-199">La migration de la cible.</span><span class="sxs-lookup"><span data-stu-id="6c800-199">The target migration.</span></span> <span data-ttu-id="6c800-200">Si '0', toutes les migrations seront annulées.</span><span class="sxs-lookup"><span data-stu-id="6c800-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="6c800-201">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="6c800-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="6c800-202">Le paramètre de Migration prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="6c800-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
