---
title: Chaînes de connexion - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191340"
---
# <a name="connection-strings"></a>Chaînes de connexion

La plupart des fournisseurs de base de données requièrent une certaine forme de chaîne de connexion pour se connecter à la base de données. Parfois, cette chaîne de connexion contient des informations sensibles qui doivent être protégées. Vous serez peut-être amené à modifier la chaîne de connexion lorsque vous déplacez votre application entre environnements, tels que le développement, test et de production.

## <a name="net-framework-applications"></a>Applications .NET framework

Les applications .NET framework, tels que WinForms, WPF, Console et ASP.NET 4, ont un modèle de chaîne de connexion éprouvée. La chaîne de connexion doit être ajoutée à votre fichier App.config d’applications (Web.config si vous utilisez ASP.NET). Si votre chaîne de connexion contient des informations sensibles, telles que le nom d’utilisateur et mot de passe, vous pouvez protéger le contenu du fichier configuration en utilisant [de la Configuration protégée](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> Le `providerName` paramètre n’est pas requis sur les chaînes de connexion EF Core stockées dans le fichier App.config, car le fournisseur de base de données est configuré par le biais de code.

Vous pouvez lire la chaîne de connexion en utilisant le `ConfigurationManager` API dans le contexte de votre `OnConfiguring` (méthode). Vous devrez peut-être ajouter une référence à la `System.Configuration` assembly .NET framework à être en mesure d’utiliser cette API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Chaînes de connexion dans une application UWP sont généralement une connexion de SQLite qui spécifie uniquement un nom de fichier local. Ils généralement ne contiennent pas d’informations sensibles et n’avez pas besoin d’être modifié comme une application est déployée. Par conséquent, ces chaînes de connexion conviennent généralement à conserver dans le code, comme indiqué ci-dessous. Si vous souhaitez les déplacer en dehors du code puis UWP prend en charge le concept de paramètres, consultez le [section de paramètres de l’application de la documentation UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) pour plus d’informations.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

Dans ASP.NET Core, le système de configuration est très souple, et la chaîne de connexion peut être stockée dans `appsettings.json`, une variable d’environnement, le magasin des secrets utilisateur ou une autre source de configuration. Consultez le [section de Configuration de la documentation ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) pour plus d’informations. L’exemple suivant montre la chaîne de connexion stockée dans `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Le contexte est généralement configuré dans `Startup.cs` avec la chaîne de connexion en cours de lecture à partir de la configuration. Remarque la `GetConnectionString()` méthode recherche une valeur de configuration dont la clé est `ConnectionStrings:<connection string name>`. Vous devez importer le [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) espace de noms pour utiliser cette méthode d’extension.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
