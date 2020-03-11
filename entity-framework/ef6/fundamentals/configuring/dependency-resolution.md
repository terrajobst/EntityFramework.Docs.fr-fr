---
title: Résolution des dépendances-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417861"
---
# <a name="dependency-resolution"></a><span data-ttu-id="8bc8c-102">Résolution des dépendances</span><span class="sxs-lookup"><span data-stu-id="8bc8c-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="8bc8c-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8bc8c-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8bc8c-105">À compter de EF6, Entity Framework contient un mécanisme à usage général pour obtenir les implémentations de services dont il a besoin.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="8bc8c-106">Autrement dit, lorsque EF utilise une instance de certaines interfaces ou classes de base, il demande une implémentation concrète de l’interface ou de la classe de base à utiliser.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="8bc8c-107">Cela est possible grâce à l’utilisation de l’interface IDbDependencyResolver :</span><span class="sxs-lookup"><span data-stu-id="8bc8c-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="8bc8c-108">La méthode GetService est généralement appelée par EF et est gérée par une implémentation de IDbDependencyResolver fournie par EF ou par l’application.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="8bc8c-109">Quand elle est appelée, l’argument de type est l’interface ou le type de classe de base du service demandé, et l’objet clé est null ou un objet fournissant des informations contextuelles sur le service demandé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="8bc8c-110">Sauf indication contraire, un objet retourné doit être thread-safe, car il peut être utilisé en tant que singleton.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="8bc8c-111">Dans de nombreux cas, l’objet retourné est une fabrique, auquel cas la fabrique elle-même doit être thread-safe, mais l’objet retourné par la fabrique n’a pas besoin d’être thread-safe, car une nouvelle instance est demandée à partir de la fabrique pour chaque utilisation.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="8bc8c-112">Cet article ne contient pas de détails complets sur la façon d’implémenter IDbDependencyResolver, mais sert à la place de référence pour les types de services (autrement dit, les types d’interface et de classe de base) pour lesquels EF appelle GetService et la sémantique de l’objet clé pour chacun de ces invite.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="8bc8c-113">System. Data. Entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="8bc8c-114">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-115">**Objet retourné**: un initialiseur de base de données pour le type de contexte donné</span><span class="sxs-lookup"><span data-stu-id="8bc8c-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="8bc8c-116">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="8bc8c-117">Func < System. Data. Entity. migrations. Sql. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="8bc8c-118">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="8bc8c-119">**Objet retourné**: fabrique pour créer un générateur SQL qui peut être utilisé pour les migrations et d’autres actions qui provoquent la création d’une base de données, telle que la création d’une base de données avec des initialiseurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="8bc8c-120">**Key**: chaîne contenant le nom invariant du fournisseur ADO.NET spécifiant le type de base de données pour lequel SQL sera généré.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="8bc8c-121">Par exemple, le SQL Server SQL Generator est retourné pour la clé « System. Data. SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="8bc8c-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-122">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="8bc8c-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="8bc8c-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="8bc8c-124">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-125">**Objet retourné**: fournisseur EF à utiliser pour un nom invariant de fournisseur donné</span><span class="sxs-lookup"><span data-stu-id="8bc8c-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="8bc8c-126">**Key**: chaîne contenant le nom invariant du fournisseur ADO.NET spécifiant le type de base de données pour lequel un fournisseur est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="8bc8c-127">Par exemple, le fournisseur SQL Server est retourné pour la clé « System. Data. SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="8bc8c-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-128">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="8bc8c-129">System. Data. Entity. infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="8bc8c-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="8bc8c-130">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-131">**Objet retourné**: fabrique de connexion qui sera utilisée quand EF crée une connexion de base de données par Convention.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="8bc8c-132">Autrement dit, si aucune chaîne de connexion ou de connexion n’est donnée à EF, et qu’aucune chaîne de connexion ne peut être trouvée dans le fichier app. config ou Web. config, ce service est utilisé pour créer une connexion par Convention.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="8bc8c-133">La modification de la fabrique de connexion peut permettre à EF d’utiliser un autre type de base de données (par exemple, SQL Server Compact Edition) par défaut.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="8bc8c-134">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-135">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="8bc8c-136">System. Data. Entity. infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="8bc8c-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="8bc8c-137">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-138">**Objet retourné**: service qui peut générer un jeton de manifeste du fournisseur à partir d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="8bc8c-139">Ce service est généralement utilisé de deux manières.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-139">This service is typically used in two ways.</span></span> <span data-ttu-id="8bc8c-140">Tout d’abord, il peut être utilisé pour éviter Code First la connexion à la base de données lors de la génération d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="8bc8c-141">Deuxièmement, il peut être utilisé pour forcer Code First à générer un modèle pour une version de base de données spécifique, par exemple, pour forcer un modèle pour SQL Server 2005 même si, parfois SQL Server 2008 est utilisé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="8bc8c-142">**Durée de vie**d’un objet : Singleton--le même objet peut être utilisé plusieurs fois et simultanément par différents threads</span><span class="sxs-lookup"><span data-stu-id="8bc8c-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="8bc8c-143">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="8bc8c-144">System. Data. Entity. infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="8bc8c-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="8bc8c-145">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-146">**Objet retourné**: service qui peut obtenir une fabrique de fournisseurs à partir d’une connexion donnée.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="8bc8c-147">Sur .NET 4,5, le fournisseur est accessible publiquement à partir de la connexion.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="8bc8c-148">Sur .NET 4, l’implémentation par défaut de ce service utilise des heuristiques pour rechercher le fournisseur correspondant.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="8bc8c-149">En cas d’échec, une nouvelle implémentation de ce service peut être inscrite pour fournir une résolution appropriée.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="8bc8c-150">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="8bc8c-151">Func < DbContext, System. Data. Entity. infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="8bc8c-152">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-153">**Objet retourné**: fabrique qui génère une clé de cache de modèle pour un contexte donné.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="8bc8c-154">Par défaut, EF met en cache un modèle par type DbContext par fournisseur.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="8bc8c-155">Une implémentation différente de ce service peut être utilisée pour ajouter d’autres informations, telles que le nom de schéma, à la clé de cache.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="8bc8c-156">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="8bc8c-157">System. Data. Entity. spatial. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="8bc8c-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="8bc8c-158">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-159">**Objet retourné**: un fournisseur spatial EF qui ajoute la prise en charge du fournisseur EF de base pour les types spatiaux Geography et Geometry.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="8bc8c-160">**Clé**: DbSptialServices est demandé pour deux manières.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="8bc8c-161">Tout d’abord, les services spatiaux spécifiques au fournisseur sont demandés à l’aide d’un objet DbProviderInfo (qui contient le nom invariant et le jeton de manifeste) comme clé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="8bc8c-162">Deuxièmement, DbSpatialServices peut être demandé sans clé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="8bc8c-163">Utilisé pour résoudre le « fournisseur spatial global » qui est utilisé lors de la création de types DbGeography ou DbGeometry autonomes.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-164">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="8bc8c-165">Func < System. Data. Entity. infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="8bc8c-166">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-167">**Objet retourné**: fabrique pour créer un service qui permet à un fournisseur d’implémenter de nouvelles tentatives ou un autre comportement lorsque des requêtes et des commandes sont exécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="8bc8c-168">Si aucune implémentation n’est fournie, EF exécute simplement les commandes et propage toutes les exceptions levées.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="8bc8c-169">Par SQL Server ce service est utilisé pour fournir une stratégie de nouvelle tentative qui est particulièrement utile lors de l’exécution sur des serveurs de base de données basés sur le Cloud, tels que des SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="8bc8c-170">**Clé**: objet ExecutionStrategyKey qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel la stratégie d’exécution sera utilisée.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-171">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="8bc8c-172">Func < DbConnection, String, System. Data. Entity. migrations. History. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="8bc8c-173">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-174">**Objet retourné**: fabrique qui permet à un fournisseur de configurer le mappage du HistoryContext à la table `__MigrationHistory` utilisée par les migrations EF.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="8bc8c-175">Le HistoryContext est un Code First DbContext et peut être configuré à l’aide de l’API Fluent normale pour modifier des éléments tels que le nom de la table et les spécifications de mappage de colonne.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="8bc8c-176">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-177">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="8bc8c-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="8bc8c-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="8bc8c-179">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-180">**Objet retourné**: fournisseur ADO.net à utiliser pour un nom invariant de fournisseur donné.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="8bc8c-181">**Key**: chaîne contenant le nom invariant du fournisseur ADO.net</span><span class="sxs-lookup"><span data-stu-id="8bc8c-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-182">Ce service n’est généralement pas modifié directement puisque l’implémentation par défaut utilise l’inscription normale du fournisseur ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="8bc8c-183">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="8bc8c-184">System. Data. Entity. infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="8bc8c-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="8bc8c-185">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-186">**Objet retourné**: service utilisé pour déterminer un nom invariant de fournisseur pour un type donné de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="8bc8c-187">L’implémentation par défaut de ce service utilise l’inscription du fournisseur ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="8bc8c-188">Cela signifie que si le fournisseur ADO.NET n’est pas enregistré de manière normale, car DbProviderFactory est résolu par EF, il est également nécessaire de résoudre ce service.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="8bc8c-189">**Clé**: l’instance DbProviderFactory pour laquelle un nom invariant est requis.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="8bc8c-190">Pour plus d’informations sur les services liés aux fournisseurs dans EF6, consultez la section relative au [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="8bc8c-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="8bc8c-191">System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="8bc8c-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="8bc8c-192">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-193">**Objet retourné**: cache des assemblys qui contiennent des vues prégénérées.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="8bc8c-194">Un remplacement est généralement utilisé pour permettre à EF de savoir quels assemblys contiennent des vues prégénérées sans effectuer de détection.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="8bc8c-195">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="8bc8c-196">System. Data. Entity. infrastructure. pluralisation. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="8bc8c-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="8bc8c-197">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-198">**Objet retourné**: service utilisé par EF pour pluralismiser et singulierer des noms.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="8bc8c-199">Par défaut, un service de pluralisme anglais est utilisé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="8bc8c-200">**Clé**: non utilisée ; aura la valeur null</span><span class="sxs-lookup"><span data-stu-id="8bc8c-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="8bc8c-201">System. Data. Entity. infrastructure. interception. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="8bc8c-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="8bc8c-202">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="8bc8c-203">**Objets retournés**: tous les intercepteurs qui doivent être inscrits au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="8bc8c-204">Notez que ces objets sont demandés à l’aide de l’appel GetServices et que tous les intercepteurs retournés par un programme de résolution de dépendance sont inscrits.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="8bc8c-205">**Clé**: non utilisée ; aura la valeur null.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="8bc8c-206">Func < System. Data. Entity. DbContext, action < chaîne\>, System. Data. Entity. infrastructure. interception. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="8bc8c-207">**Version introduite**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8bc8c-208">**Objet retourné**: fabrique qui sera utilisée pour créer le formateur de journal de base de données qui sera utilisé lorsque le contexte. La propriété Database. log est définie sur le contexte donné.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="8bc8c-209">**Clé**: non utilisée ; aura la valeur null.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="8bc8c-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="8bc8c-211">**Version introduite**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8bc8c-212">**Objet retourné**: fabrique qui sera utilisée pour créer des instances de contexte pour les migrations lorsque le contexte n’a pas de constructeur sans paramètre accessible.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="8bc8c-213">**Clé**: objet de type pour le type du DbContext dérivé pour lequel une fabrique est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="8bc8c-214">Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="8bc8c-215">**Version introduite**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8bc8c-216">**Objet retourné**: fabrique qui sera utilisée pour créer des sérialiseurs pour la sérialisation d’annotations personnalisées fortement typées de sorte qu’elles puissent être sérialisées et déstérilisées en XML en vue de leur utilisation dans migrations code First.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="8bc8c-217">**Clé**: nom de l’annotation en cours de sérialisation ou de désérialisation.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="8bc8c-218">Func < System. Data. Entity. infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="8bc8c-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="8bc8c-219">**Version introduite**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8bc8c-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8bc8c-220">**Objet retourné**: fabrique qui sera utilisée pour créer des gestionnaires pour les transactions afin qu’une gestion spéciale puisse être appliquée pour des situations telles que la gestion des échecs de validation.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="8bc8c-221">**Clé**: objet ExecutionStrategyKey qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel le gestionnaire de transactions sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="8bc8c-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
