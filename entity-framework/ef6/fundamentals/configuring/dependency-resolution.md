---
title: Résolution des dépendances - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998160"
---
# <a name="dependency-resolution"></a>Résolution des dépendances
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

À compter de EF6, Entity Framework contient un mécanisme à usage général pour obtenir les implémentations des services dont il a besoin. Autrement dit, quand EF utilise une instance de certaines interfaces ou les classes de base il demandera une implémentation concrète de la classe d’interface ou de base à utiliser. Cela s’effectue via l’utilisation de l’interface IDbDependencyResolver :  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

La méthode GetService est généralement appelée par Entity Framework et est gérée par une implémentation de IDbDependencyResolver fourni par Entity Framework ou par l’application. Lorsqu’elle est appelée, l’argument de type est le type de classe interface ou de base du service demandé, et l’objet de clé est null ou un objet qui fournit des informations contextuelles sur le service demandé.  

Cet article ne contient-elle pas tous les détails d’implémentation IDbDependencyResolver, mais au lieu de cela agit comme une référence pour les types de service (autrement dit, l’interface et la base de types de classe) pour lequel EF appelle GetService et la sémantique de l’objet clé pour chacun d’eux appels. Ce document sera mis à jour comme des services supplémentaires sont ajoutés.  

## <a name="services-resolved"></a>Services résolus  

Sauf mention contraire, n’importe quel objet retourné doit être thread-safe, car il peut être utilisé comme un singleton. Dans de nombreux cas, que l’objet retourné est une fabrique dans ce cas, l’usine elle-même doit être thread-safe, mais l’objet retourné par la fabrique est inutile être thread-safe, car une nouvelle instance est demandée à partir de la fabrique pour chaque utilisation.  

### <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Version introduite**: EF6.0.0  

**Objet retourné**: un initialiseur de base de données pour le type de contexte donné  

**Clé**: non utilisé ; sera null  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Version introduite**: EF6.0.0

**Objet retourné**: une fabrique pour créer un générateur SQL qui peut être utilisé pour les Migrations et les autres actions qui entraînent une base de données doit être créée, telle que la création de base de données avec des initialiseurs de base de données.  

**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle générer la requête SQL. Par exemple, le Générateur SQL de SQL Server est retourné pour la clé « System.Data.SqlClient ».  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Version introduite**: EF6.0.0  

**Objet retourné**: fournisseur de l’Entity Framework à utiliser pour un nom invariant de fournisseur donné  

**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET en spécifiant le type de base de données pour laquelle un fournisseur est nécessaire. Par exemple, le fournisseur SQL Server est retourné pour la clé « System.Data.SqlClient ».  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Version introduite**: EF6.0.0  

**Objet retourné**: la fabrique de connexions qui sera utilisée lors de l’Entity Framework crée une connexion de base de données par convention. Autrement dit, lorsque aucune connexion ou une chaîne de connexion correspond à EF, et aucune chaîne de connexion ne peut être trouvé dans le fichier app.config ou web.config, ce service est utilisé pour créer une connexion par convention. Modification de la fabrique de connexion peut autoriser EF d’utiliser un autre type de base de données (par exemple, SQL Server Compact Edition) par défaut.  

**Clé**: non utilisé ; sera null  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Version introduite**: EF6.0.0  

**Objet retourné**: un service qui peut générer un jeton du manifeste du fournisseur à partir d’une connexion. Ce service est généralement utilisé de deux manières. Tout d’abord, il peut être utilisé afin d’éviter un Code First, connexion à la base de données lors de la création d’un modèle. En second lieu, il peut être utilisé pour forcer le Code First pour générer un modèle pour une version de base de données spécifique, par exemple, pour forcer un modèle pour SQL Server 2005, même si SQL Server 2008 est parfois utilisé.  

**Durée de vie**: Singleton--le même objet peut être utilisé plusieurs fois simultanément par différents threads  

**Clé**: non utilisé ; sera null  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Version introduite**: EF6.0.0  

**Objet retourné**: un service qui peut obtenir une fabrique de fournisseurs à partir d’une connexion donnée. Sur le .NET 4.5, le fournisseur est publiquement accessible à partir de la connexion. Sur .NET 4 l’implémentation par défaut de ce service utilise quelques méthodes heuristiques pour trouver le fournisseur correspondant. Si ces cas de défaillance une nouvelle implémentation de ce service peuvent être inscrits pour fournir une résolution appropriées.  

**Clé**: non utilisé ; sera null  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Version introduite**: EF6.0.0  

**Objet retourné**: une fabrique qui génère une clé de cache de modèle pour un contexte donné. Par défaut, Entity Framework met en cache un modèle par type de DbContext par le fournisseur. Une implémentation différente de ce service peut être utilisée pour ajouter d’autres informations, telles que nom de schéma, à la clé de cache.  

**Clé**: non utilisé ; sera null  

### <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Version introduite**: EF6.0.0  

**Objet retourné**: fournisseur spatial EF une qui ajoute la prise en charge pour le fournisseur EF de base pour les types geography et geometry spatiaux.  

**Clé**: DbSptialServices est demandé de deux manières. Tout d’abord, spécifique au fournisseur de services spatiaux sont demandés à l’aide d’un objet DbProviderInfo (qui contient l’invariant jeton de nom et le manifest) comme clé. En second lieu, DbSpatialServices peuvent être demandées sans clé. Cela permet de résoudre le « spatial mondial » qui est utilisé lors de la création des types de DbGeography ou DbGeometry autonomes.  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Version introduite**: EF6.0.0  

**Objet retourné**: une fabrique pour créer un service qui permet à un fournisseur implémenter les nouvelles tentatives ou un autre comportement lorsque les requêtes et les commandes sont exécutées sur la base de données. Si aucune implémentation n’est fournie, puis EF sera simplement exécuter les commandes et propager toutes les exceptions levées. Pour SQL Server ce service est utilisé pour fournir une stratégie de nouvelle tentative, ce qui est particulièrement utile lors de l’exécution par rapport à des serveurs de base de données basée sur le cloud, tels que SQL Azure.  

**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel la stratégie d’exécution est utilisée.  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, chaîne, System.Data.Entity.Migrations.History.HistoryContext\>  

**Version introduite**: EF6.0.0  

**Objet retourné**: une fabrique qui permet à un fournisseur configurer le mappage de la HistoryContext à la `__MigrationHistory` table utilisée par des Migrations Entity Framework. Le HistoryContext est un Code premier DbContext et peut être configuré à l’aide de l’API fluent normale pour modifier des éléments tels que le nom de la table et les spécifications de mappage de colonne.  

**Clé**: non utilisé ; sera null  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Version introduite**: EF6.0.0  

**Objet retourné**: fournisseur de l’ADO.NET à utiliser pour un nom invariant de fournisseur donné.  

**Clé**: une chaîne contenant le nom invariant du fournisseur ADO.NET  

>[!NOTE]
> Ce service n’est généralement pas modifié directement dans la mesure où l’implémentation par défaut utilise l’inscription du fournisseur ADO.NET normale. Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Version introduite**: EF6.0.0  

**Objet retourné**: un service qui est utilisé pour déterminer un nom invariant du fournisseur pour un type donné de DbProviderFactory. L’implémentation par défaut de ce service utilise l’inscription du fournisseur ADO.NET. Cela signifie que si le fournisseur ADO.NET n’est pas inscrit de façon normale, car DbProviderFactory est résolue par Entity Framework, puis il sera également nécessaire pour résoudre ce service.  

**Clé**: instance de la DbProviderFactory pour lequel un nom invariant est requis.  

>[!NOTE]
> Pour plus d’informations sur des services de fournisseur dans EF6, consultez le [modèle de fournisseur EF6](~/ef6/fundamentals/providers/provider-model.md) section.  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Version introduite**: EF6.0.0  

**Objet retourné**: un cache des assemblys qui contiennent des vues prégénérées. Un remplacement est généralement utilisé pour informer EF les assemblys qui contiennent des vues prégénérées sans effectuer n’importe quel découverte.  

**Clé**: non utilisé ; sera null  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Version introduite**: EF6.0.0  

**Objet retourné**: un service utilisé par EF au pluriel et au singulier les noms. Par défaut, un service de pluralisation anglais est utilisé.  

**Clé**: non utilisé ; sera null  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Version introduite**: EF6.0.0

**Objets retournés**: des intercepteurs qui doivent être inscrite au démarrage de l’application. Notez que ces objets sont demandées à l’aide de l’appel GetServices et tous les intercepteurs retournés par n’importe quel programme de résolution de dépendance seront inscrit.

**Clé**: non utilisé ; sera null.  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, Action < chaîne\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Version introduite**: EF6.0.0  

**Objet retourné**: une fabrique qui sera utilisée pour créer le formateur de journal de base de données qui sera utilisée lorsque le contexte. Propriété de Database.Log est définie sur le contexte donné.  

**Clé**: non utilisé ; sera null.  

### <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**Version introduite**: EF6.1.0  

**Objet retourné**: une fabrique qui sera utilisée pour créer des instances de contexte pour les Migrations lorsque le contexte n’a pas un constructeur sans paramètre accessible.  

**Clé**: objet de Type pour le type de DbContext dérivé pour laquelle une fabrique est nécessaire.  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Version introduite**: EF6.1.0  

**Objet retourné**: une fabrique qui sera utilisée pour créer des sérialiseurs pour la sérialisation de fortement typée des annotations personnalisées telles que s’ils peuvent être sérialisés ou desterilized en XML pour une utilisation dans les Migrations Code First.  

**Clé**: le nom de l’annotation est sérialisée ou désérialisée.  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Version introduite**: EF6.1.0  

**Objet retourné**: une fabrique qui sera utilisée pour créer des gestionnaires pour les transactions afin qu’un traitement spécial peut être appliqué aux situations telles que la gestion des échecs de validation.  

**Clé**: ExecutionStrategyKey un objet qui contient le nom invariant du fournisseur et éventuellement un nom de serveur pour lequel le Gestionnaire de transaction sera utilisé.  
