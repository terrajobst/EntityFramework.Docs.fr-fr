---
title: Informations de référence sur les outils de EF Core (console du gestionnaire de package)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416717"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="9e899-102">Référence des outils de Entity Framework Core-console du gestionnaire de package dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e899-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="9e899-103">Les outils de la console du gestionnaire de package (PMC) pour Entity Framework Core effectuer des tâches de développement au moment du Design.</span><span class="sxs-lookup"><span data-stu-id="9e899-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="9e899-104">Par exemple, ils créent des [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), appliquent des migrations et génèrent du code pour un modèle basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="9e899-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="9e899-105">Les commandes s’exécutent dans Visual Studio à l’aide de la [console du gestionnaire de package](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="9e899-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="9e899-106">Ces outils fonctionnent avec les projets .NET Framework et .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e899-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="9e899-107">Si vous n’utilisez pas Visual Studio, nous vous recommandons d’utiliser les [outils en ligne de commande EF Core à](dotnet.md) la place.</span><span class="sxs-lookup"><span data-stu-id="9e899-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="9e899-108">Les outils CLI sont inter-plateformes et s’exécutent à l’intérieur d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="9e899-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="9e899-109">Installation des outils</span><span class="sxs-lookup"><span data-stu-id="9e899-109">Installing the tools</span></span>

<span data-ttu-id="9e899-110">Les procédures d’installation et de mise à jour des outils diffèrent entre ASP.NET Core 2.1 + et les versions antérieures ou d’autres types de projets.</span><span class="sxs-lookup"><span data-stu-id="9e899-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="9e899-111">ASP.NET Core version 2,1 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="9e899-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="9e899-112">Les outils sont inclus automatiquement dans un projet ASP.NET Core 2.1 +, car le package `Microsoft.EntityFrameworkCore.Tools` est inclus dans le [AspNetCore](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9e899-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="9e899-113">Par conséquent, vous n’avez rien à faire pour installer les outils, mais vous devez effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="9e899-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="9e899-114">Restaurez les packages avant d’utiliser les outils d’un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="9e899-115">Installez un package pour mettre à jour les outils vers une version plus récente.</span><span class="sxs-lookup"><span data-stu-id="9e899-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="9e899-116">Pour vous assurer que vous obtenez la version la plus récente des outils, nous vous recommandons également d’effectuer les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9e899-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="9e899-117">Modifiez votre fichier *. csproj* et ajoutez une ligne spécifiant la dernière version du package [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="9e899-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="9e899-118">Par exemple, le fichier *. csproj* peut inclure une `ItemGroup` qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="9e899-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="9e899-119">Mettez à jour les outils lorsque vous recevez un message comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9e899-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="9e899-120">La version des outils de EF Core « 2.1.1-RTM-30846 » est plus ancienne que celle du runtime « 2.1.3-RTM-32065 ».</span><span class="sxs-lookup"><span data-stu-id="9e899-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="9e899-121">Mettez à jour les outils des dernières fonctionnalités et correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="9e899-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="9e899-122">Pour mettre à jour les outils :</span><span class="sxs-lookup"><span data-stu-id="9e899-122">To update the tools:</span></span>

* <span data-ttu-id="9e899-123">Installez la dernière kit SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e899-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="9e899-124">Mettez à jour Visual Studio vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="9e899-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="9e899-125">Modifiez le fichier *. csproj* afin qu’il inclue une référence de package au package d’outils le plus récent, comme indiqué plus haut.</span><span class="sxs-lookup"><span data-stu-id="9e899-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="9e899-126">Autres types de versions et de projets</span><span class="sxs-lookup"><span data-stu-id="9e899-126">Other versions and project types</span></span>

<span data-ttu-id="9e899-127">Installez les outils de la console du gestionnaire de package en exécutant la commande suivante dans la **console du gestionnaire de package**:</span><span class="sxs-lookup"><span data-stu-id="9e899-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="9e899-128">Mettez à jour les outils en exécutant la commande suivante dans la **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="9e899-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="9e899-129">Vérifier l’installation</span><span class="sxs-lookup"><span data-stu-id="9e899-129">Verify the installation</span></span>

<span data-ttu-id="9e899-130">Vérifiez que les outils sont installés en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9e899-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="9e899-131">La sortie ressemble à ceci (il ne vous indique pas la version des outils que vous utilisez) :</span><span class="sxs-lookup"><span data-stu-id="9e899-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="9e899-132">Utilisation des outils</span><span class="sxs-lookup"><span data-stu-id="9e899-132">Using the tools</span></span>

<span data-ttu-id="9e899-133">Avant d’utiliser les outils :</span><span class="sxs-lookup"><span data-stu-id="9e899-133">Before using the tools:</span></span>

* <span data-ttu-id="9e899-134">Comprenez la différence entre le projet cible et le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="9e899-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="9e899-135">Découvrez comment utiliser les outils avec des bibliothèques de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e899-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="9e899-136">Pour les projets ASP.NET Core, définissez l’environnement.</span><span class="sxs-lookup"><span data-stu-id="9e899-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="9e899-137">Projet de démarrage et cible</span><span class="sxs-lookup"><span data-stu-id="9e899-137">Target and startup project</span></span>

<span data-ttu-id="9e899-138">Les commandes font référence à un *projet* et à un *projet de démarrage*.</span><span class="sxs-lookup"><span data-stu-id="9e899-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="9e899-139">Le *projet* est également appelé *projet cible* , car il s’agit de l’emplacement où les commandes ajoutent ou suppriment des fichiers.</span><span class="sxs-lookup"><span data-stu-id="9e899-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="9e899-140">Par défaut, le **projet par défaut** sélectionné dans la **console du gestionnaire de package** est le projet cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="9e899-141">Vous pouvez spécifier un projet différent comme projet cible à l’aide de l’option <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="9e899-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="9e899-142">Le *projet de démarrage* est celui que les outils génèrent et exécutent.</span><span class="sxs-lookup"><span data-stu-id="9e899-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="9e899-143">Les outils doivent exécuter le code de l’application au moment de la conception pour obtenir des informations sur le projet, telles que la chaîne de connexion à la base de données et la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="9e899-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="9e899-144">Par défaut, le **projet de démarrage** dans **Explorateur de solutions** est le projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="9e899-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="9e899-145">Vous pouvez spécifier un projet différent comme projet de démarrage à l’aide de l’option <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="9e899-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="9e899-146">Le projet de démarrage et le projet cible sont souvent le même projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="9e899-147">Un scénario classique dans lequel il s’agit de projets distincts est le suivant :</span><span class="sxs-lookup"><span data-stu-id="9e899-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="9e899-148">Le contexte de EF Core et les classes d’entité se trouvent dans une bibliothèque de classes .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e899-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="9e899-149">Une application console .NET Core ou une application Web fait référence à la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="9e899-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="9e899-150">Il est également possible de [Placer le code de migrations dans une bibliothèque de classes distincte du contexte de EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="9e899-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="9e899-151">Autres frameworks cibles</span><span class="sxs-lookup"><span data-stu-id="9e899-151">Other target frameworks</span></span>

<span data-ttu-id="9e899-152">Les outils de la console du gestionnaire de package fonctionnent avec .NET Core ou des projets .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e899-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="9e899-153">Les applications qui ont le modèle EF Core dans une bibliothèque de classes .NET Standard peuvent ne pas avoir de projet .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e899-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="9e899-154">C’est le cas, par exemple, des applications Xamarin et plateforme Windows universelle.</span><span class="sxs-lookup"><span data-stu-id="9e899-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="9e899-155">Dans ce cas, vous pouvez créer un projet d’application console .NET Core ou .NET Framework dont l’objectif est d’agir comme projet de démarrage pour les outils.</span><span class="sxs-lookup"><span data-stu-id="9e899-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="9e899-156">Le projet peut être un projet factice sans code réel &mdash; il n’est nécessaire que pour fournir une cible pour les outils.</span><span class="sxs-lookup"><span data-stu-id="9e899-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="9e899-157">Pourquoi un projet factice est-il nécessaire ?</span><span class="sxs-lookup"><span data-stu-id="9e899-157">Why is a dummy project required?</span></span> <span data-ttu-id="9e899-158">Comme mentionné précédemment, les outils doivent exécuter le code de l’application au moment de la conception.</span><span class="sxs-lookup"><span data-stu-id="9e899-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="9e899-159">Pour ce faire, ils doivent utiliser le Runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e899-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="9e899-160">Lorsque le modèle de EF Core se trouve dans un projet qui cible .NET Core ou .NET Framework, les outils de EF Core empruntent le runtime du projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="9e899-161">Ils ne peuvent pas le faire si le modèle de EF Core se trouve dans une bibliothèque de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e899-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="9e899-162">Le .NET Standard n’est pas une implémentation .NET réelle. Il s’agit d’une spécification d’un ensemble d’API que les implémentations .NET doivent prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="9e899-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="9e899-163">Par conséquent, .NET Standard n’est pas suffisant pour que les outils de EF Core exécutent le code d’application.</span><span class="sxs-lookup"><span data-stu-id="9e899-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="9e899-164">Le projet factice que vous créez à utiliser comme projet de démarrage fournit une plateforme cible concrète dans laquelle les outils peuvent charger la bibliothèque de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e899-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="9e899-165">Environnement de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e899-165">ASP.NET Core environment</span></span>

<span data-ttu-id="9e899-166">Pour spécifier l’environnement pour les projets ASP.NET Core, définissez **env : ASPNETCORE_ENVIRONMENT** avant d’exécuter les commandes.</span><span class="sxs-lookup"><span data-stu-id="9e899-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="9e899-167">Paramètres communs</span><span class="sxs-lookup"><span data-stu-id="9e899-167">Common parameters</span></span>

<span data-ttu-id="9e899-168">Le tableau suivant montre les paramètres qui sont communs à toutes les commandes EF Core :</span><span class="sxs-lookup"><span data-stu-id="9e899-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="9e899-169">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-169">Parameter</span></span>                 | <span data-ttu-id="9e899-170">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-171">-Context \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-171">-Context \<String></span></span>        | <span data-ttu-id="9e899-172">Classe `DbContext` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="9e899-172">The `DbContext` class to use.</span></span> <span data-ttu-id="9e899-173">Nom de classe uniquement ou qualifié complet avec des espaces de noms.</span><span class="sxs-lookup"><span data-stu-id="9e899-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="9e899-174">Si ce paramètre est omis, EF Core recherche la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="9e899-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="9e899-175">S’il existe plusieurs classes de contexte, ce paramètre est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9e899-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="9e899-176">-Project \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-176">-Project \<String></span></span>        | <span data-ttu-id="9e899-177">Projet cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-177">The target project.</span></span> <span data-ttu-id="9e899-178">Si ce paramètre est omis, le **projet par défaut** pour la **console du gestionnaire de package** est utilisé comme projet cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="9e899-179">-StartupProject \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-179">-StartupProject \<String></span></span> | <span data-ttu-id="9e899-180">Projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="9e899-180">The startup project.</span></span> <span data-ttu-id="9e899-181">Si ce paramètre est omis, le **projet de démarrage** dans les propriétés de la **solution** est utilisé comme projet cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="9e899-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="9e899-182">-Verbose</span></span>                  | <span data-ttu-id="9e899-183">Affichez la sortie détaillée.</span><span class="sxs-lookup"><span data-stu-id="9e899-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="9e899-184">Pour afficher des informations d’aide sur une commande, utilisez la commande `Get-Help` de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e899-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="9e899-185">Les paramètres Context, Project et StartupProject prennent en charge l’expansion de tabulation.</span><span class="sxs-lookup"><span data-stu-id="9e899-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="9e899-186">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="9e899-186">Add-Migration</span></span>

<span data-ttu-id="9e899-187">Ajoute une nouvelle migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-187">Adds a new migration.</span></span>

<span data-ttu-id="9e899-188">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="9e899-188">Parameters:</span></span>

| <span data-ttu-id="9e899-189">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-189">Parameter</span></span>                         | <span data-ttu-id="9e899-190">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-191">Nom de <nobr>\<chaîne ><nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="9e899-192">Nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-192">The name of the migration.</span></span> <span data-ttu-id="9e899-193">Il s’agit d’un paramètre positionnel qui est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9e899-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="9e899-194"><nobr>-OutputDir \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="9e899-195">Répertoire (et sous-espace de noms) à utiliser.</span><span class="sxs-lookup"><span data-stu-id="9e899-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="9e899-196">Les chemins d’accès sont relatifs au répertoire du projet cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="9e899-197">La valeur par défaut est « migrations ».</span><span class="sxs-lookup"><span data-stu-id="9e899-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="9e899-198">Supprimer la base de données</span><span class="sxs-lookup"><span data-stu-id="9e899-198">Drop-Database</span></span>

<span data-ttu-id="9e899-199">Supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="9e899-199">Drops the database.</span></span>

<span data-ttu-id="9e899-200">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="9e899-200">Parameters:</span></span>

| <span data-ttu-id="9e899-201">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-201">Parameter</span></span> | <span data-ttu-id="9e899-202">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9e899-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="9e899-203">-WhatIf</span></span>   | <span data-ttu-id="9e899-204">Affichez la base de données qui sera supprimée, mais ne la supprimez pas.</span><span class="sxs-lookup"><span data-stu-id="9e899-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="9e899-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="9e899-205">Get-DbContext</span></span>

<span data-ttu-id="9e899-206">Obtient des informations sur un type de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9e899-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="9e899-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="9e899-207">Remove-Migration</span></span>

<span data-ttu-id="9e899-208">Supprime la dernière migration (restaure les modifications de code qui ont été effectuées pour la migration).</span><span class="sxs-lookup"><span data-stu-id="9e899-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="9e899-209">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="9e899-209">Parameters:</span></span>

| <span data-ttu-id="9e899-210">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-210">Parameter</span></span> | <span data-ttu-id="9e899-211">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-212">-Force</span><span class="sxs-lookup"><span data-stu-id="9e899-212">-Force</span></span>    | <span data-ttu-id="9e899-213">Rétablissez la migration (annulez les modifications qui ont été appliquées à la base de données).</span><span class="sxs-lookup"><span data-stu-id="9e899-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="9e899-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="9e899-214">Scaffold-DbContext</span></span>

<span data-ttu-id="9e899-215">Génère du code pour une `DbContext` et des types d’entités pour une base de données.</span><span class="sxs-lookup"><span data-stu-id="9e899-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="9e899-216">Pour que `Scaffold-DbContext` génère un type d’entité, la table de base de données doit avoir une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="9e899-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="9e899-217">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="9e899-217">Parameters:</span></span>

| <span data-ttu-id="9e899-218">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-218">Parameter</span></span>                          | <span data-ttu-id="9e899-219">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-220"><nobr>-Connection \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="9e899-221">Chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="9e899-221">The connection string to the database.</span></span> <span data-ttu-id="9e899-222">Pour les projets ASP.NET Core 2. x, la valeur peut être *Name =\<nom de la chaîne de connexion >* .</span><span class="sxs-lookup"><span data-stu-id="9e899-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="9e899-223">Dans ce cas, le nom provient des sources de configuration qui sont configurées pour le projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="9e899-224">Il s’agit d’un paramètre positionnel qui est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9e899-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="9e899-225"><nobr>-Provider \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="9e899-226">Fournisseur à utiliser.</span><span class="sxs-lookup"><span data-stu-id="9e899-226">The provider to use.</span></span> <span data-ttu-id="9e899-227">En général, il s’agit du nom du package NuGet, par exemple : `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="9e899-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="9e899-228">Il s’agit d’un paramètre positionnel qui est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9e899-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="9e899-229">-OutputDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-229">-OutputDir \<String></span></span>               | <span data-ttu-id="9e899-230">Répertoire dans lequel placer les fichiers.</span><span class="sxs-lookup"><span data-stu-id="9e899-230">The directory to put files in.</span></span> <span data-ttu-id="9e899-231">Les chemins d’accès sont relatifs au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="9e899-232">-ContextDir \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-232">-ContextDir \<String></span></span>              | <span data-ttu-id="9e899-233">Répertoire dans lequel placer le fichier `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9e899-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="9e899-234">Les chemins d’accès sont relatifs au répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="9e899-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="9e899-235">-Context \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-235">-Context \<String></span></span>                 | <span data-ttu-id="9e899-236">Nom de la classe `DbContext` à générer.</span><span class="sxs-lookup"><span data-stu-id="9e899-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="9e899-237">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="9e899-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="9e899-238">Schémas des tables pour lesquelles générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="9e899-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="9e899-239">Si ce paramètre est omis, tous les schémas sont inclus.</span><span class="sxs-lookup"><span data-stu-id="9e899-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="9e899-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="9e899-240">-Tables \<String[]></span></span>                | <span data-ttu-id="9e899-241">Tables pour lesquelles générer des types d’entité.</span><span class="sxs-lookup"><span data-stu-id="9e899-241">The tables to generate entity types for.</span></span> <span data-ttu-id="9e899-242">Si ce paramètre est omis, toutes les tables sont incluses.</span><span class="sxs-lookup"><span data-stu-id="9e899-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="9e899-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="9e899-243">-DataAnnotations</span></span>                   | <span data-ttu-id="9e899-244">Utilisez des attributs pour configurer le modèle (dans la mesure du possible).</span><span class="sxs-lookup"><span data-stu-id="9e899-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9e899-245">Si ce paramètre est omis, seule l’API Fluent est utilisée.</span><span class="sxs-lookup"><span data-stu-id="9e899-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="9e899-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="9e899-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="9e899-247">Utilisez les noms de table et de colonne exactement tels qu’ils apparaissent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="9e899-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="9e899-248">Si ce paramètre est omis, les noms de base de données sont modifiés pour être C# plus conformes aux conventions de style de nom.</span><span class="sxs-lookup"><span data-stu-id="9e899-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="9e899-249">-Force</span><span class="sxs-lookup"><span data-stu-id="9e899-249">-Force</span></span>                             | <span data-ttu-id="9e899-250">Remplacer les fichiers existants.</span><span class="sxs-lookup"><span data-stu-id="9e899-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="9e899-251">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9e899-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="9e899-252">Exemple qui génère uniquement les tables sélectionnées et crée le contexte dans un dossier distinct avec un nom spécifié :</span><span class="sxs-lookup"><span data-stu-id="9e899-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="9e899-253">Script-migration</span><span class="sxs-lookup"><span data-stu-id="9e899-253">Script-Migration</span></span>

<span data-ttu-id="9e899-254">Génère un script SQL qui applique toutes les modifications d’une migration sélectionnée à une autre migration sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="9e899-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="9e899-255">Paramètres :</span><span class="sxs-lookup"><span data-stu-id="9e899-255">Parameters:</span></span>

| <span data-ttu-id="9e899-256">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-256">Parameter</span></span>                | <span data-ttu-id="9e899-257">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-258">*-From* \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-258">*-From* \<String></span></span>        | <span data-ttu-id="9e899-259">Début de la migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-259">The starting migration.</span></span> <span data-ttu-id="9e899-260">Les migrations peuvent être identifiées par leur nom ou par leur ID.</span><span class="sxs-lookup"><span data-stu-id="9e899-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9e899-261">Le nombre 0 est un cas spécial qui signifie *avant la première migration*.</span><span class="sxs-lookup"><span data-stu-id="9e899-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="9e899-262">La valeur par défaut est 0.</span><span class="sxs-lookup"><span data-stu-id="9e899-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="9e899-263">*-To* \<chaîne ></span><span class="sxs-lookup"><span data-stu-id="9e899-263">*-To* \<String></span></span>          | <span data-ttu-id="9e899-264">Fin de la migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-264">The ending migration.</span></span> <span data-ttu-id="9e899-265">La valeur par défaut est la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="9e899-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="9e899-267">Générez un script qui peut être utilisé sur une base de données lors d’une migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="9e899-268">-Output \<String ></span><span class="sxs-lookup"><span data-stu-id="9e899-268">-Output \<String></span></span>        | <span data-ttu-id="9e899-269">Fichier dans lequel écrire le résultat.</span><span class="sxs-lookup"><span data-stu-id="9e899-269">The file to write the result to.</span></span> <span data-ttu-id="9e899-270">Si ce paramètre est omis, le fichier est créé avec un nom généré dans le même dossier que celui dans lequel les fichiers d’exécution de l’application sont créés, par exemple : */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="9e899-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="9e899-271">Les paramètres to, from et Output prennent en charge l’expansion de tabulation.</span><span class="sxs-lookup"><span data-stu-id="9e899-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="9e899-272">L’exemple suivant crée un script pour la migration InitialCreate, en utilisant le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="9e899-273">L’exemple suivant crée un script pour toutes les migrations après la migration de InitialCreate, à l’aide de l’ID de migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="9e899-274">Mettre à jour-base de données</span><span class="sxs-lookup"><span data-stu-id="9e899-274">Update-Database</span></span>

<span data-ttu-id="9e899-275">Met à jour la base de données jusqu’à la dernière migration ou à une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9e899-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="9e899-276">Paramètre</span><span class="sxs-lookup"><span data-stu-id="9e899-276">Parameter</span></span>                           | <span data-ttu-id="9e899-277">Description</span><span class="sxs-lookup"><span data-stu-id="9e899-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e899-278"><nobr> *-Migration* \<chaîne ></nobr></span><span class="sxs-lookup"><span data-stu-id="9e899-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="9e899-279">Migration cible.</span><span class="sxs-lookup"><span data-stu-id="9e899-279">The target migration.</span></span> <span data-ttu-id="9e899-280">Les migrations peuvent être identifiées par leur nom ou par leur ID.</span><span class="sxs-lookup"><span data-stu-id="9e899-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9e899-281">Le nombre 0 est un cas spécial qui signifie *avant la première migration* et entraîne la restauration de toutes les migrations.</span><span class="sxs-lookup"><span data-stu-id="9e899-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="9e899-282">Si aucune migration n’est spécifiée, la commande prend par défaut la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="9e899-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="9e899-283">Le paramètre de migration prend en charge l’expansion de tabulation.</span><span class="sxs-lookup"><span data-stu-id="9e899-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="9e899-284">L’exemple suivant rétablit toutes les migrations.</span><span class="sxs-lookup"><span data-stu-id="9e899-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="9e899-285">Les exemples suivants mettent à jour la base de données vers une migration spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9e899-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="9e899-286">Le premier utilise le nom de la migration et le second utilise l’ID de migration :</span><span class="sxs-lookup"><span data-stu-id="9e899-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="9e899-287">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9e899-287">Additional resources</span></span>

* [<span data-ttu-id="9e899-288">Migrations</span><span class="sxs-lookup"><span data-stu-id="9e899-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="9e899-289">Reconstitution de la logique des produits</span><span class="sxs-lookup"><span data-stu-id="9e899-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
