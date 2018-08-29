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
# <a name="configuration-file-settings"></a><span data-ttu-id="44bdb-102">Fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="44bdb-102">Configuration File Settings</span></span>
<span data-ttu-id="44bdb-103">Entity Framework permet un certain nombre de paramètres de la définir à partir du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="44bdb-104">En général EF suit un principe de « convention sur configuration » : tous les paramètres décrits dans ce billet un comportement par défaut, vous devez uniquement à vous soucier de la modification du paramètre lorsque la valeur par défaut ne répond plus aux besoins de votre.</span><span class="sxs-lookup"><span data-stu-id="44bdb-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="44bdb-105">Une Alternative basée sur le Code</span><span class="sxs-lookup"><span data-stu-id="44bdb-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="44bdb-106">Tous ces paramètres peuvent également être appliqué à l’aide de code.</span><span class="sxs-lookup"><span data-stu-id="44bdb-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="44bdb-107">À compter de EF6, nous avons introduit [configuration basée sur le code](code-based.md), qui fournit un moyen centralisé de l’application de configuration à partir du code.</span><span class="sxs-lookup"><span data-stu-id="44bdb-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="44bdb-108">Avant d’EF6, configuration peut toujours être appliquée à partir du code, mais vous devez utiliser les diverses API pour configurer les différentes zones.</span><span class="sxs-lookup"><span data-stu-id="44bdb-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="44bdb-109">L’option de configuration permet à ces paramètres pour être facilement modifiées pendant le déploiement sans mettre à jour votre code.</span><span class="sxs-lookup"><span data-stu-id="44bdb-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="44bdb-110">La Section de Configuration d’Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44bdb-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="44bdb-111">En commençant par EF4.1, vous pouvez définir l’initialiseur de base de données pour un contexte à l’aide de la **appSettings** section du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="44bdb-112">Dans EF 4.3, nous avons introduit personnalisé **entityFramework** section pour gérer les nouveaux paramètres.</span><span class="sxs-lookup"><span data-stu-id="44bdb-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="44bdb-113">Entity Framework reconnaît toujours les initialiseurs de base de données définis à l’aide de l’ancien format, mais nous vous recommandons de passer au nouveau format lorsque cela est possible.</span><span class="sxs-lookup"><span data-stu-id="44bdb-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="44bdb-114">Le **entityFramework** section a été ajoutée automatiquement au fichier de configuration de votre projet lorsque vous avez installé le package EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="44bdb-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="44bdb-115">Chaînes de connexion</span><span class="sxs-lookup"><span data-stu-id="44bdb-115">Connection Strings</span></span>  

<span data-ttu-id="44bdb-116">[Cette page](~/ef6/fundamentals/configuring/connection-strings.md) fournit plus de détails sur la façon dont Entity Framework détermine la base de données à utiliser, y compris les chaînes de connexion dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="44bdb-117">Passez des chaînes de connexion dans la norme **connectionStrings** élément et ne nécessitent pas la **entityFramework** section.</span><span class="sxs-lookup"><span data-stu-id="44bdb-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="44bdb-118">Les modèles de code First basé utilisent des chaînes de connexion ADO.NET normales.</span><span class="sxs-lookup"><span data-stu-id="44bdb-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="44bdb-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="44bdb-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="44bdb-120">Entity Framework Designer en fonction des modèles utilisation spéciale EF les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="44bdb-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="44bdb-121">Exemple :</span><span class="sxs-lookup"><span data-stu-id="44bdb-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="44bdb-122">Configuration basée sur le code de Type (Entity Framework 6 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="44bdb-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="44bdb-123">À compter de EF6, vous pouvez spécifier la DbConfiguration pour Entity Framework à utiliser pour [configuration basée sur le code](code-based.md) dans votre application.</span><span class="sxs-lookup"><span data-stu-id="44bdb-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="44bdb-124">Dans la plupart des cas vous n’avez pas besoin de spécifier ce paramètre comme Entity Framework détecte automatiquement votre DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="44bdb-125">Pour plus d’informations de lorsque vous devrez peut-être spécifier DbConfiguration dans votre fichier de configuration, consultez le **DbConfiguration déplacement** section de [Configuration basée sur le Code](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="44bdb-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="44bdb-126">Pour définir un type DbConfiguration, vous spécifiez le nom de type qualifié d’assembly dans le **codeConfigurationType** élément.</span><span class="sxs-lookup"><span data-stu-id="44bdb-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="44bdb-127">Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside.</span><span class="sxs-lookup"><span data-stu-id="44bdb-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="44bdb-128">Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.</span><span class="sxs-lookup"><span data-stu-id="44bdb-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="44bdb-129">Fournisseurs de base de données Entity Framework (Entity Framework 6 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="44bdb-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="44bdb-130">Avant d’EF6, les parties spécifiques à Entity Framework d’un fournisseur de base de données devaient être inclus dans le cadre du fournisseur ADO.NET core.</span><span class="sxs-lookup"><span data-stu-id="44bdb-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="44bdb-131">En commençant avec EF6, les parties spécifiques d’EF sont désormais gérés et inscrit séparément.</span><span class="sxs-lookup"><span data-stu-id="44bdb-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="44bdb-132">Normalement, vous ne devrez inscrire des fournisseurs vous-même.</span><span class="sxs-lookup"><span data-stu-id="44bdb-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="44bdb-133">Ce est généralement effectué par le fournisseur lorsque vous l’installez.</span><span class="sxs-lookup"><span data-stu-id="44bdb-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="44bdb-134">Fournisseurs sont inscrits en incluant un **fournisseur** élément sous le **fournisseurs** section enfant de la **entityFramework** section.</span><span class="sxs-lookup"><span data-stu-id="44bdb-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="44bdb-135">Il existe deux attributs requis pour une entrée de fournisseur :</span><span class="sxs-lookup"><span data-stu-id="44bdb-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="44bdb-136">**invariantName** identifie le fournisseur ADO.NET core que ces cibles de fournisseur Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44bdb-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="44bdb-137">**type** est le nom de type qualifié d’assembly de l’implémentation du fournisseur Entity Framework</span><span class="sxs-lookup"><span data-stu-id="44bdb-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="44bdb-138">Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside.</span><span class="sxs-lookup"><span data-stu-id="44bdb-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="44bdb-139">Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.</span><span class="sxs-lookup"><span data-stu-id="44bdb-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="44bdb-140">Par exemple ici est l’entrée créée pour inscrire le fournisseur de SQL Server par défaut lorsque vous installez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="44bdb-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="44bdb-141">Intercepteurs (EF6.1 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="44bdb-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="44bdb-142">En commençant par EF6.1, vous pouvez inscrire des intercepteurs dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="44bdb-143">Les intercepteurs vous autorise à exécuter une logique supplémentaire quand EF effectue certaines opérations, telles que l’exécution de requêtes de base de données, d’ouverture de connexions, etc.</span><span class="sxs-lookup"><span data-stu-id="44bdb-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="44bdb-144">Intercepteurs sont inscrits en incluant un **intercepteur** élément sous le **intercepteurs** section enfant de la **entityFramework** section.</span><span class="sxs-lookup"><span data-stu-id="44bdb-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="44bdb-145">Par exemple, la configuration suivante inscrit intégrés **DatabaseLogger** intercepteur qui enregistre toutes les opérations de base de données dans la Console.</span><span class="sxs-lookup"><span data-stu-id="44bdb-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="44bdb-146">Opérations de base de données de journalisation dans un fichier (EF6.1 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="44bdb-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="44bdb-147">L’inscription d’intercepteurs via le fichier de configuration est particulièrement utile lorsque vous souhaitez ajouter un enregistrement à une application existante pour aider à déboguer un problème.</span><span class="sxs-lookup"><span data-stu-id="44bdb-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="44bdb-148">**DatabaseLogger** prend en charge de la journalisation dans un fichier en fournissant le nom de fichier en tant que paramètre de constructeur.</span><span class="sxs-lookup"><span data-stu-id="44bdb-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="44bdb-149">Par défaut, cela entraîne le fichier journal doivent être remplacées par un nouveau fichier chaque fois que l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="44bdb-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="44bdb-150">Pour ajouter à la place dans le journal de fichier s’il existe déjà utiliser quelque chose comme :</span><span class="sxs-lookup"><span data-stu-id="44bdb-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="44bdb-151">Pour plus d’informations sur **DatabaseLogger** et l’inscription d’intercepteurs, voir le blog [EF 6.1 : activer la journalisation sans avoir à recompiler](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="44bdb-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="44bdb-152">Code première fabrique de connexion par défaut</span><span class="sxs-lookup"><span data-stu-id="44bdb-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="44bdb-153">La section de configuration vous permet de spécifier une fabrique de connexion par défaut que Code First doit utiliser pour localiser une base de données à utiliser pour un contexte.</span><span class="sxs-lookup"><span data-stu-id="44bdb-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="44bdb-154">La fabrique de connexion par défaut est utilisée uniquement lorsqu’aucune chaîne de connexion n’a été ajouté au fichier de configuration pour un contexte.</span><span class="sxs-lookup"><span data-stu-id="44bdb-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="44bdb-155">Lorsque vous avez installé le package NuGet d’EF qui pointe vers SQL Express ou LocalDB, selon celle qui vous avez installé a été inscrit par une fabrique de connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="44bdb-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="44bdb-156">Pour définir une fabrique de connexion, vous spécifiez le nom de type qualifié d’assembly dans le **deafultConnectionFactory** élément.</span><span class="sxs-lookup"><span data-stu-id="44bdb-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="44bdb-157">Un nom qualifié d’assembly est le nom qualifié d’espace de noms, suivi par une virgule, puis l’assembly du type dans lequel réside.</span><span class="sxs-lookup"><span data-stu-id="44bdb-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="44bdb-158">Vous pouvez éventuellement spécifier également la version de l’assembly, la culture et le jeton de clé publique.</span><span class="sxs-lookup"><span data-stu-id="44bdb-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="44bdb-159">Voici un exemple de définition de votre propre fabrique de connexion par défaut :</span><span class="sxs-lookup"><span data-stu-id="44bdb-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="44bdb-160">L’exemple ci-dessus nécessite la fabrique personnalisée pour avoir un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="44bdb-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="44bdb-161">Si nécessaire, vous pouvez spécifier les paramètres du constructeur à l’aide de la **paramètres** élément.</span><span class="sxs-lookup"><span data-stu-id="44bdb-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="44bdb-162">Par exemple, SqlCeConnectionFactory, ce qui est inclus dans Entity Framework, vous oblige à fournir un nom invariant du fournisseur au constructeur.</span><span class="sxs-lookup"><span data-stu-id="44bdb-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="44bdb-163">Nom invariant du fournisseur identifie la version de SQL Compact que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="44bdb-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="44bdb-164">La configuration suivante entraîne des contextes à utiliser la version SQL Compact 4.0 par défaut.</span><span class="sxs-lookup"><span data-stu-id="44bdb-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="44bdb-165">Si vous ne définissez pas une fabrique de connexion par défaut, Code First utilise le SqlConnectionFactory, pointant vers `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="44bdb-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="44bdb-166">SqlConnectionFactory possède également un constructeur qui vous permet de remplacer des parties de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="44bdb-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="44bdb-167">Si vous souhaitez utiliser une instance de SQL Server autre que `.\SQLEXPRESS` vous pouvez utiliser ce constructeur pour définir le serveur.</span><span class="sxs-lookup"><span data-stu-id="44bdb-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="44bdb-168">La configuration suivante entraîne le premier Code à utiliser **MonServeurBaseDeDonnées** pour les contextes qui ne sont pas une chaîne de connexion explicite défini.</span><span class="sxs-lookup"><span data-stu-id="44bdb-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="44bdb-169">Par défaut, il est supposé que les arguments de constructeur sont de type chaîne.</span><span class="sxs-lookup"><span data-stu-id="44bdb-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="44bdb-170">Vous pouvez utiliser l’attribut de type pour y remédier.</span><span class="sxs-lookup"><span data-stu-id="44bdb-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="44bdb-171">Initialiseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="44bdb-171">Database Initializers</span></span>  

<span data-ttu-id="44bdb-172">Initialiseurs de base de données sont configurés sur une base par contexte.</span><span class="sxs-lookup"><span data-stu-id="44bdb-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="44bdb-173">Elles peuvent être définies dans le fichier de configuration à l’aide de la **contexte** élément.</span><span class="sxs-lookup"><span data-stu-id="44bdb-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="44bdb-174">Cet élément utilise le nom qualifié d’assembly pour identifier le contexte en cours de configuration.</span><span class="sxs-lookup"><span data-stu-id="44bdb-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="44bdb-175">Par défaut, Code First contextes sont configurés pour utiliser l’initialiseur CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="44bdb-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="44bdb-176">Il existe un **disableDatabaseInitialization** d’attribut sur le **contexte** élément qui peut être utilisé pour désactiver l’initialisation de base de données.</span><span class="sxs-lookup"><span data-stu-id="44bdb-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="44bdb-177">Par exemple, la configuration suivante désactive l’initialisation de base de données pour le contexte de Blogging.BlogContext défini dans MyAssembly.dll.</span><span class="sxs-lookup"><span data-stu-id="44bdb-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="44bdb-178">Vous pouvez utiliser la **databaseInitializer** élément à définir un initialiseur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="44bdb-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="44bdb-179">Paramètres du constructeur utilisent la même syntaxe que les fabriques de connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="44bdb-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="44bdb-180">Vous pouvez configurer une des initialiseurs de base de données générique qui sont incluses dans Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="44bdb-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="44bdb-181">Le **type** attribut utilise le format de .NET Framework pour les types génériques.</span><span class="sxs-lookup"><span data-stu-id="44bdb-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="44bdb-182">Par exemple, si vous utilisez des Migrations Code First, vous pouvez configurer la base de données pour être migrés automatiquement à l’aide de la `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initialiseur.</span><span class="sxs-lookup"><span data-stu-id="44bdb-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
