---
title: La mise à niveau à partir de EF Core 1.0 RC2 vers la version RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/23/2018
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="16c53-102">La mise à niveau à partir de EF Core 1.0 RC2 vers la version RTM</span><span class="sxs-lookup"><span data-stu-id="16c53-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="16c53-103">Cet article fournit des conseils pour déplacer une application générée avec les packages 1.0.0 RC2 RTM.</span><span class="sxs-lookup"><span data-stu-id="16c53-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="16c53-104">Versions de package</span><span class="sxs-lookup"><span data-stu-id="16c53-104">Package Versions</span></span>

<span data-ttu-id="16c53-105">Les noms des packages de niveau supérieur que vous devez généralement installer dans une application n’a pas changé entre RC2 et RTM.</span><span class="sxs-lookup"><span data-stu-id="16c53-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="16c53-106">**Vous devez mettre à niveau les packages installés pour les versions RTM :**</span><span class="sxs-lookup"><span data-stu-id="16c53-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="16c53-107">Les packages d’exécution (par exemple, `Microsoft.EntityFrameworkCore.SqlServer`) a été remplacée par `1.0.0-rc2-final` à `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="16c53-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="16c53-108">Le `Microsoft.EntityFrameworkCore.Tools` package a été remplacée par `1.0.0-preview1-final` à `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="16c53-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="16c53-109">Notez que les outils sont toujours en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="16c53-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="16c53-110">Migrations existantes doit peut-être maxLength ajouté</span><span class="sxs-lookup"><span data-stu-id="16c53-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="16c53-111">Dans RC2, la définition de colonne dans une migration présentait `table.Column<string>(nullable: true)` et la longueur de la colonne a été recherchée dans des métadonnées que nous stockons dans le code-behind de la migration.</span><span class="sxs-lookup"><span data-stu-id="16c53-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="16c53-112">Dans la version finale, la longueur est désormais incluse dans le code de modèle généré automatiquement `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="16c53-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="16c53-113">Les migrations existantes qui ont été structurées avant d’utiliser la version RTM n’aura pas la `maxLength` argument spécifié.</span><span class="sxs-lookup"><span data-stu-id="16c53-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="16c53-114">Cela signifie que la longueur maximale prise en charge par la base de données sera utilisée (`nvarchar(max)` sur SQL Server).</span><span class="sxs-lookup"><span data-stu-id="16c53-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="16c53-115">Cela peut convenir pour certaines colonnes, mais les colonnes qui font partie d’une clé, la clé étrangère, ou les index doivent être mis à jour pour inclure une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="16c53-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="16c53-116">Par convention, 450 correspond à la longueur maximale utilisée pour les clés étrangères, clés et les colonnes indexées.</span><span class="sxs-lookup"><span data-stu-id="16c53-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="16c53-117">Si vous avez explicitement configuré une longueur dans le modèle, vous devez utiliser cette longueur à la place.</span><span class="sxs-lookup"><span data-stu-id="16c53-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="16c53-118">**Identité ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="16c53-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="16c53-119">Cette modification a un impact sur les projets qui utilisent ASP.NET Identity et ont été créés à partir d’un pré-RTM de modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="16c53-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="16c53-120">Le modèle de projet inclut une migration permet de créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="16c53-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="16c53-121">Cette migration doit être modifiée pour spécifier une longueur maximale de `256` sur les colonnes suivantes.</span><span class="sxs-lookup"><span data-stu-id="16c53-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="16c53-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="16c53-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="16c53-123">Name</span><span class="sxs-lookup"><span data-stu-id="16c53-123">Name</span></span>

    * <span data-ttu-id="16c53-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="16c53-124">NormalizedName</span></span>

*  <span data-ttu-id="16c53-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="16c53-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="16c53-126">Messagerie</span><span class="sxs-lookup"><span data-stu-id="16c53-126">Email</span></span>

   * <span data-ttu-id="16c53-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="16c53-127">NormalizedEmail</span></span>

   * <span data-ttu-id="16c53-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="16c53-128">NormalizedUserName</span></span>

   * <span data-ttu-id="16c53-129">UserName</span><span class="sxs-lookup"><span data-stu-id="16c53-129">UserName</span></span>

<span data-ttu-id="16c53-130">Pour effectuer ce changement entraînera l’exception suivante lors de la migration initiale est appliquée à une base de données.</span><span class="sxs-lookup"><span data-stu-id="16c53-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="16c53-131">.NET core : Supprimez « importations » dans project.json</span><span class="sxs-lookup"><span data-stu-id="16c53-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="16c53-132">Si vous ciblait .NET Core avec RC2, vous deviez ajouter `imports` à project.json en tant que solution de contournement temporaire pour certaines des dépendances de cœur EF ne pas de prise en charge .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="16c53-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="16c53-133">Vous pouvez maintenant les supprimer.</span><span class="sxs-lookup"><span data-stu-id="16c53-133">These can now be removed.</span></span>

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
> <span data-ttu-id="16c53-134">À partir de la version 1.0 RTM, les [le Kit de développement .NET Core](https://www.microsoft.com/net/download/core) ne prend plus en `project.json` ou développez des applications .NET Core à l’aide de Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="16c53-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="16c53-135">Nous vous recommandons de [migrer de project.json vers csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="16c53-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="16c53-136">Si vous utilisez Visual Studio, nous vous recommandons de vous mettre à niveau vers [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="16c53-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="16c53-137">UWP : Ajouter des redirections de liaison</span><span class="sxs-lookup"><span data-stu-id="16c53-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="16c53-138">Essayez d’exécuter des commandes d’EF sur les projets de plateforme Windows universelle (UWP) entraîne l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="16c53-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="16c53-139">Vous devez ajouter manuellement des redirections de liaison pour le projet UWP.</span><span class="sxs-lookup"><span data-stu-id="16c53-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="16c53-140">Créez un fichier nommé `App.config` dans le projet racine du dossier et ajouter des redirections vers les versions correcte de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="16c53-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
