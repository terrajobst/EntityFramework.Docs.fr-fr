---
title: Le modèle de fournisseur Entity Framework 6 - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: d07a8689fe968bb1512095a59a61abc7ac346a31
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022322"
---
# <a name="the-entity-framework-6-provider-model"></a>Le modèle de fournisseur Entity Framework 6

Le modèle de fournisseur Entity Framework permet à Entity Framework à utiliser avec différents types de serveur de base de données. Par exemple, un seul fournisseur peut être branché pour permettre d’EF être exploitée dans Microsoft SQL Server, tandis qu’un autre fournisseur peut être raccordé à pour permettre d’EF être exploitée dans Microsoft SQL Server Compact Edition. Vous trouverez les fournisseurs pour EF6 que nous sommes conscients de sur le [fournisseurs d’Entity Framework](~/ef6/fundamentals/providers/index.md) page.

Certaines modifications étaient nécessaires à la façon dont EF interagit avec les fournisseurs d’Entity Framework pour être commercialisées sous une licence open source. Ces modifications exigent une reconstruction de fournisseurs EF sur les assemblys EF6 ainsi que de nouveaux mécanismes pour l’inscription du fournisseur.

## <a name="rebuilding"></a>La reconstruction

Avec EF6 code principal qui faisait précédemment partie du .NET Framework est désormais fourni en tant que les assemblys hors-bande (OOB). Vous trouverez plus d’informations sur la façon de créer des applications dans EF6 sur le [mise à jour des applications pour EF6](~/ef6/what-is-new/upgrading-to-ef6.md) page. Fournisseurs devez également être reconstruits à l’aide de ces instructions.

## <a name="provider-types-overview"></a>Vue d’ensemble des types de fournisseur

Un fournisseur EF est vraiment une collection de services spécifiques au fournisseur définis par les types CLR que ces services étendre à partir de (pour une classe de base) ou implémentent (pour une interface). Deux de ces services sont fondamentaux et nécessaire pour Entity Framework de fonctionner tout. D’autres sont facultatifs et uniquement besoin d’être implémentée si une fonctionnalité spécifique est requise et/ou les implémentations par défaut de ces services ne fonctionne pas pour le serveur de base de données spécifique qui est ciblé.

## <a name="fundamental-provider-types"></a>Types de fournisseurs fondamentaux

### <a name="dbproviderfactory"></a>DbProviderFactory

EF dépend d’un type dérivé [System.Data.Common.DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) pour l’exécution de tous les accès de bas niveau de la base de données. DbProviderFactory ne faisant pas partie d’EF mais est plutôt une classe dans le .NET Framework qui fait Office de point d’entrée pour les fournisseurs ADO.NET qui peut être utilisée par Entity Framework, autres O/RMs, ou directement par une application pour obtenir des instances de connexions, les commandes, les paramètres et autres abstractions ADO.NET dans un fournisseur de façon indépendante. Plus d’informations sur DbProviderFactory un se trouve dans le [documentation MSDN pour ADO.NET](https://msdn.microsoft.com/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

EF dépend d’avoir un type dérivé DbProviderServices de fournir des fonctionnalités supplémentaires requises par Entity Framework en haut de la fonctionnalité déjà fournie par le fournisseur ADO.NET. Dans les versions antérieures d’EF la classe DbProviderServices faisait partie du .NET Framework et a été trouvée dans l’espace de noms System.Data.Common. EF6 compter de cette classe est désormais partie de l’EntityFramework.dll et est dans l’espace de noms System.Data.Entity.Core.Common.

Vous trouverez plus d’informations sur la fonctionnalité fondamentale d’une implémentation DbProviderServices sur [MSDN](https://msdn.microsoft.com/library/ee789835.aspx). Toutefois, notez qu’au moment de l’écriture de ces informations n'est pas mis à jour pour EF6 bien que la plupart des concepts est toujours valide. Les implémentations de SQL Server et SQL Server Compact de DbProviderServices sont également archivées dans à la [codebase open source](https://github.com/aspnet/EntityFramework6/) et peuvent servir de référence utile pour d’autres implémentations.

Dans les versions antérieures d’EF implémentation DbProviderServices à utiliser a été obtenue directement à partir d’un fournisseur ADO.NET. Cela a été effectuée en effectuant un cast de DbProviderFactory de IServiceProvider en appelant la méthode GetService. Cela étroitement le fournisseur EF DbProviderFactory. Ce couplage bloqué EF enlève le .NET Framework et pour EF6 ce couplage étroit a donc été supprimé et une implémentation de DbProviderServices est maintenant inscrit directement dans le fichier de configuration de l’application ou dans la base de code configuration comme décrit plus en détail la _DbProviderServices inscription_ section ci-dessous.

## <a name="additional-services"></a>Services supplémentaires

En plus des services fondamentaux décrits ci-dessus, il existe également beaucoup d’autres services utilisés par Entity Framework qui est systématiquement ou parfois spécifiques au fournisseur. Les implémentations spécifiques au fournisseur par défaut de ces services peuvent être fournies par une implémentation DbProviderServices. Les applications peuvent également remplacer les implémentations de ces services ou fournir des implémentations lorsqu’un type DbProviderServices ne fournit pas de valeur par défaut. Cette opération est décrite plus en détail dans le _des services supplémentaires de résolution_ section ci-dessous.

Les types de service supplémentaires un fournisseur peut-être être utiles à un fournisseur sont répertoriées ci-dessous. Vous trouverez plus d’informations sur chacun de ces types de service dans la documentation de l’API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Il s’agit d’un service facultatif qui permet à un fournisseur implémenter les nouvelles tentatives ou un autre comportement lorsque les requêtes et les commandes sont exécutées sur la base de données. Si aucune implémentation n’est fournie, puis EF sera simplement exécuter les commandes et propager toutes les exceptions levées. Pour SQL Server ce service est utilisé pour fournir une stratégie de nouvelle tentative, ce qui est particulièrement utile lors de l’exécution par rapport à des serveurs de base de données basée sur le cloud, tels que SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Il s’agit d’un service facultatif qui permet à un fournisseur créer des objets de DbConnection par convention donné uniquement un nom de la base de données. Notez que bien que ce service peut être résolu par une implémentation DbProviderServices il était présente depuis EF 4.1 et peut également être explicitement définie dans le fichier de configuration ou dans le code. Le fournisseur obtient uniquement la possibilité de résoudre ce service s’il est inscrit en tant que le fournisseur par défaut (consultez _le fournisseur par défaut_ ci-dessous) et si une fabrique de connexion par défaut n’est pas définie ailleurs.

### <a name="dbspatialservices"></a>DbSpatialServices

Il s’agit d’un services facultatif qui permet à un fournisseur à ajouter la prise en charge pour les types geography et geometry spatiaux. Une implémentation de ce service doit être fournie pour une application à utiliser EF avec des types spatiaux. DbSptialServices est invité à entrer de deux manières. Tout d’abord, spécifique au fournisseur de services spatiaux sont demandés à l’aide d’un objet DbProviderInfo (qui contient l’invariant jeton de nom et le manifest) en tant que clé. En second lieu, DbSpatialServices peuvent être demandées sans clé. Cela permet de résoudre le « spatial mondial » qui est utilisé lors de la création des types de DbGeography ou DbGeometry autonomes.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Il s’agit d’un service facultatif qui permet des Migrations Entity Framework à utiliser pour la génération de la session SQL utilisée dans la création et modification des schémas de base de données par Code First. Une implémentation est requise pour prendre en charge les Migrations. Si une implémentation est fournie, il sera également utiliser lors de la création de bases de données à l’aide d’initialiseurs de base de données ou la méthode Database.Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, chaîne, HistoryContextFactory >

Il s’agit d’un service facultatif qui permet à un fournisseur configurer le mappage de la HistoryContext à la `__MigrationHistory` table utilisée par des Migrations Entity Framework. Le HistoryContext est un Code premier DbContext et peut être configuré à l’aide de l’API fluent normale pour modifier des éléments tels que le nom de la table et les spécifications de mappage de colonne. L’implémentation par défaut de ce service retourné par EF pour tous les fournisseurs peut-être fonctionner pour un serveur de base de données donné si tous les mappages table et de colonne par défaut sont pris en charge par le fournisseur. Dans ce cas le fournisseur est inutile de fournir une implémentation de ce service.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Il s’agit d’un service facultatif pour l’obtention du DbProviderFactory correct à partir d’un objet DbConnection donné. L’implémentation par défaut de ce service retourné par EF pour tous les fournisseurs est destinée à fonctionner pour tous les fournisseurs. Toutefois, lors de l’exécution sur .NET 4, DbProviderFactory n’est pas accessible publiquement à partir d’un seul si son DbConnections. Par conséquent, EF utilise quelques méthodes heuristiques pour rechercher les fournisseurs inscrits pour rechercher une correspondance. Il est possible que pour certains fournisseurs ces heuristiques échouera et dans ce cas, le fournisseur doit fournir une nouvelle implémentation.

## <a name="registering-dbproviderservices"></a>L’inscription DbProviderServices

Implémentation DbProviderServices à utiliser peut être inscrits dans l’application à l’aide de la configuration basée sur le code ou de fichier de configuration (app.config ou web.config). Dans les deux cas, l’inscription utilise le nom du fournisseur « indifférent » en tant que clé. Cela permet à plusieurs fournisseurs être enregistrés et utilisés dans une application unique. Le nom invariant est utilisé pour les inscriptions d’EF est la même que le nom invariant utilisé pour les chaînes de l’inscription et connexion de fournisseur ADO.NET. Par exemple, pour SQL Server, le nom invariant « System.Data.SqlClient » est utilisé.

### <a name="config-file-registration"></a>Inscription dans le fichier config

Le type DbProviderServices à utiliser est enregistré comme un élément de fournisseur dans la liste des fournisseurs de la section entityFramework de fichier de configuration de l’application. Exemple :

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

Le _type_ chaîne doit être le nom de type qualifié d’assembly de l’implémentation DbProviderServices à utiliser.

### <a name="code-based-registration"></a>Inscription basée sur le code

En commençant par les fournisseurs de EF6 peut aussi être inscrits à l’aide de code. Ainsi, un fournisseur EF utilisable sans aucune modification au fichier de configuration de l’application. Pour utiliser la configuration basée sur le code une application doit créer une classe DbConfiguration, comme décrit dans la [documentation de configuration basée sur le code](https://msdn.com/data/jj680699). Le constructeur de la classe DbConfiguration doit appeler SetProviderServices pour inscrire le fournisseur Entity Framework. Exemple :

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Résoudre des services supplémentaires

Comme indiqué plus haut dans la _présentation des types de fournisseur_ section, un DbProviderServices classe peut également être utilisée pour résoudre des services supplémentaires. Cela est possible car DbProviderServices implémente IDbDependencyResolver et chaque type DbProviderServices enregistré est ajouté comme un « programme de résolution par défaut ». Le mécanisme de IDbDpendencyResolver est décrite plus en détail dans [résolution des dépendances](~/ef6/fundamentals/configuring/dependency-resolution.md). Toutefois, il n’est pas nécessaire de comprendre tous les concepts dans cette spécification pour résoudre des services supplémentaires dans un fournisseur.

La méthode la plus courante pour un fournisseur résoudre des services supplémentaires consiste à appeler DbProviderServices.AddDependencyResolver pour chaque service dans le constructeur de la classe DbProviderServices. Par exemple, SqlProviderServices (le fournisseur Entity Framework pour SQL Server) a code similaire à celui-ci pour l’initialisation :

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Ce constructeur utilise les classes d’assistance suivantes :

*   SingletonDependencyResolver : fournit un moyen simple pour résoudre des services de Singleton, autrement dit, les services pour lesquels la même instance est retournée chaque fois que GetService est appelée. Services transitoires sont souvent enregistrés comme une fabrique de singleton qui servira à créer des instances temporaires à la demande.
*   ExecutionStrategyResolver : un programme de résolution spécifique pour retourner des implémentations de IExecutionStrategy.

Au lieu d’utiliser DbProviderServices.AddDependencyResolver, il est également possible de remplacer DbProviderServices.GetService et résoudre des services supplémentaires directement. Cette méthode est appelée quand EF a besoin d’un service défini par un certain type et, dans certains cas, pour une clé donnée. La méthode doit retourner le service s’il peut, ou retournez null pour les annulations de retourner le service et permettre à la place une autre classe résoudre le problème. Par exemple, pour résoudre la fabrique de connexion par défaut le code dans GetService peut ressembler à ceci :

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Ordre d’inscription

Lorsque plusieurs implémentations DbProviderServices sont enregistrées dans le fichier de configuration d’une application, ils seront ajoutés en tant que programmes de résolution secondaire dans l’ordre d’apparition. Dans la mesure où les programmes de résolution soient toujours ajoutées à la partie supérieure de la chaîne de programme de résolution secondaire que cela signifie que le fournisseur à la fin de la liste obtiendront une chance à résoudre les dépendances avant les autres. (Cela peut sembler un peu intuitif dans un premier temps, mais il est judicieux si vous imaginez en prenant chaque fournisseur de la liste et leur empilement par-dessus les fournisseurs existants).

Ce classement généralement peu, car la plupart des services de fournisseur sont spécifiques au fournisseur et à la clé par le nom invariant du fournisseur. Toutefois, pour les services qui ne sont pas indexés par nom invariant du fournisseur ou certaines autres spécifique au fournisseur clé que le service sera résolu en fonction de cet ordre. Par exemple, si il n'est pas explicitement défini différemment à un endroit ; sinon, la fabrique de connexion par défaut proviendront à partir du fournisseur au premier plan dans la chaîne.

## <a name="additional-config-file-registrations"></a>Enregistrements de fichier de configuration supplémentaires

Il est possible de s’inscrire explicitement les services de fournisseur supplémentaires décrites ci-dessus directement dans le fichier de configuration d’une application. Lorsque cette opération s’effectue l’inscription dans le fichier de configuration sera utilisé au lieu de quoi que ce soit retourné par la méthode GetService de l’implémentation DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Inscription de la fabrique de connexion par défaut

Démarrage avec EF5 du package EntityFramework NuGet automatiquement inscrit la fabrique de connexion SQL Express ou de la fabrique de connexion de base de données locale dans le fichier de configuration.

Exemple :

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

Le _type_ est le nom de type qualifié d’assembly pour la fabrique de connexion par défaut, qui doit implémenter IDbConnectionFactory.

Il est recommandé qu’un package NuGet du fournisseur défini la fabrique de connexion par défaut de cette manière lors de l’installation. Consultez _les Packages NuGet pour les fournisseurs_ ci-dessous.

## <a name="additional-ef6-provider-changes"></a>Modifications supplémentaires du fournisseur EF6

### <a name="spatial-provider-changes"></a>Modifications de fournisseur spatial

Fournisseurs qui prennent en charge les types de données spatiales doivent maintenant implémenter des méthodes supplémentaires sur les classes dérivées de DbSpatialDataReader :

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Il existe également des nouvelles versions asynchrones des méthodes existantes qui sont recommandées pour être remplacée car les implémentations par défaut délèguent aux méthodes synchrones et par conséquent ne pas exécuter de façon asynchrone :

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Prise en charge native pour Enumerable.Contains

EF6 introduit un nouveau type d’expression, DbInExpression, ce qui a été ajouté pour résoudre les problèmes de performances autour de l’utilisation de Enumerable.Contains dans les requêtes LINQ. La classe DbProviderManifest a une nouvelle méthode virtuelle, SupportsInExpression, qui est appelée par EF pour déterminer si un fournisseur gère le nouveau type d’expression. Pour la compatibilité avec les implémentations de fournisseur existantes, la méthode retourne false. Pour tirer parti de cette amélioration, un fournisseur d’EF6 pouvez ajouter du code pour gérer les DbInExpression et substituer SupportsInExpression pour retourner true. Une instance de DbInExpression peut être créée en appelant la méthode DbExpressionBuilder.In. Une instance de DbInExpression est composée d’une DbExpression, représentant généralement une colonne de table et une liste de DbConstantExpression à tester pour une correspondance.

## <a name="nuget-packages-for-providers"></a>Packages NuGet pour les fournisseurs

Première à proposer un fournisseur d’EF6 consiste à libérer sous forme de package NuGet. À l’aide d’un package NuGet présente les avantages suivants :

*   Il est facile à utiliser NuGet pour ajouter l’inscription du fournisseur de fichier de configuration de l’application
*   Des modifications supplémentaires peuvent porter sur le fichier de configuration pour définir la fabrique de connexion par défaut afin que les connexions établies par convention utilise le fournisseur enregistré
*   NuGet gère l’ajout des redirections de liaison afin que le fournisseur EF6 doit continuer à fonctionner même après la publication d’un nouveau package EF

Un exemple de ceci est le package EntityFramework.SqlServerCompact qui est inclus dans le [open source codebase](http://github.com/aspnet/entityframework6). Ce package fournit un bon modèle pour créer des packages NuGet fournisseur EF.

### <a name="powershell-commands"></a>Commandes PowerShell

Lorsque le package EntityFramework NuGet est installé, il inscrit un module PowerShell qui contient les deux commandes sont très utiles pour les packages de fournisseur :

*   EFProvider ajouter ajoute une nouvelle entité pour le fournisseur dans le fichier de configuration du projet cible et s’assure qu’il est à la fin de la liste des fournisseurs inscrits.
*   EFDefaultConnectionFactory ajouter ajoute ou met à jour l’inscription defaultconnectionfactory que dans le fichier de configuration du projet cible.

Ces deux commandes s’occuper de l’ajout d’une section entityFramework au fichier de configuration et l’ajout d’une collection de fournisseurs si nécessaire.

Il est prévu que ces commandes être appelée à partir du script de NuGet Install.ps1. Par exemple, Install.ps1 pour le fournisseur SQL Compact ressemble à ceci :

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Plus d’informations sur ces commandes peuvent être obtenus à l’aide de get-help dans la fenêtre de Console du Gestionnaire de Package.

## <a name="wrapping-providers"></a>Fournisseurs d’habillage

Un fournisseur de retour à la ligne est un fournisseur EF et/ou ADO.NET qui encapsule un fournisseur existant à l’étendre avec d’autres fonctionnalités telles que le profilage ou le suivi des fonctionnalités. Fournisseurs d’habillage peut être inscrit de façon normale, mais il est souvent plus pratique pour configurer le fournisseur d’habillage lors de l’exécution en interceptant la résolution de liée au fournisseur de services. L’événement statique OnLockingConfiguration sur la classe DbConfiguration peut être utilisé pour ce faire.

OnLockingConfiguration est appelée après que EF a déterminé où toute la configuration EF pour le domaine d’application est obtenus depuis mais avant d’être verrouillé pour une utilisation. Au démarrage de l’application (avant EF est utilisé) l’application doit s’inscrire un gestionnaire d’événements pour cet événement. (Nous envisageons d’ajouter la prise en charge de l’inscription de ce gestionnaire dans le fichier de configuration, mais cela n’est pas encore possible.) Le Gestionnaire d’événements doit ensuite effectuer un appel à ReplaceService pour chaque service qui doit être incluse.  

Par exemple, pour encapsuler IDbConnectionFactory et DbProviderService, un gestionnaire comme celui-ci doit être inscrit :

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Le service qui a été résolu et doit maintenant être encapsulé avec la clé qui a été utilisée pour résoudre le service sont passés au gestionnaire. Le gestionnaire peut encapsuler ce service, puis remplacez le service retourné par la version encapsulée.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Résolution d’un DbProviderFactory avec Entity Framework

DbProviderFactory est un des types de fournisseurs fondamentales nécessaires par Entity Framework, comme décrit dans la _présentation des types de fournisseur_ section ci-dessus. Comme déjà mentionné, il n’est pas un type EF et l’inscription est généralement pas partie de la configuration d’EF, mais à la place l’inscription du fournisseur ADO.NET normale dans le fichier machine.config et/ou le fichier de configuration de l’application.

Malgré cette EF utilise toujours son mécanisme de résolution de dépendance normale lorsque vous recherchez un DbProviderFactory à utiliser. Le résolveur par défaut utilise l’inscription ADO.NET normale dans les fichiers de configuration et c’est généralement transparente. En raison de la résolution de dépendance normale mécanisme est utilisé, mais cela signifie qu’un IDbDependencyResolver peut être utilisé pour résoudre un DbProviderFactory même lorsque l’enregistrement normal de ADO.NET n’a pas été effectuée.

Résolution DbProviderFactory de cette manière a plusieurs conséquences :

*   Une application à l’aide de la configuration basée sur le code peut ajouter des appels dans leur classe DbConfiguration pour inscrire la DbProviderFactory approprié. Cela est particulièrement utile pour les applications qui ne souhaitent pas (ou ne peut pas) rendre tout utiliser de n’importe quelle configuration basée sur le fichier.
*   Le service peut être encapsulé ou remplacé à l’aide de ReplaceService comme décrit dans la _encapsulant des fournisseurs_ section ci-dessus
*   En théorie, une implémentation DbProviderServices pu résoudre un DbProviderFactory.

Le point important à noter concernant effectuant l’une de ces éléments est qu’ils affectent uniquement la recherche de DbProviderFactory par Entity Framework. Tout autre code non-EF peut s’attendent le fournisseur ADO.NET pour être inscrit de façon normale et risquent d’échouer si l’inscription est introuvable. Pour cette raison, il est normalement préférable d’un DbProviderFactory à inscrire dans ADO.NET normalement.

### <a name="related-services"></a>Services associés

Si Entity Framework est utilisé pour résoudre un DbProviderFactory, il doit également résoudre les services IProviderInvariantName et IDbProviderFactoryResolver.

IProviderInvariantName est un service qui est utilisé pour déterminer un nom invariant du fournisseur pour un type donné de DbProviderFactory. L’implémentation par défaut de ce service utilise l’inscription du fournisseur ADO.NET. Cela signifie que si le fournisseur ADO.NET n’est pas inscrit de façon normale, car DbProviderFactory est résolue par Entity Framework, puis il sera également nécessaire pour résoudre ce service. Notez qu’un programme de résolution de ce service est automatiquement ajouté lorsque vous utilisez la méthode DbConfiguration.SetProviderFactory.

Comme décrit dans la _présentation des types de fournisseur_ ci-dessus, le IDbProviderFactoryResolver est utilisé pour obtenir la DbProviderFactory correct à partir d’un objet DbConnection donné. L’implémentation par défaut de ce service lors de l’exécution sur .NET 4 utilise l’inscription du fournisseur ADO.NET. Cela signifie que si le fournisseur ADO.NET n’est pas inscrit de façon normale, car DbProviderFactory est résolue par Entity Framework, puis il sera également nécessaire pour résoudre ce service.
