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
# <a name="connection-strings"></a><span data-ttu-id="6a727-102">Chaînes de connexion</span><span class="sxs-lookup"><span data-stu-id="6a727-102">Connection Strings</span></span>

<span data-ttu-id="6a727-103">La plupart des fournisseurs de base de données requièrent une certaine forme de chaîne de connexion pour se connecter à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6a727-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="6a727-104">Parfois, cette chaîne de connexion contient des informations sensibles qui doivent être protégées.</span><span class="sxs-lookup"><span data-stu-id="6a727-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="6a727-105">Vous serez peut-être amené à modifier la chaîne de connexion lorsque vous déplacez votre application entre environnements, tels que le développement, test et de production.</span><span class="sxs-lookup"><span data-stu-id="6a727-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="6a727-106">Applications .NET framework</span><span class="sxs-lookup"><span data-stu-id="6a727-106">.NET Framework Applications</span></span>

<span data-ttu-id="6a727-107">Les applications .NET framework, tels que WinForms, WPF, Console et ASP.NET 4, ont un modèle de chaîne de connexion éprouvée.</span><span class="sxs-lookup"><span data-stu-id="6a727-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="6a727-108">La chaîne de connexion doit être ajoutée à votre fichier App.config d’applications (Web.config si vous utilisez ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="6a727-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="6a727-109">Si votre chaîne de connexion contient des informations sensibles, telles que le nom d’utilisateur et mot de passe, vous pouvez protéger le contenu du fichier configuration en utilisant [de la Configuration protégée](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="6a727-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="6a727-110">Le `providerName` paramètre n’est pas requis sur les chaînes de connexion EF Core stockées dans le fichier App.config, car le fournisseur de base de données est configuré par le biais de code.</span><span class="sxs-lookup"><span data-stu-id="6a727-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="6a727-111">Vous pouvez lire la chaîne de connexion en utilisant le `ConfigurationManager` API dans le contexte de votre `OnConfiguring` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6a727-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="6a727-112">Vous devrez peut-être ajouter une référence à la `System.Configuration` assembly .NET framework à être en mesure d’utiliser cette API.</span><span class="sxs-lookup"><span data-stu-id="6a727-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="6a727-113">Plateforme Windows universelle (UWP)</span><span class="sxs-lookup"><span data-stu-id="6a727-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="6a727-114">Chaînes de connexion dans une application UWP sont généralement une connexion de SQLite qui spécifie uniquement un nom de fichier local.</span><span class="sxs-lookup"><span data-stu-id="6a727-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="6a727-115">Ils généralement ne contiennent pas d’informations sensibles et n’avez pas besoin d’être modifié comme une application est déployée.</span><span class="sxs-lookup"><span data-stu-id="6a727-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="6a727-116">Par conséquent, ces chaînes de connexion conviennent généralement à conserver dans le code, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6a727-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="6a727-117">Si vous souhaitez les déplacer en dehors du code puis UWP prend en charge le concept de paramètres, consultez le [section de paramètres de l’application de la documentation UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="6a727-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="6a727-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a727-118">ASP.NET Core</span></span>

<span data-ttu-id="6a727-119">Dans ASP.NET Core, le système de configuration est très souple, et la chaîne de connexion peut être stockée dans `appsettings.json`, une variable d’environnement, le magasin des secrets utilisateur ou une autre source de configuration.</span><span class="sxs-lookup"><span data-stu-id="6a727-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="6a727-120">Consultez le [section de Configuration de la documentation ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="6a727-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="6a727-121">L’exemple suivant montre la chaîne de connexion stockée dans `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="6a727-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="6a727-122">Le contexte est généralement configuré dans `Startup.cs` avec la chaîne de connexion en cours de lecture à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="6a727-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="6a727-123">Remarque la `GetConnectionString()` méthode recherche une valeur de configuration dont la clé est `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="6a727-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="6a727-124">Vous devez importer le [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) espace de noms pour utiliser cette méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="6a727-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
