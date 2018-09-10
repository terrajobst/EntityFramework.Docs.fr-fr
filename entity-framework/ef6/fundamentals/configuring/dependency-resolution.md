---
title: Résolution des dépendances - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: c6c56c3048e17a5c888ffe564e7606abf8b0c4ed
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251243"
---
# <a name="dependency-resolution"></a><span data-ttu-id="fb173-102">Résolution des dépendances</span><span class="sxs-lookup"><span data-stu-id="fb173-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="fb173-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fb173-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fb173-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="fb173-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fb173-105">À compter de EF6, Entity Framework contient un mécanisme à usage général pour obtenir les implémentations des services dont il a besoin.</span><span class="sxs-lookup"><span data-stu-id="fb173-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="fb173-106">Autrement dit, quand EF utilise une instance de certaines interfaces ou les classes de base il demandera une implémentation concrète de la classe d’interface ou de base à utiliser.</span><span class="sxs-lookup"><span data-stu-id="fb173-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="fb173-107">Cela s’effectue via l’utilisation de l’interface IDbDependencyResolver :</span><span class="sxs-lookup"><span data-stu-id="fb173-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="fb173-108">La méthode GetService est généralement appelée par Entity Framework et est gérée par une implémentation de IDbDependencyResolver fourni par Entity Framework ou par l’application.</span><span class="sxs-lookup"><span data-stu-id="fb173-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="fb173-109">Lorsqu’elle est appelée, l’argument de type est le type de classe interface ou de base du service demandé, et l’objet de clé est null ou un objet qui fournit des informations contextuelles sur le service demandé.</span><span class="sxs-lookup"><span data-stu-id="fb173-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="fb173-110">Sauf mention contraire, n’importe quel objet retourné doit être thread-safe, car il peut être utilisé comme un singleton.</span><span class="sxs-lookup"><span data-stu-id="fb173-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="fb173-111">Dans de nombreux cas, que l’objet retourné est une fabrique dans ce cas, l’usine elle-même doit être thread-safe, mais l’objet retourné par la fabrique est inutile être thread-safe, car une nouvelle instance est demandée à partir de la fabrique pour chaque utilisation.</span><span class="sxs-lookup"><span data-stu-id="fb173-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="fb173-112">Cet article ne contient-elle pas tous les détails d’implémentation IDbDependencyResolver, mais au lieu de cela agit comme une référence pour les types de service (autrement dit, l’interface et la base de types de classe) pour lequel EF appelle GetService et la sémantique de l’objet clé pour chacun d’eux appels.</span><span class="sxs-lookup"><span data-stu-id="fb173-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="fb173-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="fb173-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="fb173-114">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-115">**Objet retourné**: un initialiseur de base de données pour le type de contexte donné</span><span class="sxs-lookup"><span data-stu-id="fb173-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="fb173-116">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="fb173-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="fb173-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="fb173-118">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="fb173-119">**Objet retourné**: une fabrique pour créer un générateur SQL qui peut être utilisé pour les Migrations et les autres actions qui entraînent une base de données doit être créée, telle que la création de base de données avec des initialiseurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="fb173-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="fb173-120">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle générer la requête SQL.</span><span class="sxs-lookup"><span data-stu-id="fb173-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="fb173-121">Par exemple, le Générateur SQL de SQL Server est retourné pour la clé « System.Data.SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="fb173-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-122">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="fb173-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="fb173-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="fb173-124">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-125">**Objet retourné**: fournisseur de l’Entity Framework à utiliser pour un nom invariant de fournisseur donné</span><span class="sxs-lookup"><span data-stu-id="fb173-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="fb173-126">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle un fournisseur est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="fb173-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="fb173-127">Par exemple, le fournisseur SQL Server est retourné pour la clé « System.Data.SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="fb173-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-128">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="fb173-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="fb173-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="fb173-130">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-131">**Objet retourné**: la fabrique de connexions qui sera utilisée lors de l’Entity Framework crée une connexion de base de données par convention.</span><span class="sxs-lookup"><span data-stu-id="fb173-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="fb173-132">Autrement dit, lorsque aucune connexion ou une chaîne de connexion correspond à EF, et aucune chaîne de connexion ne peut être trouvé dans le fichier app.config ou web.config, ce service est utilisé pour créer une connexion par convention.</span><span class="sxs-lookup"><span data-stu-id="fb173-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="fb173-133">Modification de la fabrique de connexion peut autoriser EF d’utiliser un autre type de base de données (par exemple, SQL Server Compact Edition) par défaut.</span><span class="sxs-lookup"><span data-stu-id="fb173-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="fb173-134">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-135">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="fb173-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="fb173-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="fb173-137">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-138">**Objet retourné**: un service qui peut générer un jeton du manifeste du fournisseur à partir d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="fb173-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="fb173-139">Ce service est généralement utilisé de deux manières.</span><span class="sxs-lookup"><span data-stu-id="fb173-139">This service is typically used in two ways.</span></span> <span data-ttu-id="fb173-140">Tout d’abord, il peut être utilisé afin d’éviter un Code First, connexion à la base de données lors de la création d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="fb173-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="fb173-141">En second lieu, il peut être utilisé pour forcer le Code First pour générer un modèle pour une version de base de données spécifique, par exemple, pour forcer un modèle pour SQL Server 2005, même si SQL Server 2008 est parfois utilisé.</span><span class="sxs-lookup"><span data-stu-id="fb173-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="fb173-142">**Durée de vie**: Singleton--le même objet peut être utilisé plusieurs fois simultanément par différents threads</span><span class="sxs-lookup"><span data-stu-id="fb173-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="fb173-143">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="fb173-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="fb173-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="fb173-145">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-146">**Objet retourné**: un service qui peut obtenir une fabrique de fournisseurs à partir d’une connexion donnée.</span><span class="sxs-lookup"><span data-stu-id="fb173-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="fb173-147">Sur le .NET 4.5, le fournisseur est publiquement accessible à partir de la connexion.</span><span class="sxs-lookup"><span data-stu-id="fb173-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="fb173-148">Sur .NET 4 l’implémentation par défaut de ce service utilise quelques méthodes heuristiques pour trouver le fournisseur correspondant.</span><span class="sxs-lookup"><span data-stu-id="fb173-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="fb173-149">Si ces cas de défaillance une nouvelle implémentation de ce service peuvent être inscrits pour fournir une résolution appropriées.</span><span class="sxs-lookup"><span data-stu-id="fb173-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="fb173-150">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="fb173-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="fb173-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="fb173-152">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-153">**Objet retourné**: une fabrique qui génère une clé de cache de modèle pour un contexte donné.</span><span class="sxs-lookup"><span data-stu-id="fb173-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="fb173-154">Par défaut, Entity Framework met en cache un modèle par type de DbContext par le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="fb173-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="fb173-155">Une implémentation différente de ce service peut être utilisée pour ajouter d’autres informations, telles que nom de schéma, à la clé de cache.</span><span class="sxs-lookup"><span data-stu-id="fb173-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="fb173-156">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="fb173-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="fb173-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="fb173-158">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-159">**Objet retourné**: fournisseur spatial EF une qui ajoute la prise en charge pour le fournisseur EF de base pour les types geography et geometry spatiaux.</span><span class="sxs-lookup"><span data-stu-id="fb173-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="fb173-160">**Clé**: DbSptialServices est demandé de deux manières.</span><span class="sxs-lookup"><span data-stu-id="fb173-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="fb173-161">Tout d’abord, spécifique au fournisseur de services spatiaux sont demandés à l’aide d’un objet DbProviderInfo (qui contient l’invariant jeton de nom et le manifest) comme clé.</span><span class="sxs-lookup"><span data-stu-id="fb173-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="fb173-162">En second lieu, DbSpatialServices peuvent être demandées sans clé.</span><span class="sxs-lookup"><span data-stu-id="fb173-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="fb173-163">Cela permet de résoudre le « spatial mondial » qui est utilisé lors de la création des types de DbGeography ou DbGeometry autonomes.</span><span class="sxs-lookup"><span data-stu-id="fb173-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-164">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="fb173-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="fb173-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="fb173-166">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-167">**Objet retourné**: une fabrique pour créer un service qui permet à un fournisseur implémenter les nouvelles tentatives ou un autre comportement lorsque les requêtes et les commandes sont exécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="fb173-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="fb173-168">Si aucune implémentation n’est fournie, puis EF sera simplement exécuter les commandes et propager toutes les exceptions levées.</span><span class="sxs-lookup"><span data-stu-id="fb173-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="fb173-169">Pour SQL Server ce service est utilisé pour fournir une stratégie de nouvelle tentative, ce qui est particulièrement utile lors de l’exécution par rapport à des serveurs de base de données basée sur le cloud, tels que SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="fb173-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="fb173-170">**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel la stratégie d’exécution est utilisée.</span><span class="sxs-lookup"><span data-stu-id="fb173-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-171">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="fb173-172">Func < DbConnection, chaîne, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="fb173-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="fb173-173">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-174">**Objet retourné**: une fabrique qui permet à un fournisseur configurer le mappage de la HistoryContext à la `__MigrationHistory` table utilisée par des Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fb173-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="fb173-175">Le HistoryContext est un Code premier DbContext et peut être configuré à l’aide de l’API fluent normale pour modifier des éléments tels que le nom de la table et les spécifications de mappage de colonne.</span><span class="sxs-lookup"><span data-stu-id="fb173-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="fb173-176">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-177">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="fb173-178">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="fb173-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="fb173-179">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-180">**Objet retourné**: fournisseur de l’ADO.NET à utiliser pour un nom invariant de fournisseur donné.</span><span class="sxs-lookup"><span data-stu-id="fb173-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="fb173-181">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET</span><span class="sxs-lookup"><span data-stu-id="fb173-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-182">Ce service n’est généralement pas modifié directement dans la mesure où l’implémentation par défaut utilise l’inscription du fournisseur ADO.NET normale.</span><span class="sxs-lookup"><span data-stu-id="fb173-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="fb173-183">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="fb173-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="fb173-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="fb173-185">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-186">**Objet retourné**: un service qui est utilisé pour déterminer un nom invariant du fournisseur pour un type donné de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="fb173-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="fb173-187">L’implémentation par défaut de ce service utilise l’inscription du fournisseur ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="fb173-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="fb173-188">Cela signifie que si le fournisseur ADO.NET n’est pas inscrit de façon normale, car DbProviderFactory est résolue par Entity Framework, puis il sera également nécessaire pour résoudre ce service.</span><span class="sxs-lookup"><span data-stu-id="fb173-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="fb173-189">**Clé**: instance de la DbProviderFactory pour lequel un nom invariant est requis.</span><span class="sxs-lookup"><span data-stu-id="fb173-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="fb173-190">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="fb173-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="fb173-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="fb173-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="fb173-192">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-193">**Objet retourné**: un cache des assemblys qui contiennent des vues prégénérées.</span><span class="sxs-lookup"><span data-stu-id="fb173-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="fb173-194">Un remplacement est généralement utilisé pour informer EF les assemblys qui contiennent des vues prégénérées sans effectuer n’importe quel découverte.</span><span class="sxs-lookup"><span data-stu-id="fb173-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="fb173-195">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="fb173-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="fb173-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="fb173-197">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-198">**Objet retourné**: un service utilisé par EF au pluriel et au singulier les noms.</span><span class="sxs-lookup"><span data-stu-id="fb173-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="fb173-199">Par défaut, un service de pluralisation anglais est utilisé.</span><span class="sxs-lookup"><span data-stu-id="fb173-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="fb173-200">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="fb173-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="fb173-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="fb173-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="fb173-202">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="fb173-203">**Objets retournés**: des intercepteurs qui doivent être inscrite au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="fb173-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="fb173-204">Notez que ces objets sont demandées à l’aide de l’appel GetServices et tous les intercepteurs retournés par n’importe quel programme de résolution de dépendance seront inscrit.</span><span class="sxs-lookup"><span data-stu-id="fb173-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="fb173-205">**Clé**: non utilisé ; sera null.</span><span class="sxs-lookup"><span data-stu-id="fb173-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="fb173-206">Func < System.Data.Entity.DbContext, Action < chaîne\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="fb173-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="fb173-207">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="fb173-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="fb173-208">**Objet retourné**: une fabrique qui sera utilisée pour créer le formateur de journal de base de données qui sera utilisée lorsque le contexte. Propriété de Database.Log est définie sur le contexte donné.</span><span class="sxs-lookup"><span data-stu-id="fb173-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="fb173-209">**Clé**: non utilisé ; sera null.</span><span class="sxs-lookup"><span data-stu-id="fb173-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="fb173-210">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="fb173-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="fb173-211">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="fb173-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fb173-212">**Objet retourné**: une fabrique qui sera utilisée pour créer des instances de contexte pour les Migrations lorsque le contexte n’a pas un constructeur sans paramètre accessible.</span><span class="sxs-lookup"><span data-stu-id="fb173-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="fb173-213">**Clé**: objet de Type pour le type de DbContext dérivé pour laquelle une fabrique est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="fb173-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="fb173-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="fb173-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="fb173-215">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="fb173-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fb173-216">**Objet retourné**: une fabrique qui sera utilisée pour créer des sérialiseurs pour la sérialisation de fortement typée des annotations personnalisées telles que s’ils peuvent être sérialisés ou desterilized en XML pour une utilisation dans les Migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="fb173-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="fb173-217">**Clé**: le nom de l’annotation est sérialisée ou désérialisée.</span><span class="sxs-lookup"><span data-stu-id="fb173-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="fb173-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="fb173-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="fb173-219">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="fb173-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="fb173-220">**Objet retourné**: une fabrique qui sera utilisée pour créer des gestionnaires pour les transactions afin qu’un traitement spécial peut être appliqué aux situations telles que la gestion des échecs de validation.</span><span class="sxs-lookup"><span data-stu-id="fb173-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="fb173-221">**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel le Gestionnaire de transaction sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="fb173-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
