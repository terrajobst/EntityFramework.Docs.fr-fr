---
title: Package Manager Console (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="a4dd0-102">Outils de la Console Gestionnaire de Package EF Core</span><span class="sxs-lookup"><span data-stu-id="a4dd0-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="a4dd0-103">Les outils EF Core Package Manager Console (PMC) est exécuté à l’intérieur de Visual Studio à l’aide de NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="a4dd0-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="a4dd0-104">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="a4dd0-105">N’utilisez ne pas Visual Studio ?</span><span class="sxs-lookup"><span data-stu-id="a4dd0-105">Not using Visual Studio?</span></span> <span data-ttu-id="a4dd0-106">Le [EF principaux outils de ligne de commande] [ 1] sont inter-plateformes et s’exécutent à l’intérieur d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a4dd0-107">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="a4dd0-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="a4dd0-108">Installez les outils de la Console Gestionnaire de Package de base EF en installant le package NuGet de Microsoft.EntityFrameworkCore.Tools.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="a4dd0-109">Vous pouvez l’installer en exécutant la commande suivante à l’intérieur de [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="a4dd0-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="a4dd0-110">Si tout fonctionne correctement, il se peut que vous devez être en mesure d’exécuter cette commande :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="a4dd0-111">Si votre projet de démarrage cible .NET Standard, [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4dd0-112">Si vous utilisez **Windows universelles** ou **Xamarin**, déplacez votre code EF à une bibliothèque de classes .NET Standard et [cross-cible une infrastructure de prise en charge] [ 3] avant d’utiliser les outils.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="a4dd0-113">Spécifiez la bibliothèque de classes en tant que projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="a4dd0-114">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="a4dd0-114">Using the tools</span></span>
---------------
<span data-ttu-id="a4dd0-115">Chaque fois que vous appelez une commande, il existe deux projets concernés :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a4dd0-116">Le projet cible contient les fichiers qui sont ajoutés (ou dans certains cas supprimés).</span><span class="sxs-lookup"><span data-stu-id="a4dd0-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a4dd0-117">Le projet cible par défaut est la **projet par défaut** sélectionné dans la Console du Gestionnaire de Package, mais peut également être spécifié à l’aide de-paramètre du projet.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="a4dd0-118">Le projet de démarrage est le projet qu’émulent les outils durant l’exécution du code de votre projet.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a4dd0-119">La valeur par défaut une **définir comme projet de démarrage** dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="a4dd0-120">Il peut également être spécifié à l’aide du paramètre - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="a4dd0-121">Paramètres communs :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="a4dd0-122">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-122">-Context \<String></span></span>        | <span data-ttu-id="a4dd0-123">DbContext à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="a4dd0-124">-Projet \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-124">-Project \<String></span></span>        | <span data-ttu-id="a4dd0-125">Le projet à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-125">The project to use.</span></span>         |
| <span data-ttu-id="a4dd0-126">-StartupProject \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-126">-StartupProject \<String></span></span> | <span data-ttu-id="a4dd0-127">Le projet de démarrage à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-127">The startup project to use.</span></span> |
| <span data-ttu-id="a4dd0-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="a4dd0-128">-Verbose</span></span>                  | <span data-ttu-id="a4dd0-129">Afficher la sortie des commentaires.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-129">Show verbose output.</span></span>        |

<span data-ttu-id="a4dd0-130">Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="a4dd0-131">Les paramètres de contexte, du projet et StartupProject prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="a4dd0-132">Définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter pour spécifier l’environnement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="a4dd0-133">Commandes</span><span class="sxs-lookup"><span data-stu-id="a4dd0-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="a4dd0-134">Ajouter la Migration</span><span class="sxs-lookup"><span data-stu-id="a4dd0-134">Add-Migration</span></span>

<span data-ttu-id="a4dd0-135">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-135">Adds a new migration.</span></span>

<span data-ttu-id="a4dd0-136">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a4dd0-137">***-Name*** \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-137">***-Name*** \<String></span></span>             | <span data-ttu-id="a4dd0-138">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="a4dd0-139"><nobr>-OutputDir \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="a4dd0-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="a4dd0-140">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a4dd0-141">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="a4dd0-142">La valeur par défaut est « Migration ».</span><span class="sxs-lookup"><span data-stu-id="a4dd0-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="a4dd0-143">Paramètres de **gras** sont requis et celles dans *italique* sont positionnels.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="a4dd0-144">Déplacer la base de données</span><span class="sxs-lookup"><span data-stu-id="a4dd0-144">Drop-Database</span></span>

<span data-ttu-id="a4dd0-145">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-145">Drops the database.</span></span>

<span data-ttu-id="a4dd0-146">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="a4dd0-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="a4dd0-147">-WhatIf</span></span> | <span data-ttu-id="a4dd0-148">Afficher la base de données qui seront supprimés, mais ne pas supprimer.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="a4dd0-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="a4dd0-149">Get-DbContext</span></span>

<span data-ttu-id="a4dd0-150">Obtient des informations sur un type de DbContext.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="a4dd0-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="a4dd0-151">Remove-Migration</span></span>

<span data-ttu-id="a4dd0-152">Supprime la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-152">Removes the last migration.</span></span>

<span data-ttu-id="a4dd0-153">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="a4dd0-154">-Force</span><span class="sxs-lookup"><span data-stu-id="a4dd0-154">-Force</span></span> | <span data-ttu-id="a4dd0-155">Rétablir la migration si elle a été appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="a4dd0-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="a4dd0-156">Scaffold-DbContext</span></span>

<span data-ttu-id="a4dd0-157">Structures un types DbContext et l’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a4dd0-158">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a4dd0-159"><nobr>***-Connexion*** \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="a4dd0-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="a4dd0-160">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="a4dd0-161">***-Fournisseur*** \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="a4dd0-162">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-162">The provider to use.</span></span> <span data-ttu-id="a4dd0-163">(Par ex.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-163">(E.g.</span></span> <span data-ttu-id="a4dd0-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a4dd0-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="a4dd0-165">-OutputDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="a4dd0-166">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-166">The directory to put files in.</span></span> <span data-ttu-id="a4dd0-167">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="a4dd0-168">-ContextDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="a4dd0-169">Répertoire à placer dans DbContext.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="a4dd0-170">Chemins d’accès sont relatif au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a4dd0-171">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-171">-Context \<String></span></span>                       | <span data-ttu-id="a4dd0-172">Le nom de la DbContext à générer.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="a4dd0-173">-Schémas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="a4dd0-174">Les schémas des tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a4dd0-175">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="a4dd0-176">Les tables pour générer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="a4dd0-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="a4dd0-177">-DataAnnotations</span></span>                         | <span data-ttu-id="a4dd0-178">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="a4dd0-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a4dd0-179">Si omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a4dd0-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="a4dd0-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="a4dd0-181">Utilisez des noms de table et de colonne directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="a4dd0-182">-Force</span><span class="sxs-lookup"><span data-stu-id="a4dd0-182">-Force</span></span>                                   | <span data-ttu-id="a4dd0-183">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="a4dd0-184">Migration de script</span><span class="sxs-lookup"><span data-stu-id="a4dd0-184">Script-Migration</span></span>

<span data-ttu-id="a4dd0-185">Génère un script SQL à partir de la migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a4dd0-186">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="a4dd0-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a4dd0-187">*-From* \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-187">*-From* \<String></span></span> | <span data-ttu-id="a4dd0-188">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-188">The starting migration.</span></span> <span data-ttu-id="a4dd0-189">La valeur par défaut est 0 (base de données initiale).</span><span class="sxs-lookup"><span data-stu-id="a4dd0-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="a4dd0-190">*-* \<Chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-190">*-To* \<String></span></span>   | <span data-ttu-id="a4dd0-191">La fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-191">The ending migration.</span></span> <span data-ttu-id="a4dd0-192">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="a4dd0-193">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="a4dd0-193">-Idempotent</span></span>       | <span data-ttu-id="a4dd0-194">Générer un script qui peut être utilisé sur toute migration, une base de données.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="a4dd0-195">-Sortie \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="a4dd0-195">-Output \<String></span></span> | <span data-ttu-id="a4dd0-196">Le fichier dans lequel écrire le résultat à.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="a4dd0-197">To, From, et les paramètres de sortie prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="a4dd0-198">Base de données de mise à jour</span><span class="sxs-lookup"><span data-stu-id="a4dd0-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="a4dd0-199"><nobr>*-Migration* \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="a4dd0-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="a4dd0-200">La migration de cible.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-200">The target migration.</span></span> <span data-ttu-id="a4dd0-201">Si 0, toutes les migrations vont être annulées.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="a4dd0-202">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="a4dd0-203">Le paramètre de la Migration prend en charge le développement par tabulation.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
