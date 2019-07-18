---
title: Mise à niveau de EF Core 1,0 RC1 vers RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5300fe459ec2b8ab9bb573c7284b009249071d65
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306453"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="e6f22-102">Mise à niveau de EF Core 1,0 RC1 vers 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="e6f22-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="e6f22-103">Cet article fournit des conseils pour déplacer une application générée avec les packages RC1 vers RC2.</span><span class="sxs-lookup"><span data-stu-id="e6f22-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="e6f22-104">Noms et versions des packages</span><span class="sxs-lookup"><span data-stu-id="e6f22-104">Package Names and Versions</span></span>

<span data-ttu-id="e6f22-105">Entre RC1 et RC2, nous avons changé de «Entity Framework 7» en «Entity Framework Core».</span><span class="sxs-lookup"><span data-stu-id="e6f22-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="e6f22-106">Pour plus d’informations sur les raisons de cette modification, consultez le [billet de blog de Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6f22-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="e6f22-107">En raison de cette modification, nos noms de packages `EntityFramework.*` ont `Microsoft.EntityFrameworkCore.*` été modifiés de à `7.0.0-rc1-final` `1.0.0-rc2-final` et de nos `1.0.0-preview1-final` versions de à (ou pour les outils).</span><span class="sxs-lookup"><span data-stu-id="e6f22-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="e6f22-108">**Vous devrez supprimer complètement les packages RC1, puis installer les packages RC2.**</span><span class="sxs-lookup"><span data-stu-id="e6f22-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="e6f22-109">Voici le mappage de certains packages courants.</span><span class="sxs-lookup"><span data-stu-id="e6f22-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="e6f22-110">Package RC1</span><span class="sxs-lookup"><span data-stu-id="e6f22-110">RC1 Package</span></span>                                               | <span data-ttu-id="e6f22-111">Équivalent RC2</span><span class="sxs-lookup"><span data-stu-id="e6f22-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="e6f22-112">EntityFramework. MicrosoftSqlServer 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e6f22-114">EntityFramework. SQLite 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e6f22-116">EntityFramework7. npgsql 3.1.0-RC1-3</span><span class="sxs-lookup"><span data-stu-id="e6f22-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="e6f22-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="e6f22-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="e6f22-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e6f22-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e6f22-122">EntityFramework. InMemory 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-123">Microsoft. EntityFrameworkCore. InMemory 1.0.0-RC2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e6f22-124">EntityFramework. IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="e6f22-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="e6f22-125">Pas encore disponible pour RC2</span><span class="sxs-lookup"><span data-stu-id="e6f22-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="e6f22-126">EntityFramework. Commands 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-Preview1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="e6f22-128">EntityFramework. MicrosoftSqlServer. Design 7.0.0-RC1-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="e6f22-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e6f22-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="e6f22-130">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="e6f22-130">Namespaces</span></span>

<span data-ttu-id="e6f22-131">Avec les noms de packages, les espaces de `Microsoft.Data.Entity.*` noms `Microsoft.EntityFrameworkCore.*`ont été modifiés de à.</span><span class="sxs-lookup"><span data-stu-id="e6f22-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="e6f22-132">Vous pouvez gérer cette modification avec une recherche/remplacement de `using Microsoft.Data.Entity` avec `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e6f22-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="e6f22-133">Modification de la Convention d’affectation des noms de table</span><span class="sxs-lookup"><span data-stu-id="e6f22-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="e6f22-134">Une modification fonctionnelle significative que nous avons apportée en RC2 consistait à `DbSet<TEntity>` utiliser le nom de la propriété pour une entité donnée comme nom de table auquel elle est mappée, plutôt que simplement le nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="e6f22-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="e6f22-135">Pour plus d’informations sur ce changement, consultez [le numéro d’annonce associé](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="e6f22-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="e6f22-136">Pour les applications RC1 existantes, nous vous recommandons d’ajouter le code suivant au début `OnModelCreating` de votre méthode pour conserver la stratégie d’appellation RC1:</span><span class="sxs-lookup"><span data-stu-id="e6f22-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="e6f22-137">Si vous souhaitez adopter la nouvelle stratégie de nommage, nous vous recommandons de terminer avec succès le reste des étapes de mise à niveau, puis de supprimer le code et de créer une migration pour appliquer les renommages de tables.</span><span class="sxs-lookup"><span data-stu-id="e6f22-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="e6f22-138">Modifications de AddDbContext/Startup.cs (projets ASP.NET Core uniquement)</span><span class="sxs-lookup"><span data-stu-id="e6f22-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e6f22-139">Dans RC1, vous deviez ajouter des services de Entity Framework au fournisseur `Startup.ConfigureServices(...)`de services d’application:</span><span class="sxs-lookup"><span data-stu-id="e6f22-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e6f22-140">Dans RC2, vous pouvez supprimer les appels à `AddEntityFramework()`, `AddSqlServer()`, etc.:</span><span class="sxs-lookup"><span data-stu-id="e6f22-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e6f22-141">Vous devez également ajouter un constructeur, à votre contexte dérivé, qui prend des options de contexte et les passe au constructeur de base.</span><span class="sxs-lookup"><span data-stu-id="e6f22-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="e6f22-142">Cela est nécessaire, car nous avons supprimé une partie de la magie de l’glissé en arrière-plan:</span><span class="sxs-lookup"><span data-stu-id="e6f22-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="e6f22-143">Passage d’un IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="e6f22-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="e6f22-144">Si vous avez du code RC1 qui passe `IServiceProvider` un au contexte, il a été déplacé vers `DbContextOptions`, plutôt que comme un paramètre de constructeur séparé.</span><span class="sxs-lookup"><span data-stu-id="e6f22-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="e6f22-145">Utilisez `DbContextOptionsBuilder.UseInternalServiceProvider(...)` pour définir le fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="e6f22-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="e6f22-146">Test</span><span class="sxs-lookup"><span data-stu-id="e6f22-146">Testing</span></span>

<span data-ttu-id="e6f22-147">Le scénario le plus courant pour effectuer cette tâche consistait à contrôler l’étendue d’une base de données InMemory lors du test.</span><span class="sxs-lookup"><span data-stu-id="e6f22-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="e6f22-148">Pour obtenir un exemple de cette procédure avec RC2, consultez l’article sur les [tests](testing/index.md) mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e6f22-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="e6f22-149">Résolution des services internes à partir du fournisseur de services d’application (projets ASP.NET Core uniquement)</span><span class="sxs-lookup"><span data-stu-id="e6f22-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e6f22-150">Si vous disposez d’une application ASP.net Core et que vous souhaitez que EF résolve les services internes à partir du fournisseur de services d' `AddDbContext` application, il existe une surcharge de qui vous permet de configurer ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="e6f22-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="e6f22-151">Nous vous recommandons d’autoriser EF à gérer ses propres services en interne, sauf si vous avez une raison d’associer les services EF internes à votre fournisseur de services d’application.</span><span class="sxs-lookup"><span data-stu-id="e6f22-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="e6f22-152">La raison principale pour laquelle vous pouvez effectuer cette opération consiste à utiliser votre fournisseur de services d’application pour remplacer les services utilisés en interne par EF.</span><span class="sxs-lookup"><span data-stu-id="e6f22-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="e6f22-153">Commandes DNX = > CLI .NET (projets ASP.NET Core uniquement)</span><span class="sxs-lookup"><span data-stu-id="e6f22-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e6f22-154">Si vous avez précédemment utilisé `dnx ef` les commandes pour les projets ASP.net 5, ceux-ci `dotnet ef` ont été déplacés vers des commandes.</span><span class="sxs-lookup"><span data-stu-id="e6f22-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="e6f22-155">La même syntaxe de commande s’applique toujours.</span><span class="sxs-lookup"><span data-stu-id="e6f22-155">The same command syntax still applies.</span></span> <span data-ttu-id="e6f22-156">Vous pouvez utiliser `dotnet ef --help` pour les informations de syntaxe.</span><span class="sxs-lookup"><span data-stu-id="e6f22-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="e6f22-157">La façon dont les commandes sont inscrites a changé en RC2, en raison du remplacement de DNX par l’interface CLI .NET.</span><span class="sxs-lookup"><span data-stu-id="e6f22-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="e6f22-158">Les commandes sont désormais inscrites `tools` dans une `project.json`section dans:</span><span class="sxs-lookup"><span data-stu-id="e6f22-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="e6f22-159">Si vous utilisez Visual Studio, vous pouvez désormais utiliser la console du gestionnaire de package pour exécuter des commandes EF pour les projets ASP.NET Core (cela n’était pas pris en charge dans RC1).</span><span class="sxs-lookup"><span data-stu-id="e6f22-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="e6f22-160">Vous devez toujours enregistrer les commandes dans la `tools` section de `project.json` pour effectuer cette opération.</span><span class="sxs-lookup"><span data-stu-id="e6f22-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="e6f22-161">Les commandes du gestionnaire de package requièrent PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="e6f22-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="e6f22-162">Si vous utilisez les commandes Entity Framework dans la console du gestionnaire de package dans Visual Studio, vous devez vous assurer que PowerShell 5 est installé.</span><span class="sxs-lookup"><span data-stu-id="e6f22-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="e6f22-163">Il s’agit d’une exigence temporaire qui sera supprimée dans la prochaine version (pour plus d’informations, consultez [#5327 du problème](https://github.com/aspnet/EntityFramework/issues/5327) ).</span><span class="sxs-lookup"><span data-stu-id="e6f22-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="e6f22-164">Utilisation de «Imports» dans Project. JSON</span><span class="sxs-lookup"><span data-stu-id="e6f22-164">Using "imports" in project.json</span></span>

<span data-ttu-id="e6f22-165">Certaines des dépendances de EF Core ne prennent pas encore en charge .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e6f22-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="e6f22-166">EF Core dans les projets .NET Standard et .NET Core peuvent nécessiter l’ajout de «Imports» à Project. JSON comme solution de contournement temporaire.</span><span class="sxs-lookup"><span data-stu-id="e6f22-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="e6f22-167">Lorsque vous ajoutez EF, la restauration NuGet affiche ce message d’erreur:</span><span class="sxs-lookup"><span data-stu-id="e6f22-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="e6f22-168">La solution consiste à importer manuellement le profil portable «portable-net451 + WIN8».</span><span class="sxs-lookup"><span data-stu-id="e6f22-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="e6f22-169">Cela force NuGet à traiter ces fichiers binaires qui correspondent à ce qui est fourni comme une infrastructure compatible avec .NET Standard, même s’ils ne le sont pas.</span><span class="sxs-lookup"><span data-stu-id="e6f22-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="e6f22-170">Bien que «portable-net451 + WIN8» ne soit pas compatible 100% avec .NET Standard, il est suffisamment compatible pour la transition de PCL vers .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e6f22-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="e6f22-171">Les importations peuvent être supprimées lorsque les dépendances EF finissent par être mises à niveau vers .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e6f22-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="e6f22-172">Plusieurs infrastructures peuvent être ajoutées à «Imports» dans la syntaxe de tableau.</span><span class="sxs-lookup"><span data-stu-id="e6f22-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="e6f22-173">D’autres importations peuvent être nécessaires si vous ajoutez des bibliothèques supplémentaires à votre projet.</span><span class="sxs-lookup"><span data-stu-id="e6f22-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="e6f22-174">Consultez [#5176 de problèmes](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="e6f22-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
