---
title: La mise à niveau à partir d’EF Core 1.0 RC2 vers RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949055"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="3abc8-102">La mise à niveau à partir d’EF Core 1.0 RC2 vers RTM</span><span class="sxs-lookup"><span data-stu-id="3abc8-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="3abc8-103">Cet article fournit des conseils pour déplacer une application générée avec les packages de RC2 à 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="3abc8-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="3abc8-104">Versions de package</span><span class="sxs-lookup"><span data-stu-id="3abc8-104">Package Versions</span></span>

<span data-ttu-id="3abc8-105">Les noms des packages de niveau supérieur que vous installeriez en général dans une application n’a pas changé entre RC2 et RTM.</span><span class="sxs-lookup"><span data-stu-id="3abc8-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="3abc8-106">**Vous devez mettre à niveau les packages installés pour les versions RTM :**</span><span class="sxs-lookup"><span data-stu-id="3abc8-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="3abc8-107">Packages de Runtime (par exemple, `Microsoft.EntityFrameworkCore.SqlServer`) a été remplacée par `1.0.0-rc2-final` à `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="3abc8-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="3abc8-108">Le `Microsoft.EntityFrameworkCore.Tools` package a été remplacée par `1.0.0-preview1-final` à `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="3abc8-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="3abc8-109">Notez que les outils sont toujours en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="3abc8-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="3abc8-110">Migrations existantes peut-être maxLength ajouté</span><span class="sxs-lookup"><span data-stu-id="3abc8-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="3abc8-111">Dans RC2, la définition de colonne dans une migration ressemblait à `table.Column<string>(nullable: true)` et la longueur de la colonne a été recherchée dans des métadonnées que nous stockons dans le code-behind de la migration.</span><span class="sxs-lookup"><span data-stu-id="3abc8-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="3abc8-112">Dans la version RTM, la longueur est désormais incluse dans le code structuré `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="3abc8-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="3abc8-113">Les migrations existantes qui ont été structurées avant d’utiliser la version RTM n’aura pas la `maxLength` argument spécifié.</span><span class="sxs-lookup"><span data-stu-id="3abc8-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="3abc8-114">Cela signifie que la longueur maximale prise en charge par la base de données sera utilisée (`nvarchar(max)` sur SQL Server).</span><span class="sxs-lookup"><span data-stu-id="3abc8-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="3abc8-115">Cela peut être adaptée pour certaines colonnes, mais les colonnes qui font partie d’une clé, clé étrangère, ou les index doivent être mis à jour pour inclure une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="3abc8-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="3abc8-116">Par convention, 450 correspond à la longueur maximale utilisés pour les clés, clés étrangères et les colonnes indexées.</span><span class="sxs-lookup"><span data-stu-id="3abc8-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="3abc8-117">Si vous avez explicitement configuré une longueur dans le modèle, vous devez utiliser cette longueur à la place.</span><span class="sxs-lookup"><span data-stu-id="3abc8-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="3abc8-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="3abc8-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="3abc8-119">Cette modification a une incidence sur les projets qui utilisent ASP.NET Identity et ont été créés à partir d’un pré-RTM de modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="3abc8-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="3abc8-120">Le modèle de projet inclut une migration permet de créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="3abc8-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="3abc8-121">Cette migration doit être modifiée pour spécifier une longueur maximale de `256` sur les colonnes suivantes.</span><span class="sxs-lookup"><span data-stu-id="3abc8-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="3abc8-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="3abc8-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="3abc8-123">Name</span><span class="sxs-lookup"><span data-stu-id="3abc8-123">Name</span></span>

    * <span data-ttu-id="3abc8-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="3abc8-124">NormalizedName</span></span>

*  <span data-ttu-id="3abc8-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="3abc8-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="3abc8-126">Messagerie</span><span class="sxs-lookup"><span data-stu-id="3abc8-126">Email</span></span>

   * <span data-ttu-id="3abc8-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="3abc8-127">NormalizedEmail</span></span>

   * <span data-ttu-id="3abc8-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="3abc8-128">NormalizedUserName</span></span>

   * <span data-ttu-id="3abc8-129">UserName</span><span class="sxs-lookup"><span data-stu-id="3abc8-129">UserName</span></span>

<span data-ttu-id="3abc8-130">Échec pour apporter cette modification entraîne l’exception suivante lors de la migration initiale est appliquée à une base de données.</span><span class="sxs-lookup"><span data-stu-id="3abc8-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="3abc8-131">.NET core : Supprimer « imports » dans project.json</span><span class="sxs-lookup"><span data-stu-id="3abc8-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="3abc8-132">Si vous cibliez .NET Core avec RC2, vous deviez ajouter `imports` à project.json en tant que solution de contournement temporaire pour certaines des dépendances d’EF Core prenant ne pas en charge .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="3abc8-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="3abc8-133">Vous pouvez maintenant en supprimer.</span><span class="sxs-lookup"><span data-stu-id="3abc8-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="3abc8-134">Depuis la version 1.0 RTM, le [du SDK .NET Core](https://www.microsoft.com/net/download/core) prend en charge n’est plus `project.json` ou que vous développez des applications .NET Core à l’aide de Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="3abc8-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="3abc8-135">Nous vous recommandons de [migrer de project.json vers csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="3abc8-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="3abc8-136">Si vous utilisez Visual Studio, nous vous recommandons de passer à [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3abc8-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="3abc8-137">UWP : Ajouter des redirections de liaison</span><span class="sxs-lookup"><span data-stu-id="3abc8-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="3abc8-138">Tentative d’exécution des commandes EF sur les projets de plateforme universelle Windows (UWP) entraîne l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="3abc8-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="3abc8-139">Vous devez ajouter manuellement des redirections de liaison pour le projet UWP.</span><span class="sxs-lookup"><span data-stu-id="3abc8-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="3abc8-140">Créez un fichier nommé `App.config` dans le projet de dossier racine et ajouter des redirections vers les versions correcte de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="3abc8-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
