---
title: Paramètres de fichier de configuration - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 88c2439f3a89c9fb9ee22f828789df4decf39cc5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996501"
---
# <a name="configuration-file-settings"></a>Fichier de configuration
Entity Framework permet un certain nombre de paramètres de la définir à partir du fichier de configuration. En général EF suit un principe de « convention sur configuration » : tous les paramètres décrits dans ce billet un comportement par défaut, vous devez uniquement à vous soucier de la modification du paramètre lorsque la valeur par défaut ne répond plus aux besoins de votre.  

## <a name="a-code-based-alternative"></a>Une Alternative basée sur le Code  

Tous ces paramètres peuvent également être appliqué à l’aide de code. À compter de EF6, nous avons introduit [configuration basée sur le code](code-based.md), qui fournit un moyen centralisé de l’application de configuration à partir du code. Avant d’EF6, configuration peut toujours être appliquée à partir du code, mais vous devez utiliser les diverses API pour configurer les différentes zones. L’option de configuration permet à ces paramètres pour être facilement modifiées pendant le déploiement sans mettre à jour votre code.

## <a name="the-entity-framework-configuration-section"></a>La Section de Configuration d’Entity Framework  

En commençant par EF4.1, vous pouvez définir l’initialiseur de base de données pour un contexte à l’aide de la **appSettings** section du fichier de configuration. Dans EF 4.3, nous avons introduit personnalisé **entityFramework** section pour gérer les nouveaux paramètres. Entity Framework reconnaît toujours les initialiseurs de base de données définis à l’aide de l’ancien format, mais nous vous recommandons de passer au nouveau format lorsque cela est possible.

Le **entityFramework** section a été ajoutée automatiquement au fichier de configuration de votre projet lorsque vous avez installé le package EntityFramework NuGet.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Chaînes de connexion  

[Cette page](~/ef6/fundamentals/configuring/connection-strings.md) fournit plus de détails sur la façon dont Entity Framework détermine la base de données à utiliser, y compris les chaînes de connexion dans le fichier de configuration.  

Passez des chaînes de connexion dans la norme **connectionStrings** élément et ne nécessitent pas la **entityFramework** section.  

Les modèles de code First basé utilisent des chaînes de connexion ADO.NET normales. Exemple :  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Entity Framework Designer en fonction des modèles utilisation spéciale EF les chaînes de connexion. Exemple :  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Configuration basée sur le code de Type (Entity Framework 6 et versions ultérieures)  

À compter de EF6, vous pouvez spécifier la DbConfiguration pour Entity Framework à utiliser pour [configuration basée sur le code](code-based.md) dans votre application. Dans la plupart des cas vous n’avez pas besoin de spécifier ce paramètre comme Entity Framework détecte automatiquement votre DbConfiguration. Pour plus d’informations de lorsque vous devrez peut-être spécifier DbConfiguration dans votre fichier de configuration, consultez le **DbConfiguration déplacement** section de [Configuration basée sur le Code](code-based.md).  

Pour définir un type DbConfiguration, vous spécifiez le nom de type qualifié d’assembly dans le **codeConfigurationType** élément.  

> [!NOTE]
> Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside. Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Fournisseurs de base de données Entity Framework (Entity Framework 6 et versions ultérieures)  

Avant d’EF6, les parties spécifiques à Entity Framework d’un fournisseur de base de données devaient être inclus dans le cadre du fournisseur ADO.NET core. En commençant avec EF6, les parties spécifiques d’EF sont désormais gérés et inscrit séparément.  

Normalement, vous ne devrez inscrire des fournisseurs vous-même. Ce est généralement effectué par le fournisseur lorsque vous l’installez.  

Fournisseurs sont inscrits en incluant un **fournisseur** élément sous le **fournisseurs** section enfant de la **entityFramework** section. Il existe deux attributs requis pour une entrée de fournisseur :  

- **invariantName** identifie le fournisseur ADO.NET core que ces cibles de fournisseur Entity Framework  
- **type** est le nom de type qualifié d’assembly de l’implémentation du fournisseur Entity Framework  

> [!NOTE]
> Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside. Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.  

Par exemple ici est l’entrée créée pour inscrire le fournisseur de SQL Server par défaut lorsque vous installez Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Intercepteurs (EF6.1 et versions ultérieures)  

En commençant par EF6.1, vous pouvez inscrire des intercepteurs dans le fichier de configuration. Les intercepteurs vous autorise à exécuter une logique supplémentaire quand EF effectue certaines opérations, telles que l’exécution de requêtes de base de données, d’ouverture de connexions, etc.  

Intercepteurs sont inscrits en incluant un **intercepteur** élément sous le **intercepteurs** section enfant de la **entityFramework** section. Par exemple, la configuration suivante inscrit intégrés **DatabaseLogger** intercepteur qui enregistre toutes les opérations de base de données dans la Console.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Opérations de base de données de journalisation dans un fichier (EF6.1 et versions ultérieures)  

L’inscription d’intercepteurs via le fichier de configuration est particulièrement utile lorsque vous souhaitez ajouter un enregistrement à une application existante pour aider à déboguer un problème. **DatabaseLogger** prend en charge de la journalisation dans un fichier en fournissant le nom de fichier en tant que paramètre de constructeur.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Par défaut, cela entraîne le fichier journal doivent être remplacées par un nouveau fichier chaque fois que l’application démarre. Pour ajouter à la place dans le journal de fichier s’il existe déjà utiliser quelque chose comme :  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Pour plus d’informations sur **DatabaseLogger** et l’inscription d’intercepteurs, voir le blog [EF 6.1 : activer la journalisation sans avoir à recompiler](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Code première fabrique de connexion par défaut  

La section de configuration vous permet de spécifier une fabrique de connexion par défaut que Code First doit utiliser pour localiser une base de données à utiliser pour un contexte. La fabrique de connexion par défaut est utilisée uniquement lorsqu’aucune chaîne de connexion n’a été ajouté au fichier de configuration pour un contexte.  

Lorsque vous avez installé le package NuGet d’EF qui pointe vers SQL Express ou LocalDB, selon celle qui vous avez installé a été inscrit par une fabrique de connexion par défaut.  

Pour définir une fabrique de connexion, vous spécifiez le nom de type qualifié d’assembly dans le **deafultConnectionFactory** élément.  

> [!NOTE]
> Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside. Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.  

Voici un exemple de définition de votre propre fabrique de connexion par défaut :  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

L’exemple ci-dessus nécessite la fabrique personnalisée pour avoir un constructeur sans paramètre. Si nécessaire, vous pouvez spécifier les paramètres du constructeur à l’aide de la **paramètres** élément.  

Par exemple, SqlCeConnectionFactory, ce qui est inclus dans Entity Framework, vous oblige à fournir un nom invariant du fournisseur au constructeur. Nom invariant du fournisseur identifie la version de SQL Compact que vous souhaitez utiliser. La configuration suivante entraîne des contextes à utiliser la version SQL Compact 4.0 par défaut.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Si vous ne définissez pas une fabrique de connexion par défaut, Code First utilise le SqlConnectionFactory, pointant vers `.\SQLEXPRESS`. SqlConnectionFactory possède également un constructeur qui vous permet de remplacer des parties de la chaîne de connexion. Si vous souhaitez utiliser une instance de SQL Server autre que `.\SQLEXPRESS` vous pouvez utiliser ce constructeur pour définir le serveur.  

La configuration suivante entraîne le premier Code à utiliser **MonServeurBaseDeDonnées** pour les contextes qui ne sont pas une chaîne de connexion explicite défini.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Par défaut, il est supposé que les arguments de constructeur sont de type chaîne. Vous pouvez utiliser l’attribut de type pour y remédier.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Initialiseurs de base de données  

Initialiseurs de base de données sont configurés sur une base par contexte. Elles peuvent être définies dans le fichier de configuration à l’aide de la **contexte** élément. Cet élément utilise le nom qualifié d’assembly pour identifier le contexte en cours de configuration.  

Par défaut, Code First contextes sont configurés pour utiliser l’initialiseur CreateDatabaseIfNotExists. Il existe un **disableDatabaseInitialization** d’attribut sur le **contexte** élément qui peut être utilisé pour désactiver l’initialisation de base de données.  

Par exemple, la configuration suivante désactive l’initialisation de base de données pour le contexte de Blogging.BlogContext défini dans MyAssembly.dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Vous pouvez utiliser la **databaseInitializer** élément à définir un initialiseur personnalisé.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Paramètres du constructeur utilisent la même syntaxe que les fabriques de connexion par défaut.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Vous pouvez configurer une des initialiseurs de base de données générique qui sont incluses dans Entity Framework. Le **type** attribut utilise le format de .NET Framework pour les types génériques.  

Par exemple, si vous utilisez des Migrations Code First, vous pouvez configurer la base de données pour être migrés automatiquement à l’aide de la `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initialiseur.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
