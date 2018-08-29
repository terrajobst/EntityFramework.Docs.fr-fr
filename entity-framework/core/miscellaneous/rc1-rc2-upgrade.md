---
title: La mise à niveau à partir d’EF Core 1.0 RC1 vers RC2 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996895"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="8a04b-102">La mise à niveau à partir d’EF Core 1.0 RC1 vers 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="8a04b-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="8a04b-103">Cet article fournit des conseils pour déplacer une application générée avec les packages de RC1 vers RC2.</span><span class="sxs-lookup"><span data-stu-id="8a04b-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="8a04b-104">Versions et les noms de package</span><span class="sxs-lookup"><span data-stu-id="8a04b-104">Package Names and Versions</span></span>

<span data-ttu-id="8a04b-105">Entre RC1 et RC2, nous avons modifié à partir de « Entity Framework 7 » à « Entity Framework Core ».</span><span class="sxs-lookup"><span data-stu-id="8a04b-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="8a04b-106">Vous trouverez plus d’informations sur les raisons de la modification dans [ce billet par Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a04b-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="8a04b-107">Grâce à cette modification, les noms de package modifié à partir de `EntityFramework.*` à `Microsoft.EntityFrameworkCore.*` et nos versions de `7.0.0-rc1-final` à `1.0.0-rc2-final` (ou `1.0.0-preview1-final` pour les outils).</span><span class="sxs-lookup"><span data-stu-id="8a04b-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="8a04b-108">**Vous devrez supprimer complètement les packages de RC1, puis installez le RC2 celles.**</span><span class="sxs-lookup"><span data-stu-id="8a04b-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="8a04b-109">Voici le mappage pour certains packages courants.</span><span class="sxs-lookup"><span data-stu-id="8a04b-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="8a04b-110">Package de RC1</span><span class="sxs-lookup"><span data-stu-id="8a04b-110">RC1 Package</span></span>                                               | <span data-ttu-id="8a04b-111">Équivalent de RC2</span><span class="sxs-lookup"><span data-stu-id="8a04b-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="8a04b-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8a04b-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8a04b-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="8a04b-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="8a04b-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="8a04b-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="8a04b-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8a04b-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8a04b-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8a04b-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="8a04b-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="8a04b-125">Pas encore disponible pour RC2</span><span class="sxs-lookup"><span data-stu-id="8a04b-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="8a04b-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="8a04b-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="8a04b-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8a04b-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="8a04b-130">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="8a04b-130">Namespaces</span></span>

<span data-ttu-id="8a04b-131">Ainsi que les noms de package, espaces de noms a été remplacée par `Microsoft.Data.Entity.*` à `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="8a04b-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="8a04b-132">Vous pouvez gérer cette modification avec une recherche/remplacement de `using Microsoft.Data.Entity` avec `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8a04b-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="8a04b-133">Modifications de la Convention d’affectation de noms de table</span><span class="sxs-lookup"><span data-stu-id="8a04b-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="8a04b-134">Une modification significative de fonctionnelle dans RC2, nous avons consistait à utiliser le nom de la `DbSet<TEntity>` propriété pour une entité donnée comme nom de table il est mappé, au lieu de simplement le nom de classe.</span><span class="sxs-lookup"><span data-stu-id="8a04b-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="8a04b-135">Vous trouverez plus d’informations sur cette modification dans [le problème connexe annonce](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="8a04b-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="8a04b-136">Pour les applications existantes de RC1, nous vous recommandons d’ajouter le code suivant au début de votre `OnModelCreating` méthode pour conserver la stratégie d’affectation de noms RC1 :</span><span class="sxs-lookup"><span data-stu-id="8a04b-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="8a04b-137">Si vous souhaitez adopter la nouvelle stratégie d’affectation de noms, nous recommande correctement effectué le reste des étapes de mise à niveau et puis, en supprimant le code et création d’une migration pour appliquer la table renomme.</span><span class="sxs-lookup"><span data-stu-id="8a04b-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="8a04b-138">AddDbContext / Startup.cs change (uniquement pour les projets ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8a04b-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8a04b-139">Dans la version RC1, vous deviez ajouter des services d’Entity Framework au fournisseur de services d’application - dans `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="8a04b-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="8a04b-140">Dans RC2, vous pouvez supprimer les appels à `AddEntityFramework()`, `AddSqlServer()`, etc. :</span><span class="sxs-lookup"><span data-stu-id="8a04b-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="8a04b-141">Vous devez également ajouter un constructeur, à votre contexte dérivé, qui accepte les options de contexte et les transmet au constructeur de base.</span><span class="sxs-lookup"><span data-stu-id="8a04b-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="8a04b-142">Cela est nécessaire, car nous avons supprimé certaines la magie effrayant qui glissé dans les coulisses :</span><span class="sxs-lookup"><span data-stu-id="8a04b-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="8a04b-143">Passer par un IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="8a04b-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="8a04b-144">Si vous avez le code RC1 qui passe une `IServiceProvider` au contexte, il a été déplacé vers `DbContextOptions`, au lieu d’être un paramètre de constructeur distinct.</span><span class="sxs-lookup"><span data-stu-id="8a04b-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="8a04b-145">Utilisez `DbContextOptionsBuilder.UseInternalServiceProvider(...)` pour définir le fournisseur de service.</span><span class="sxs-lookup"><span data-stu-id="8a04b-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="8a04b-146">Test</span><span class="sxs-lookup"><span data-stu-id="8a04b-146">Testing</span></span>

<span data-ttu-id="8a04b-147">Le scénario le plus courant pour effectuer cette opération était de contrôler la portée d’une base de données en mémoire lors du test.</span><span class="sxs-lookup"><span data-stu-id="8a04b-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="8a04b-148">Consultez la mise à jour [test](testing/index.md) article pour obtenir un exemple de cette opération avec RC2.</span><span class="sxs-lookup"><span data-stu-id="8a04b-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="8a04b-149">Résolution des Services internes à partir du fournisseur de services d’Application (uniquement pour les projets ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8a04b-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8a04b-150">Si vous avez une application ASP.NET Core et EF pour résoudre des services internes à partir du fournisseur de service d’application, il existe une surcharge de `AddDbContext` qui vous permet de configurer cela :</span><span class="sxs-lookup"><span data-stu-id="8a04b-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="8a04b-151">Nous vous recommandons d’autoriser EF gérer en interne ses propres services, sauf si vous avez une raison de combiner les services EF internes dans votre fournisseur de services d’application.</span><span class="sxs-lookup"><span data-stu-id="8a04b-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="8a04b-152">La principale raison souhaitées pour ce faire consiste à utiliser votre fournisseur de services d’application pour remplacer les services EF utilise en interne</span><span class="sxs-lookup"><span data-stu-id="8a04b-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="8a04b-153">Les commandes DNX = > CLI de .NET (uniquement pour les projets ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8a04b-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8a04b-154">Si vous avez utilisé précédemment le `dnx ef` commandes pour les projets ASP.NET 5, ceux-ci ont été déplacées vers `dotnet ef` commandes.</span><span class="sxs-lookup"><span data-stu-id="8a04b-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="8a04b-155">S’applique toujours la même syntaxe de commande.</span><span class="sxs-lookup"><span data-stu-id="8a04b-155">The same command syntax still applies.</span></span> <span data-ttu-id="8a04b-156">Vous pouvez utiliser `dotnet ef --help` pour des informations sur la syntaxe.</span><span class="sxs-lookup"><span data-stu-id="8a04b-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="8a04b-157">Les commandes sont inscrits a été modifiée dans RC2, en raison de DNX, qui est remplacée par l’interface CLI .NET.</span><span class="sxs-lookup"><span data-stu-id="8a04b-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="8a04b-158">Commandes sont maintenant enregistrés dans un `tools` section `project.json`:</span><span class="sxs-lookup"><span data-stu-id="8a04b-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="8a04b-159">Si vous utilisez Visual Studio, vous pouvez maintenant utiliser la Console du Gestionnaire de Package pour exécuter des commandes EF pour les projets ASP.NET Core (cela a été pas pris en charge dans la version RC1).</span><span class="sxs-lookup"><span data-stu-id="8a04b-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="8a04b-160">Vous devez toujours enregistrer les commandes dans le `tools` section de `project.json` pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="8a04b-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="8a04b-161">Commandes du Gestionnaire de package nécessitent PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="8a04b-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="8a04b-162">Si vous utilisez les commandes Entity Framework dans la Console du Gestionnaire de Package dans Visual Studio, vous devez vous assurer PowerShell 5 est installé.</span><span class="sxs-lookup"><span data-stu-id="8a04b-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="8a04b-163">Il s’agit d’une exigence temporaire qui sera supprimée dans la prochaine version (consultez [émettre #5327](https://github.com/aspnet/EntityFramework/issues/5327) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="8a04b-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="8a04b-164">À l’aide de « imports » dans project.json</span><span class="sxs-lookup"><span data-stu-id="8a04b-164">Using "imports" in project.json</span></span>

<span data-ttu-id="8a04b-165">Certaines des dépendances d’EF Core ne pas encore en charge .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a04b-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="8a04b-166">EF Core dans les projets .NET Standard et .NET Core peut nécessiter l’ajout de « imports » au fichier project.json en tant que solution de contournement temporaire.</span><span class="sxs-lookup"><span data-stu-id="8a04b-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="8a04b-167">Lors de l’ajout d’EF, restauration NuGet affiche ce message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="8a04b-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="8a04b-168">La solution de contournement consiste à importer manuellement le profil portable « portable-net451 + win8 ».</span><span class="sxs-lookup"><span data-stu-id="8a04b-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="8a04b-169">Cette force NuGet à traiter cette binaires qui correspondent à ce fournir une infrastructure compatible avec .NET Standard, même si elles ne sont pas.</span><span class="sxs-lookup"><span data-stu-id="8a04b-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="8a04b-170">« Portable-net451 + win8 » n’est pas 100 % compatible avec .NET Standard, mais il est suffisamment compatible pour la transition à partir de la bibliothèque de classes portable vers .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a04b-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="8a04b-171">Les importations peuvent être supprimées lorsque les dépendances d’Entity Framework finira par mettre à niveau vers .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a04b-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="8a04b-172">Plusieurs infrastructures peuvent être ajoutés à « imports » dans la syntaxe de tableau.</span><span class="sxs-lookup"><span data-stu-id="8a04b-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="8a04b-173">Autres importations peuvent être nécessaires si vous ajoutez des bibliothèques supplémentaires à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8a04b-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="8a04b-174">Consultez [émettre #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="8a04b-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
