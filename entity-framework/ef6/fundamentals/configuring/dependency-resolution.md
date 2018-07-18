---
title: Résolution des dépendances - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121482"
---
# <a name="dependency-resolution"></a><span data-ttu-id="bcb7a-102">Résolution des dépendances</span><span class="sxs-lookup"><span data-stu-id="bcb7a-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="bcb7a-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="bcb7a-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="bcb7a-105">À compter de EF6, Entity Framework contient un mécanisme à usage général pour obtenir les implémentations des services dont il a besoin.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="bcb7a-106">Autrement dit, quand EF utilise une instance de certaines interfaces ou les classes de base il demandera une implémentation concrète de la classe d’interface ou de base à utiliser.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="bcb7a-107">Cela s’effectue via l’utilisation de l’interface IDbDependencyResolver :</span><span class="sxs-lookup"><span data-stu-id="bcb7a-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="bcb7a-108">La méthode GetService est généralement appelée par Entity Framework et est gérée par une implémentation de IDbDependencyResolver fourni par Entity Framework ou par l’application.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="bcb7a-109">Lorsqu’elle est appelée, l’argument de type est le type de classe interface ou de base du service demandé, et l’objet de clé est null ou un objet qui fournit des informations contextuelles sur le service demandé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="bcb7a-110">Cet article ne contient-elle pas tous les détails d’implémentation IDbDependencyResolver, mais au lieu de cela agit comme une référence pour les types de service (autrement dit, l’interface et la base de types de classe) pour lequel EF appelle GetService et la sémantique de l’objet clé pour chacun d’eux appels.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="bcb7a-111">Ce document sera mis à jour comme des services supplémentaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="bcb7a-112">Services résolus</span><span class="sxs-lookup"><span data-stu-id="bcb7a-112">Services resolved</span></span>  

<span data-ttu-id="bcb7a-113">Sauf mention contraire, n’importe quel objet retourné doit être thread-safe, car il peut être utilisé comme un singleton.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="bcb7a-114">Dans de nombreux cas, que l’objet retourné est une fabrique dans ce cas, l’usine elle-même doit être thread-safe, mais l’objet retourné par la fabrique est inutile être thread-safe, car une nouvelle instance est demandée à partir de la fabrique pour chaque utilisation.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="bcb7a-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="bcb7a-116">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-117">**Objet retourné**: un initialiseur de base de données pour le type de contexte donné</span><span class="sxs-lookup"><span data-stu-id="bcb7a-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="bcb7a-118">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="bcb7a-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="bcb7a-120">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="bcb7a-121">**Objet retourné**: une fabrique pour créer un générateur SQL qui peut être utilisé pour les Migrations et les autres actions qui entraînent une base de données doit être créée, telle que la création de base de données avec des initialiseurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="bcb7a-122">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle générer la requête SQL.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="bcb7a-123">Par exemple, le Générateur SQL de SQL Server est retourné pour la clé « System.Data.SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="bcb7a-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-124">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="bcb7a-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="bcb7a-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="bcb7a-126">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-127">**Objet retourné**: fournisseur de l’Entity Framework à utiliser pour un nom invariant de fournisseur donné</span><span class="sxs-lookup"><span data-stu-id="bcb7a-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="bcb7a-128">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle un fournisseur est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="bcb7a-129">Par exemple, le fournisseur SQL Server est retourné pour la clé « System.Data.SqlClient ».</span><span class="sxs-lookup"><span data-stu-id="bcb7a-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-130">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="bcb7a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="bcb7a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="bcb7a-132">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-133">**Objet retourné**: la fabrique de connexions qui sera utilisée lors de l’Entity Framework crée une connexion de base de données par convention.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="bcb7a-134">Autrement dit, lorsque aucune connexion ou une chaîne de connexion correspond à EF, et aucune chaîne de connexion ne peut être trouvé dans le fichier app.config ou web.config, ce service est utilisé pour créer une connexion par convention.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="bcb7a-135">Modification de la fabrique de connexion peut autoriser EF d’utiliser un autre type de base de données (par exemple, SQL Server Compact Edition) par défaut.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="bcb7a-136">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-137">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="bcb7a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="bcb7a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="bcb7a-139">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-140">**Objet retourné**: un service qui peut générer un jeton du manifeste du fournisseur à partir d’une connexion.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="bcb7a-141">Ce service est généralement utilisé de deux manières.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-141">This service is typically used in two ways.</span></span> <span data-ttu-id="bcb7a-142">Tout d’abord, il peut être utilisé afin d’éviter un Code First, connexion à la base de données lors de la création d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="bcb7a-143">En second lieu, il peut être utilisé pour forcer le Code First pour générer un modèle pour une version de base de données spécifique, par exemple, pour forcer un modèle pour SQL Server 2005, même si SQL Server 2008 est parfois utilisé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="bcb7a-144">**Durée de vie**: Singleton--le même objet peut être utilisé plusieurs fois simultanément par différents threads</span><span class="sxs-lookup"><span data-stu-id="bcb7a-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="bcb7a-145">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="bcb7a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="bcb7a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="bcb7a-147">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-148">**Objet retourné**: un service qui peut obtenir une fabrique de fournisseurs à partir d’une connexion donnée.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="bcb7a-149">Sur le .NET 4.5, le fournisseur est publiquement accessible à partir de la connexion.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="bcb7a-150">Sur .NET 4 l’implémentation par défaut de ce service utilise quelques méthodes heuristiques pour trouver le fournisseur correspondant.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="bcb7a-151">Si ces cas de défaillance une nouvelle implémentation de ce service peuvent être inscrits pour fournir une résolution appropriées.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="bcb7a-152">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="bcb7a-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="bcb7a-154">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-155">**Objet retourné**: une fabrique qui génère une clé de cache de modèle pour un contexte donné.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="bcb7a-156">Par défaut, Entity Framework met en cache un modèle par type de DbContext par le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="bcb7a-157">Une implémentation différente de ce service peut être utilisée pour ajouter d’autres informations, telles que nom de schéma, à la clé de cache.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="bcb7a-158">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="bcb7a-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="bcb7a-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="bcb7a-160">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-161">**Objet retourné**: fournisseur spatial EF une qui ajoute la prise en charge pour le fournisseur EF de base pour les types geography et geometry spatiaux.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="bcb7a-162">**Clé**: DbSptialServices est demandé de deux manières.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="bcb7a-163">Tout d’abord, spécifique au fournisseur de services spatiaux sont demandés à l’aide d’un objet DbProviderInfo (qui contient l’invariant jeton de nom et le manifest) comme clé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="bcb7a-164">En second lieu, DbSpatialServices peuvent être demandées sans clé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="bcb7a-165">Cela permet de résoudre le « spatial mondial » qui est utilisé lors de la création des types de DbGeography ou DbGeometry autonomes.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-166">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="bcb7a-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="bcb7a-168">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-169">**Objet retourné**: une fabrique pour créer un service qui permet à un fournisseur implémenter les nouvelles tentatives ou un autre comportement lorsque les requêtes et les commandes sont exécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="bcb7a-170">Si aucune implémentation n’est fournie, puis EF sera simplement exécuter les commandes et propager toutes les exceptions levées.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="bcb7a-171">Pour SQL Server ce service est utilisé pour fournir une stratégie de nouvelle tentative, ce qui est particulièrement utile lors de l’exécution par rapport à des serveurs de base de données basée sur le cloud, tels que SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="bcb7a-172">**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel la stratégie d’exécution est utilisée.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-173">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="bcb7a-174">Func < DbConnection, chaîne, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="bcb7a-175">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-176">**Objet retourné**: une fabrique qui permet à un fournisseur configurer le mappage de la HistoryContext à la `__MigrationHistory` table utilisée par des Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="bcb7a-177">Le HistoryContext est un Code premier DbContext et peut être configuré à l’aide de l’API fluent normale pour modifier des éléments tels que le nom de la table et les spécifications de mappage de colonne.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="bcb7a-178">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-179">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="bcb7a-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="bcb7a-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="bcb7a-181">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-182">**Objet retourné**: fournisseur de l’ADO.NET à utiliser pour un nom invariant de fournisseur donné.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="bcb7a-183">**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET</span><span class="sxs-lookup"><span data-stu-id="bcb7a-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-184">Ce service n’est généralement pas modifié directement dans la mesure où l’implémentation par défaut utilise l’inscription du fournisseur ADO.NET normale.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="bcb7a-185">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="bcb7a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="bcb7a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="bcb7a-187">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-188">**Objet retourné**: un service qui est utilisé pour déterminer un nom invariant du fournisseur pour un type donné de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="bcb7a-189">L’implémentation par défaut de ce service utilise l’inscription du fournisseur ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="bcb7a-190">Cela signifie que si le fournisseur ADO.NET n’est pas inscrit de façon normale, car DbProviderFactory est résolue par Entity Framework, puis il sera également nécessaire pour résoudre ce service.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="bcb7a-191">**Clé**: instance de la DbProviderFactory pour lequel un nom invariant est requis.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="bcb7a-192">Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="bcb7a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="bcb7a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="bcb7a-194">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-195">**Objet retourné**: un cache des assemblys qui contiennent des vues prégénérées.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="bcb7a-196">Un remplacement est généralement utilisé pour informer EF les assemblys qui contiennent des vues prégénérées sans effectuer n’importe quel découverte.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="bcb7a-197">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="bcb7a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="bcb7a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="bcb7a-199">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-200">**Objet retourné**: un service utilisé par EF au pluriel et au singulier les noms.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="bcb7a-201">Par défaut, un service de pluralisation anglais est utilisé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="bcb7a-202">**Clé**: non utilisé ; sera null</span><span class="sxs-lookup"><span data-stu-id="bcb7a-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="bcb7a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="bcb7a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="bcb7a-204">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="bcb7a-205">**Objets retournés**: des intercepteurs qui doivent être inscrite au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="bcb7a-206">Notez que ces objets sont demandées à l’aide de l’appel GetServices et tous les intercepteurs retournés par n’importe quel programme de résolution de dépendance seront inscrit.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="bcb7a-207">**Clé**: non utilisé ; sera null.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="bcb7a-208">Func < System.Data.Entity.DbContext, Action < chaîne\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="bcb7a-209">**Version introduite**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bcb7a-210">**Objet retourné**: une fabrique qui sera utilisée pour créer le formateur de journal de base de données qui sera utilisée lorsque le contexte. Propriété de Database.Log est définie sur le contexte donné.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="bcb7a-211">**Clé**: non utilisé ; sera null.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="bcb7a-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="bcb7a-213">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bcb7a-214">**Objet retourné**: une fabrique qui sera utilisée pour créer des instances de contexte pour les Migrations lorsque le contexte n’a pas un constructeur sans paramètre accessible.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="bcb7a-215">**Clé**: objet de Type pour le type de DbContext dérivé pour laquelle une fabrique est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="bcb7a-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="bcb7a-217">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bcb7a-218">**Objet retourné**: une fabrique qui sera utilisée pour créer des sérialiseurs pour la sérialisation de fortement typée des annotations personnalisées telles que s’ils peuvent être sérialisés ou desterilized en XML pour une utilisation dans les Migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="bcb7a-219">**Clé**: le nom de l’annotation est sérialisée ou désérialisée.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="bcb7a-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="bcb7a-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="bcb7a-221">**Version introduite**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bcb7a-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bcb7a-222">**Objet retourné**: une fabrique qui sera utilisée pour créer des gestionnaires pour les transactions afin qu’un traitement spécial peut être appliqué aux situations telles que la gestion des échecs de validation.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="bcb7a-223">**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel le Gestionnaire de transaction sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="bcb7a-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
