---
title: Chaînes de connexion-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416588"
---
# <a name="connection-strings"></a>Chaînes de connexion

La plupart des fournisseurs de base de données requièrent une forme de chaîne de connexion pour se connecter à la base de données. Parfois, cette chaîne de connexion contient des informations sensibles qui doivent être protégées. Vous devrez peut-être également modifier la chaîne de connexion lorsque vous déplacez votre application entre des environnements, tels que le développement, le test et la production.

## <a name="winforms--wpf-applications"></a>WinForms & les applications WPF

Les applications WinForms, WPF et ASP.NET 4 ont un modèle de chaîne de connexion essayé et testé. La chaîne de connexion doit être ajoutée au fichier app. config de votre application (Web. config si vous utilisez ASP.NET). Si votre chaîne de connexion contient des informations sensibles, telles que le nom d’utilisateur et le mot de passe, vous pouvez protéger le contenu du fichier de configuration à l’aide de l' [outil Gestionnaire de secret](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> Le paramètre `providerName` n’est pas requis sur EF Core chaînes de connexion stockées dans App. config, car le fournisseur de base de données est configuré par le biais du code.

Vous pouvez ensuite lire la chaîne de connexion à l’aide de l’API `ConfigurationManager` dans la méthode `OnConfiguring` de votre contexte. Vous devrez peut-être ajouter une référence à l’assembly du Framework `System.Configuration` pour pouvoir utiliser cette API.

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

Les chaînes de connexion dans une application UWP sont généralement une connexion SQLite qui spécifie simplement un nom de fichier local. En général, elles ne contiennent pas d’informations sensibles et n’ont pas besoin d’être modifiées lorsqu’une application est déployée. Par conséquent, ces chaînes de connexion sont généralement confines dans le code, comme indiqué ci-dessous. Si vous souhaitez les déplacer hors du code, UWP prend en charge le concept de paramètres. pour plus d’informations, consultez la [section paramètres de l’application dans la documentation UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .

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

Dans ASP.NET Core le système de configuration est très flexible, et la chaîne de connexion peut être stockée dans `appsettings.json`, une variable d’environnement, le magasin des secrets de l’utilisateur ou une autre source de configuration. Pour plus d’informations, consultez la [section Configuration de la documentation de ASP.net Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) . L’exemple suivant illustre la chaîne de connexion stockée dans `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Le contexte est généralement configuré dans `Startup.cs` avec la chaîne de connexion lue à partir de la configuration. Notez que la méthode `GetConnectionString()` recherche une valeur de configuration dont la clé est `ConnectionStrings:<connection string name>`. Vous devez importer l’espace de noms [Microsoft. extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) pour utiliser cette méthode d’extension.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
