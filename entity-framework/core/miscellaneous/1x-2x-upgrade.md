---
title: La mise à niveau depuis des versions antérieures à EF Core 2 - EF Core
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 30f4de794d42b1385145286e77c2e7c67987fea6
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678612"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Mise à niveau des applications à partir de versions antérieures à EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Commun de procédures à toutes les Applications

Mise à jour d’une application existante vers EF Core 2.0 peut nécessiter :

1. La mise à niveau de la plateforme .NET de cible de l’application qui prend en charge le Standard .NET 2.0. Consultez [prise en charge de plates-formes](../platforms/index.md) pour plus d’informations.

2. Identifier un fournisseur de base de données cible qui est compatible avec EF Core 2.0. Consultez [EF Core 2.0 requiert un fournisseur de base de 2.0 données](#ef-core-20-requires-a-20-database-provider) ci-dessous.

3. Mise à niveau de tous les packages d’EF Core (runtime et outils) vers la version 2.0. Reportez-vous à [l’installation de la base de EF](../get-started/install/index.md) pour plus d’informations.

4. Apportez les modifications de code nécessaire pour compenser les modifications avec rupture. Consultez le [modifications avec rupture](#breaking-changes) section ci-dessous pour plus de détails.

## <a name="aspnet-core-applications"></a>Applications ASP.NET Core

1. Voir en particulier le [nouveau modèle pour l’initialisation du fournisseur de service de l’application](#new-way-of-getting-application-services) décrites ci-dessous.

> [!TIP]  
> L’adoption de ce nouveau modèle lors de la mise à jour des applications vers la version 2.0 est vivement recommandée et est requis pour les fonctionnalités de produit comme Entity Framework Core Migrations fonctionne. L’autre alternative courante consiste à [implémenter *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Les applications ciblant ASP.NET Core 2.0 peuvent utiliser EF Core 2.0 sans dépendances supplémentaires en plus des fournisseurs de base de données tiers. Toutefois, les applications qui ciblent les versions précédentes d’ASP.NET Core devront mise à niveau vers ASP.NET Core 2.0 pour pouvoir utiliser EF Core 2.0. Pour plus d’informations sur la mise à niveau vers la version 2.0, les applications ASP.NET Core, consultez [la documentation d’ASP.NET Core sur l’objet](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Modifications avec rupture

Nous avons pris l’opportunité pour affiner considérablement de nos API existantes et les comportements de la version 2.0. Il existe quelques améliorations qui peuvent nécessiter la modification du code d’application existant, bien que nous pensons que pour la majorité des applications l’impact est faible, dans la plupart des cas nécessitant une recompilation et modifications guidées minimales pour remplacer les API obsolètes.

### <a name="new-way-of-getting-application-services"></a>Nouvelle façon d’obtenir les services d’application

Le modèle recommandé pour les applications web ASP.NET Core a été mis à jour pour la version 2.0 d’une manière qui a interrompu la logique au moment du design QU'EF principal utilisé dans 1.x. Précédemment au moment du design, EF Core essayez d’appeler `Startup.ConfigureServices` directement afin d’accéder au fournisseur de service de l’application. Dans ASP.NET 2.0 de base, la Configuration est initialisée en dehors de la `Startup` classe. Les applications utilisant EF Core généralement accèdent leur chaîne de connexion de Configuration, `Startup` en lui-même n’est plus suffisante. Si vous mettez à niveau une application de 1.x ASP.NET Core, vous pouvez recevoir l’erreur suivante lors de l’utilisation des outils EF Core.

> Aucun constructeur sans paramètre a été trouvé sur 'ApplicationContext'. Ajoutez un constructeur sans paramètre pour 'ApplicationContext' ou ajouter une implémentation de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' dans le même assembly en tant que 'ApplicationContext'

Un nouveau hook au moment du design a été ajouté dans le modèle par défaut de ASP.NET Core 2.0. La méthode statique `Program.BuildWebHost` méthode permet de noyaux EF accéder au fournisseur de services de l’application au moment du design. Si vous mettez à niveau une application de 1.x ASP.NET Core, vous devez mettre à jour vous `Program` classe ressemble à ce qui suit.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory renommé

Afin de prendre en charge les modèles de diverses applications et de donner aux utilisateurs de contrôler la façon leur `DbContext` est utilisé au moment du design, nous avons, dans le passé, fourni le `IDbContextFactory<TContext>` interface. Au moment du design, les outils de base de EF détecte des implémentations de cette interface dans votre projet et utilisez-le pour créer `DbContext` objets.

Cette interface a un nom très général qui induire en erreur certains utilisateurs à essayer de nouveau l’utiliser pour d’autres `DbContext`-création de scénarios. Ils ont été dépourvus lorsque les outils EF puis a tenté d’utiliser leur implémentation au moment du design et a provoqué des commandes telles que `Update-Database` ou `dotnet ef database update` échec.

Afin de communiquer la sémantique au moment du design fort de cette interface, nous avons renommé à `IDesignTimeDbContextFactory<TContext>`.

Pour la version 2.0 version le `IDbContextFactory<TContext>` existe toujours, mais est marqué comme obsolète.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions supprimées

En raison des modifications ASP.NET Core 2.0 décrites ci-dessus, nous avons constaté que `DbContextFactoryOptions` était n’est plus nécessaire sur le nouveau `IDesignTimeDbContextFactory<TContext>` interface. Voici les alternatives que vous devez utiliser à la place.

| DbContextFactoryOptions | Alternative                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Répertoire de travail au moment du design modifié

Les modifications de ASP.NET Core 2.0 requises également le répertoire de travail utilisé par `dotnet ef` pour s’aligner avec le répertoire de travail utilisé par Visual Studio lors de l’exécution de votre application. Un effet secondaire observable de ce est que SQLite les noms de fichiers sont désormais relatif au répertoire du projet et pas le répertoire de sortie qu’à réaliser.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 requiert un fournisseur de base de 2.0 données

EF Core 2.0 nous avons apporté plusieurs simplifications et des améliorations dans les fournisseurs de base de données de façon fonctionne. Cela signifie que les fournisseurs 1.0.x et 1.1.x ne fonctionnent pas avec EF Core 2.0.

Les fournisseurs SQL Server et SQLite sont fournis par l’équipe EF et les 2.0 versions seront disponibles dans le cadre de la version 2.0 de version. Les fournisseurs tiers open source pour [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), et [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) sont mis à jour pour la version 2.0. Pour tous les autres fournisseurs, veuillez contacter le rédacteur de fournisseur.

### <a name="logging-and-diagnostics-events-have-changed"></a>Événements de journalisation et de diagnostic ont été modifiés.

Remarque : ces modifications ne devraient pas affecter la plupart des codes d’application.

Les ID d’événement pour les messages envoyés à un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) ont été modifiés dans la version 2.0. Les ID d’événement sont maintenant uniques dans le code EF Core. En outre, ces messages suivent désormais le modèle standard de la journalisation structurée utilisé, par exemple, par le modèle MVC.

Les catégories d’enregistreurs d’événements ont également changé. Il existe désormais un jeu connu de catégories accessibles par le biais de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) événements maintenant utilisent les mêmes noms de ID d’événement correspondants `ILogger` messages. Les charges utiles d’événement sont tous les types nominaux dérivés [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

ID d’événement, les types de charge utile et les catégories sont documentées dans les [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) et [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classes.

Les ID ont également déplacés de Microsoft.EntityFrameworkCore.Infraestructure vers le nouvel espace de noms Microsoft.EntityFrameworkCore.Diagnostics.

### <a name="ef-core-relational-metadata-api-changes"></a>Modifications d’API de métadonnées relationnelles EF Core

EF Core 2.0 génère désormais un [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) différent par fournisseur utilisé. Cela est généralement transparent pour l’application. Il en résulte une simplification des API de métadonnées de niveau inférieur, au point que tout accès aux _concepts de métadonnées relationnelles communs_ est toujours établi par le biais d’un appel à `.Relational` au lieu de `.SqlServer`, `.Sqlite`, etc. Par exemple, 1.1.x code similaire à celui-ci :

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Doit maintenant être écrit comme suit :

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Au lieu d’utiliser des méthodes, telles que `ForSqlServerToTable`, méthodes d’extension sont maintenant disponibles pour écrire du code conditionnel basé sur le fournisseur actuel en cours d’utilisation. Exemple :

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Notez que cette modification s’applique uniquement aux API/métadonnées qui est définie pour _tous les_ fournisseurs relationnelles. Les API et les métadonnées reste le même lorsqu’il est spécifique à uniquement un seul fournisseur. Par exemple, les index ordonnés en clusters sont spécifiques à SQL Server, donc `ForSqlServerIsClustered` et `.SqlServer().IsClustered()` doit toujours être utilisé.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Ne pas prendre le contrôle du fournisseur de services EF

EF Core utilise interne `IServiceProvider` (autrement dit, un conteneur d’injection de dépendance) pour l’implémentation interne. Applications doivent autoriser Core EF créer et gérer ce fournisseur, sauf dans certains cas. Envisagez la suppression de tous les appels à `UseInternalServiceProvider`. Si une application a besoin d’appeler `UseInternalServiceProvider`, puis pensez [un problème de classement](https://github.com/aspnet/EntityFramework/Issues) afin de nous pouvons examiner autres façons de gérer votre scénario.

Appel de `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. n’est pas requis par le code d’application, sauf si `UseInternalServiceProvider` est également appelée. Supprimez tous les appels à `AddEntityFramework` ou `AddEntityFrameworkSqlServer`, etc. `AddDbContext` doit toujours être utilisé dans la même façon qu’avant.

### <a name="in-memory-databases-must-be-named"></a>Bases de données en mémoire doivent être nommés.

La base de données en mémoire sans nom global a été supprimé et à la place toutes les bases de données en mémoire doivent être nommés. Exemple :

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Cela crée/utilise une base de données nommée « MyDatabase ». Si `UseInMemoryDatabase` est appelée à nouveau avec le même nom, la même base de données en mémoire sera alors utilisée, ce qui lui permet d’être partagées par plusieurs instances de contexte.

### <a name="read-only-api-changes"></a>Modifications de l’API en lecture seule

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, et `IsStoreGeneratedAlways` ont été rendue obsolète et remplacé par [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) et [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Ces comportements s’appliquent à n’importe quelle propriété (pas uniquement générées par le magasin de propriétés) et déterminer comment la valeur de la propriété doit être utilisée lors de l’insertion dans une ligne de base de données (`BeforeSaveBehavior`) ou lorsque la mise à jour d’un fichier base de données ligne (`AfterSaveBehavior`).

Les propriétés marquées comme [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (par exemple, pour les colonnes calculées) par défaut ignore toute valeur actuellement définie sur la propriété. Cela signifie qu’une valeur générée par le magasin sera toujours obtenue indépendamment de si n’importe quelle valeur a été définie ou modifiée sur l’entité suivie. Cela peut être modifié en définissant une autre `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nouveau comportement de suppression ClientSetNull

Dans les versions précédentes, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) a un comportement pour les entités suivis par le contexte que plusieurs fermé mise en correspondance `SetNull` sémantique. Dans EF Core 2.0, un nouveau `ClientSetNull` comportement a été introduit en tant que la valeur par défaut pour les relations facultatif. Ce comportement a `SetNull` sémantique pour les entités suivies et `Restrict` comportement pour les bases de données créées à l’aide de EF Core. Dans notre expérience, il s’agit plus attendu/comportements utiles pour les entités de suivi et de la base de données. `DeleteBehavior.Restrict` est maintenant respectés pour les entités suivies lorsque la valeur pour les relations facultatif.

### <a name="provider-design-time-packages-removed"></a>Packages au moment du design fournisseur supprimés

Le `Microsoft.EntityFrameworkCore.Relational.Design` package a été supprimé. Son contenu a été consolidé dans `Microsoft.EntityFrameworkCore.Relational` et `Microsoft.EntityFrameworkCore.Design`.

Cela se propage dans les packages au moment du design de fournisseur. Ces packages (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) ont été supprimées et leur contenu consolidées dans les packages de fournisseur principal.

Pour activer `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold` dans EF Core 2.0, vous devez uniquement font référence au package de fournisseur unique :

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
