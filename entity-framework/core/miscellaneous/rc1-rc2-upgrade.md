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
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>La mise à niveau à partir d’EF Core 1.0 RC1 vers 1.0 RC2

Cet article fournit des conseils pour déplacer une application générée avec les packages de RC1 vers RC2.

## <a name="package-names-and-versions"></a>Versions et les noms de package

Entre RC1 et RC2, nous avons modifié à partir de « Entity Framework 7 » à « Entity Framework Core ». Vous trouverez plus d’informations sur les raisons de la modification dans [ce billet par Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Grâce à cette modification, les noms de package modifié à partir de `EntityFramework.*` à `Microsoft.EntityFrameworkCore.*` et nos versions de `7.0.0-rc1-final` à `1.0.0-rc2-final` (ou `1.0.0-preview1-final` pour les outils).

**Vous devrez supprimer complètement les packages de RC1, puis installez le RC2 celles.** Voici le mappage pour certains packages courants.

| Package de RC1                                               | Équivalent de RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Pas encore disponible pour RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Espaces de noms

Ainsi que les noms de package, espaces de noms a été remplacée par `Microsoft.Data.Entity.*` à `Microsoft.EntityFrameworkCore.*`. Vous pouvez gérer cette modification avec une recherche/remplacement de `using Microsoft.Data.Entity` avec `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Modifications de la Convention d’affectation de noms de table

Une modification significative de fonctionnelle dans RC2, nous avons consistait à utiliser le nom de la `DbSet<TEntity>` propriété pour une entité donnée comme nom de table il est mappé, au lieu de simplement le nom de classe. Vous trouverez plus d’informations sur cette modification dans [le problème connexe annonce](https://github.com/aspnet/Announcements/issues/167).

Pour les applications existantes de RC1, nous vous recommandons d’ajouter le code suivant au début de votre `OnModelCreating` méthode pour conserver la stratégie d’affectation de noms RC1 :

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Si vous souhaitez adopter la nouvelle stratégie d’affectation de noms, nous recommande correctement effectué le reste des étapes de mise à niveau et puis, en supprimant le code et création d’une migration pour appliquer la table renomme.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs change (uniquement pour les projets ASP.NET Core)

Dans la version RC1, vous deviez ajouter des services d’Entity Framework au fournisseur de services d’application - dans `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Dans RC2, vous pouvez supprimer les appels à `AddEntityFramework()`, `AddSqlServer()`, etc. :

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Vous devez également ajouter un constructeur, à votre contexte dérivé, qui accepte les options de contexte et les transmet au constructeur de base. Cela est nécessaire, car nous avons supprimé certaines la magie effrayant qui glissé dans les coulisses :

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Passer par un IServiceProvider

Si vous avez le code RC1 qui passe une `IServiceProvider` au contexte, il a été déplacé vers `DbContextOptions`, au lieu d’être un paramètre de constructeur distinct. Utilisez `DbContextOptionsBuilder.UseInternalServiceProvider(...)` pour définir le fournisseur de service.

### <a name="testing"></a>Test

Le scénario le plus courant pour effectuer cette opération était de contrôler la portée d’une base de données en mémoire lors du test. Consultez la mise à jour [test](testing/index.md) article pour obtenir un exemple de cette opération avec RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Résolution des Services internes à partir du fournisseur de services d’Application (uniquement pour les projets ASP.NET Core)

Si vous avez une application ASP.NET Core et EF pour résoudre des services internes à partir du fournisseur de service d’application, il existe une surcharge de `AddDbContext` qui vous permet de configurer cela :

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Nous vous recommandons d’autoriser EF gérer en interne ses propres services, sauf si vous avez une raison de combiner les services EF internes dans votre fournisseur de services d’application. La principale raison souhaitées pour ce faire consiste à utiliser votre fournisseur de services d’application pour remplacer les services EF utilise en interne

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Les commandes DNX = > CLI de .NET (uniquement pour les projets ASP.NET Core)

Si vous avez utilisé précédemment le `dnx ef` commandes pour les projets ASP.NET 5, ceux-ci ont été déplacées vers `dotnet ef` commandes. S’applique toujours la même syntaxe de commande. Vous pouvez utiliser `dotnet ef --help` pour des informations sur la syntaxe.

Les commandes sont inscrits a été modifiée dans RC2, en raison de DNX, qui est remplacée par l’interface CLI .NET. Commandes sont maintenant enregistrés dans un `tools` section `project.json`:

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
> Si vous utilisez Visual Studio, vous pouvez maintenant utiliser la Console du Gestionnaire de Package pour exécuter des commandes EF pour les projets ASP.NET Core (cela a été pas pris en charge dans la version RC1). Vous devez toujours enregistrer les commandes dans le `tools` section de `project.json` pour ce faire.

## <a name="package-manager-commands-require-powershell-5"></a>Commandes du Gestionnaire de package nécessitent PowerShell 5

Si vous utilisez les commandes Entity Framework dans la Console du Gestionnaire de Package dans Visual Studio, vous devez vous assurer PowerShell 5 est installé. Il s’agit d’une exigence temporaire qui sera supprimée dans la prochaine version (consultez [émettre #5327](https://github.com/aspnet/EntityFramework/issues/5327) pour plus d’informations).

## <a name="using-imports-in-projectjson"></a>À l’aide de « imports » dans project.json

Certaines des dépendances d’EF Core ne pas encore en charge .NET Standard. EF Core dans les projets .NET Standard et .NET Core peut nécessiter l’ajout de « imports » au fichier project.json en tant que solution de contournement temporaire.

Lors de l’ajout d’EF, restauration NuGet affiche ce message d’erreur :

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

La solution de contournement consiste à importer manuellement le profil portable « portable-net451 + win8 ». Cette force NuGet à traiter cette binaires qui correspondent à ce fournir une infrastructure compatible avec .NET Standard, même si elles ne sont pas. « Portable-net451 + win8 » n’est pas 100 % compatible avec .NET Standard, mais il est suffisamment compatible pour la transition à partir de la bibliothèque de classes portable vers .NET Standard. Les importations peuvent être supprimées lorsque les dépendances d’Entity Framework finira par mettre à niveau vers .NET Standard.

Plusieurs infrastructures peuvent être ajoutés à « imports » dans la syntaxe de tableau. Autres importations peuvent être nécessaires si vous ajoutez des bibliothèques supplémentaires à votre projet.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Consultez [émettre #5176](https://github.com/aspnet/EntityFramework/issues/5176).
