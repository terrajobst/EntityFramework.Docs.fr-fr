---
title: EF Core références relatives aux outils (Console du Gestionnaire de Package) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 9a57b58f8569ee1241e40c3809b03487d1d88e02
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834758"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="47329-102">Référence - Console du Gestionnaire de Package dans Visual Studio des outils Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="47329-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="47329-103">Les outils de la Console de gestionnaire de Package (PMC) pour Entity Framework Core effectuer des tâches de développement au moment du design.</span><span class="sxs-lookup"><span data-stu-id="47329-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="47329-104">Par exemple, ils créent [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), appliquer des migrations et générer du code pour un modèle basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="47329-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="47329-105">Les commandes s’exécutent à l’intérieur de Visual Studio en utilisant le [Console du Gestionnaire de Package](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="47329-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="47329-106">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="47329-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="47329-107">Si vous n’utilisez pas Visual Studio, nous vous recommandons du [outils de ligne de commande de EF Core](dotnet.md) à la place.</span><span class="sxs-lookup"><span data-stu-id="47329-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="47329-108">Les outils CLI sont multiplateformes et d’exécution à l’intérieur d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="47329-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="47329-109">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="47329-109">Installing the tools</span></span>

<span data-ttu-id="47329-110">Les procédures d’installation et de la mise à jour les outils diffèrent entre ASP.NET Core 2.1 + et les versions antérieures ou autres types de projets.</span><span class="sxs-lookup"><span data-stu-id="47329-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="47329-111">ASP.NET Core 2.1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="47329-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="47329-112">Les outils sont automatiquement inclus dans un projet ASP.NET Core 2.1 +, car le `Microsoft.EntityFrameworkCore.Tools` package est inclus dans le [Microsoft.AspNetCore.App métapackage](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="47329-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="47329-113">Par conséquent, vous n’avez rien à faire pour installer les outils, mais vous avez à :</span><span class="sxs-lookup"><span data-stu-id="47329-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="47329-114">Restaurer les packages avant d’utiliser les outils dans un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="47329-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="47329-115">Installer un package pour mettre à jour les outils vers une version plus récente.</span><span class="sxs-lookup"><span data-stu-id="47329-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="47329-116">Pour vous assurer que vous obtenez la dernière version des outils, nous vous recommandons d’également effectuer l’étape suivante :</span><span class="sxs-lookup"><span data-stu-id="47329-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="47329-117">Modifier votre *.csproj* fichier, puis ajoutez une ligne spécifiant la dernière version de la [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span><span class="sxs-lookup"><span data-stu-id="47329-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="47329-118">Par exemple, le *.csproj* fichier peut inclure un `ItemGroup` qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="47329-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="47329-119">Mettre à jour les outils lorsque vous recevez un message similaire à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="47329-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="47329-120">La version des outils EF Core « 2.1.1-rtm-30846 » est antérieure à celle du runtime « 2.1.3-rtm-32065 ».</span><span class="sxs-lookup"><span data-stu-id="47329-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="47329-121">Mettre à jour les outils pour les dernières fonctionnalités et correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="47329-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="47329-122">Pour mettre à jour les outils :</span><span class="sxs-lookup"><span data-stu-id="47329-122">To update the tools:</span></span>
* <span data-ttu-id="47329-123">Installez la dernière version du .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="47329-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="47329-124">Mettre à jour de Visual Studio vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="47329-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="47329-125">Modifier le *.csproj* fichier afin qu’il inclue une référence de package pour le dernier package d’outils, comme indiqué précédemment.</span><span class="sxs-lookup"><span data-stu-id="47329-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="47329-126">Autres versions et les types de projets</span><span class="sxs-lookup"><span data-stu-id="47329-126">Other versions and project types</span></span>

<span data-ttu-id="47329-127">Installer les outils de la Console du Gestionnaire de Package en exécutant la commande suivante **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="47329-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="47329-128">Mettre à jour les outils en exécutant la commande suivante dans **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="47329-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="47329-129">Vérifier l’installation</span><span class="sxs-lookup"><span data-stu-id="47329-129">Verify the installation</span></span>

<span data-ttu-id="47329-130">Vérifiez que les outils sont installés en exécutant cette commande :</span><span class="sxs-lookup"><span data-stu-id="47329-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="47329-131">La sortie ressemble à ceci (il n’indique pas quelle version des outils que vous utilisez) :</span><span class="sxs-lookup"><span data-stu-id="47329-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="47329-132">L’utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="47329-132">Using the tools</span></span>

<span data-ttu-id="47329-133">Avant d’utiliser les outils :</span><span class="sxs-lookup"><span data-stu-id="47329-133">Before using the tools:</span></span>
* <span data-ttu-id="47329-134">Comprendre la différence entre le projet de démarrage et cible.</span><span class="sxs-lookup"><span data-stu-id="47329-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="47329-135">Découvrez comment utiliser les outils avec des bibliothèques de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="47329-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="47329-136">Pour les projets ASP.NET Core, définissez l’environnement.</span><span class="sxs-lookup"><span data-stu-id="47329-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="47329-137">Projet de démarrage et cible</span><span class="sxs-lookup"><span data-stu-id="47329-137">Target and startup project</span></span>

<span data-ttu-id="47329-138">Les commandes font référence à un *projet* et un *projet de démarrage*.</span><span class="sxs-lookup"><span data-stu-id="47329-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="47329-139">Le *projet* est également connu sous le *projet cible* , car il est où les commandes ajoutent ou supprimer des fichiers.</span><span class="sxs-lookup"><span data-stu-id="47329-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="47329-140">Par défaut, le **projet par défaut** sélectionné dans **Console du Gestionnaire de Package** est le projet cible.</span><span class="sxs-lookup"><span data-stu-id="47329-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="47329-141">Vous pouvez spécifier un autre projet comme projet cible à l’aide de la <nobr> `--project` </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="47329-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="47329-142">Le *projet de démarrage* est celui que les outils de générer et exécuter.</span><span class="sxs-lookup"><span data-stu-id="47329-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="47329-143">Les outils ont exécuter du code de l’application au moment du design pour obtenir des informations sur le projet, telles que la chaîne de connexion de base de données et la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="47329-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="47329-144">Par défaut, le **projet de démarrage** dans **l’Explorateur de solutions** est le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="47329-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="47329-145">Vous pouvez spécifier un autre projet comme projet de démarrage à l’aide de la <nobr> `--startup-project` </nobr> option.</span><span class="sxs-lookup"><span data-stu-id="47329-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="47329-146">Le projet de démarrage et le projet cible sont souvent le même projet.</span><span class="sxs-lookup"><span data-stu-id="47329-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="47329-147">Un scénario classique dans lequel ils sont des projets distincts est cas suivants :</span><span class="sxs-lookup"><span data-stu-id="47329-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="47329-148">Les classes d’entité et de contexte EF Core sont dans une bibliothèque de classes .NET Core.</span><span class="sxs-lookup"><span data-stu-id="47329-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="47329-149">Une application console .NET Core ou une application web fait référence à la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="47329-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="47329-150">Il est également possible de [mettre le code de migrations dans une bibliothèque de classes distincte à partir du contexte EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="47329-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="47329-151">Autres frameworks cibles</span><span class="sxs-lookup"><span data-stu-id="47329-151">Other target frameworks</span></span>

<span data-ttu-id="47329-152">Les outils de la Console du Gestionnaire de Package fonctionnent avec des projets .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="47329-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="47329-153">Les applications qui ont le modèle EF Core dans une bibliothèque de classes .NET Standard peut-être pas un projet de .NET Framework ou le .NET Core.</span><span class="sxs-lookup"><span data-stu-id="47329-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="47329-154">Par exemple, cela est vrai pour les applications Xamarin et de la plateforme Windows universelle.</span><span class="sxs-lookup"><span data-stu-id="47329-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="47329-155">Dans ce cas, vous pouvez créer un projet d’application console .NET Core ou .NET Framework dont le seul but est d’agir en tant que projet de démarrage pour les outils.</span><span class="sxs-lookup"><span data-stu-id="47329-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="47329-156">Le projet peut être un projet factice sans code réel &mdash; il est uniquement nécessaire pour fournir une cible pour les outils.</span><span class="sxs-lookup"><span data-stu-id="47329-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="47329-157">Pourquoi est-un projet factice requis ?</span><span class="sxs-lookup"><span data-stu-id="47329-157">Why is a dummy project required?</span></span> <span data-ttu-id="47329-158">Comme mentionné précédemment, les outils ont exécuter du code de l’application au moment du design.</span><span class="sxs-lookup"><span data-stu-id="47329-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="47329-159">Pour ce faire, ils doivent utiliser le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="47329-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="47329-160">Lorsque le modèle EF Core est dans un projet qui cible .NET Core ou .NET Framework, les outils EF Core emprunt le runtime à partir du projet.</span><span class="sxs-lookup"><span data-stu-id="47329-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="47329-161">Ils ne le sauront pas si le modèle EF Core se trouve dans une bibliothèque de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="47329-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="47329-162">.NET Standard n’est pas une implémentation réelle de .NET ; Il est une spécification d’un ensemble d’API implémentations .NET doivent prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="47329-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="47329-163">Par conséquent, .NET Standard n’est pas suffisant pour les outils EF Core exécuter du code d’application.</span><span class="sxs-lookup"><span data-stu-id="47329-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="47329-164">Le projet factice que vous créez à utiliser en tant que projet de démarrage fournit une plateforme cible concrète dans lequel les outils peuvent charger la bibliothèque de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="47329-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="47329-165">Environnement ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47329-165">ASP.NET Core environment</span></span>

<span data-ttu-id="47329-166">Pour spécifier l’environnement pour les projets ASP.NET Core, définissez **env:ASPNETCORE_ENVIRONMENT** avant d’exécuter des commandes.</span><span class="sxs-lookup"><span data-stu-id="47329-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="47329-167">Paramètres communs</span><span class="sxs-lookup"><span data-stu-id="47329-167">Common parameters</span></span>

<span data-ttu-id="47329-168">Le tableau suivant présente les paramètres qui sont communes à toutes les commandes EF Core :</span><span class="sxs-lookup"><span data-stu-id="47329-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="47329-169">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-169">Parameter</span></span>                 | <span data-ttu-id="47329-170">Description</span><span class="sxs-lookup"><span data-stu-id="47329-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47329-171">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-171">-Context \<String></span></span>        | <span data-ttu-id="47329-172">Le `DbContext` classe à utiliser.</span><span class="sxs-lookup"><span data-stu-id="47329-172">The `DbContext` class to use.</span></span> <span data-ttu-id="47329-173">Nom de classe complet avec des espaces de noms ou uniquement.</span><span class="sxs-lookup"><span data-stu-id="47329-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="47329-174">Si ce paramètre est omis, EF Core recherche la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="47329-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="47329-175">S’il existe plusieurs classes de contexte, ce paramètre est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="47329-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="47329-176">-Projet \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-176">-Project \<String></span></span>        | <span data-ttu-id="47329-177">Le projet cible.</span><span class="sxs-lookup"><span data-stu-id="47329-177">The target project.</span></span> <span data-ttu-id="47329-178">Si ce paramètre est omis, le **projet par défaut** pour **Console du Gestionnaire de Package** est utilisé en tant que le projet cible.</span><span class="sxs-lookup"><span data-stu-id="47329-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="47329-179">-StartupProject \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-179">-StartupProject \<String></span></span> | <span data-ttu-id="47329-180">Le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="47329-180">The startup project.</span></span> <span data-ttu-id="47329-181">Si ce paramètre est omis, le **projet de démarrage** dans **propriétés de la Solution** est utilisé en tant que le projet cible.</span><span class="sxs-lookup"><span data-stu-id="47329-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="47329-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="47329-182">-Verbose</span></span>                  | <span data-ttu-id="47329-183">Afficher la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="47329-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="47329-184">Pour afficher les informations d’aide sur une commande, utilisez PowerShell `Get-Help` commande.</span><span class="sxs-lookup"><span data-stu-id="47329-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="47329-185">Les paramètres de contexte, le projet et StartupProject prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="47329-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="47329-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="47329-186">Add-Migration</span></span>

<span data-ttu-id="47329-187">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="47329-187">Adds a new migration.</span></span>

<span data-ttu-id="47329-188">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="47329-188">Parameters:</span></span>

| <span data-ttu-id="47329-189">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-189">Parameter</span></span>                         | <span data-ttu-id="47329-190">Description</span><span class="sxs-lookup"><span data-stu-id="47329-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47329-191"><nobr>-Name \<chaîne ><nobr></span><span class="sxs-lookup"><span data-stu-id="47329-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="47329-192">Le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="47329-192">The name of the migration.</span></span> <span data-ttu-id="47329-193">Ceci est un paramètre positionnel et est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="47329-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="47329-194"><nobr>-OutputDir \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="47329-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="47329-195">Le répertoire (et espace de noms secondaire) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="47329-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="47329-196">Chemins d’accès sont relatifs au répertoire de projet cible.</span><span class="sxs-lookup"><span data-stu-id="47329-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="47329-197">La valeur par défaut est « Migrations ».</span><span class="sxs-lookup"><span data-stu-id="47329-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="47329-198">DROP Database</span><span class="sxs-lookup"><span data-stu-id="47329-198">Drop-Database</span></span>

<span data-ttu-id="47329-199">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="47329-199">Drops the database.</span></span>

<span data-ttu-id="47329-200">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="47329-200">Parameters:</span></span>

| <span data-ttu-id="47329-201">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-201">Parameter</span></span> | <span data-ttu-id="47329-202">Description</span><span class="sxs-lookup"><span data-stu-id="47329-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="47329-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="47329-203">-WhatIf</span></span>   | <span data-ttu-id="47329-204">Afficher la base de données serait supprimée, mais ne la supprimez.</span><span class="sxs-lookup"><span data-stu-id="47329-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="47329-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="47329-205">Get-DbContext</span></span>

<span data-ttu-id="47329-206">Listes disponibles `DbContext` types.</span><span class="sxs-lookup"><span data-stu-id="47329-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="47329-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="47329-207">Remove-Migration</span></span>

<span data-ttu-id="47329-208">Supprime la dernière migration (annule les modifications de code qui ont été effectuées pour la migration).</span><span class="sxs-lookup"><span data-stu-id="47329-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="47329-209">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="47329-209">Parameters:</span></span>

| <span data-ttu-id="47329-210">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-210">Parameter</span></span> | <span data-ttu-id="47329-211">Description</span><span class="sxs-lookup"><span data-stu-id="47329-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="47329-212">-Force</span><span class="sxs-lookup"><span data-stu-id="47329-212">-Force</span></span>    | <span data-ttu-id="47329-213">Rétablir la migration (annuler les modifications qui ont été appliquées à la base de données).</span><span class="sxs-lookup"><span data-stu-id="47329-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="47329-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="47329-214">Scaffold-DbContext</span></span>

<span data-ttu-id="47329-215">Génère du code pour un `DbContext` et types d’entité pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="47329-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="47329-216">Dans l’ordre pour `Scaffold-DbContext` pour générer un type d’entité, la table de base de données doit avoir une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="47329-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="47329-217">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="47329-217">Parameters:</span></span>

| <span data-ttu-id="47329-218">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-218">Parameter</span></span>                          | <span data-ttu-id="47329-219">Description</span><span class="sxs-lookup"><span data-stu-id="47329-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47329-220"><nobr>-Connexion \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="47329-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="47329-221">La chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="47329-221">The connection string to the database.</span></span> <span data-ttu-id="47329-222">Pour les projets ASP.NET Core 2.x, la valeur peut être *nom =\<nom de chaîne de connexion >*.</span><span class="sxs-lookup"><span data-stu-id="47329-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="47329-223">Dans ce cas, le nom est fourni à partir des sources de configuration qui sont configurées pour le projet.</span><span class="sxs-lookup"><span data-stu-id="47329-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="47329-224">Ceci est un paramètre positionnel et est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="47329-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="47329-225"><nobr>-Fournisseur \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="47329-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="47329-226">Le fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="47329-226">The provider to use.</span></span> <span data-ttu-id="47329-227">En général, c’est le nom du package NuGet, par exemple : `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="47329-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="47329-228">Ceci est un paramètre positionnel et est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="47329-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="47329-229">-OutputDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-229">-OutputDir \<String></span></span>               | <span data-ttu-id="47329-230">Répertoire à placer les fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="47329-230">The directory to put files in.</span></span> <span data-ttu-id="47329-231">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="47329-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="47329-232">-ContextDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-232">-ContextDir \<String></span></span>              | <span data-ttu-id="47329-233">Le répertoire de placer le `DbContext` de fichiers dans.</span><span class="sxs-lookup"><span data-stu-id="47329-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="47329-234">Chemins d’accès sont relatif au répertoire de projet.</span><span class="sxs-lookup"><span data-stu-id="47329-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="47329-235">-Contexte \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-235">-Context \<String></span></span>                 | <span data-ttu-id="47329-236">Le nom de la `DbContext` classe à générer.</span><span class="sxs-lookup"><span data-stu-id="47329-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="47329-237">-Schémas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="47329-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="47329-238">Les schémas des tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="47329-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="47329-239">Si ce paramètre est omis, tous les schémas sont inclus.</span><span class="sxs-lookup"><span data-stu-id="47329-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="47329-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="47329-240">-Tables \<String[]></span></span>                | <span data-ttu-id="47329-241">Les tables pour générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="47329-241">The tables to generate entity types for.</span></span> <span data-ttu-id="47329-242">Si ce paramètre est omis, toutes les tables sont inclus.</span><span class="sxs-lookup"><span data-stu-id="47329-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="47329-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="47329-243">-DataAnnotations</span></span>                   | <span data-ttu-id="47329-244">Utilisez des attributs pour configurer le modèle (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="47329-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="47329-245">Si ce paramètre est omis, uniquement l’API fluent est utilisé.</span><span class="sxs-lookup"><span data-stu-id="47329-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="47329-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="47329-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="47329-247">Utiliser des noms de table et colonne exactement telles qu’elles apparaissent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="47329-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="47329-248">Si ce paramètre est omis, les noms de base de données sont modifiés pour mieux se conformer aux conventions de style de nom C#.</span><span class="sxs-lookup"><span data-stu-id="47329-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="47329-249">-Force</span><span class="sxs-lookup"><span data-stu-id="47329-249">-Force</span></span>                             | <span data-ttu-id="47329-250">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="47329-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="47329-251">Exemple :</span><span class="sxs-lookup"><span data-stu-id="47329-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="47329-252">Exemple de structure que les tables sélectionnées et crée le contexte dans un dossier distinct avec un nom spécifié :</span><span class="sxs-lookup"><span data-stu-id="47329-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="47329-253">Migration de script</span><span class="sxs-lookup"><span data-stu-id="47329-253">Script-Migration</span></span>

<span data-ttu-id="47329-254">Génère un script SQL qui s’applique toutes les modifications à partir d’une migration sélectionnée à une autre migration sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="47329-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="47329-255">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="47329-255">Parameters:</span></span>

| <span data-ttu-id="47329-256">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-256">Parameter</span></span>                | <span data-ttu-id="47329-257">Description</span><span class="sxs-lookup"><span data-stu-id="47329-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47329-258">*-From* \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-258">*-From* \<String></span></span>        | <span data-ttu-id="47329-259">La migration de départ.</span><span class="sxs-lookup"><span data-stu-id="47329-259">The starting migration.</span></span> <span data-ttu-id="47329-260">Migrations peuvent être identifiées par nom ou par ID.</span><span class="sxs-lookup"><span data-stu-id="47329-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="47329-261">La valeur 0 est un cas spécial signifie *avant la première migration*.</span><span class="sxs-lookup"><span data-stu-id="47329-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="47329-262">La valeur par défaut est 0.</span><span class="sxs-lookup"><span data-stu-id="47329-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="47329-263">*-* \<Chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-263">*-To* \<String></span></span>          | <span data-ttu-id="47329-264">La migration de fin.</span><span class="sxs-lookup"><span data-stu-id="47329-264">The ending migration.</span></span> <span data-ttu-id="47329-265">Valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="47329-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="47329-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="47329-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="47329-267">Générer un script qui peut être utilisé sur toute migration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="47329-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="47329-268">-Sortie \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="47329-268">-Output \<String></span></span>        | <span data-ttu-id="47329-269">Fichier dans lequel écrire le résultat.</span><span class="sxs-lookup"><span data-stu-id="47329-269">The file to write the result to.</span></span> <span data-ttu-id="47329-270">Si ce paramètre est omis, le fichier est créé avec un nom généré dans le même dossier que les fichiers exécutables de l’application sont créés, par exemple : */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="47329-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="47329-271">To, From, et les paramètres de sortie prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="47329-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="47329-272">L’exemple suivant crée un script pour la migration InitialCreate, en utilisant le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="47329-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="47329-273">L’exemple suivant crée un script pour toutes les migrations après la migration InitialCreate, à l’aide de l’ID de la migration.</span><span class="sxs-lookup"><span data-stu-id="47329-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="47329-274">Mise à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="47329-274">Update-Database</span></span>

<span data-ttu-id="47329-275">Met à jour la base de données pour la dernière migration ou pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="47329-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="47329-276">Paramètre</span><span class="sxs-lookup"><span data-stu-id="47329-276">Parameter</span></span>                           | <span data-ttu-id="47329-277">Description</span><span class="sxs-lookup"><span data-stu-id="47329-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47329-278"><nobr>*-Migration* \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="47329-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="47329-279">La migration de la cible.</span><span class="sxs-lookup"><span data-stu-id="47329-279">The target migration.</span></span> <span data-ttu-id="47329-280">Migrations peuvent être identifiées par nom ou par ID.</span><span class="sxs-lookup"><span data-stu-id="47329-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="47329-281">La valeur 0 est un cas spécial signifie *avant la première migration* et oblige toutes les migrations à rétablir.</span><span class="sxs-lookup"><span data-stu-id="47329-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="47329-282">Si aucune migration n’est spécifiée, la commande par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="47329-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="47329-283">Le paramètre de Migration prend en charge d’extension de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="47329-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="47329-284">L’exemple suivant rétablit toutes les migrations.</span><span class="sxs-lookup"><span data-stu-id="47329-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="47329-285">Les exemples suivants mettre à jour la base de données pour une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="47329-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="47329-286">La première utilise le nom de la migration et la seconde utilise l’ID de la migration :</span><span class="sxs-lookup"><span data-stu-id="47329-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```
