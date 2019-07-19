---
title: Outils et extensions - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 08231cd93002a6d1b3cebe20f4f7cf57ea085af2
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306403"
---
# <a name="ef-core-tools--extensions"></a>Outils et extensions EF Core

Ces outils et extensions fournissent des fonctionnalités supplémentaires pour Entity Framework Core 2.0 et ultérieur.

> [!IMPORTANT]  
> Les extensions sont générées par des sources diverses et ne sont pas gérées dans le cadre du projet Entity Framework Core. Quand vous envisagez une extension tierce, veillez à évaluer ses caractéristiques, notamment en termes de qualité, de gestion des licences, de compatibilité et de prise en charge, pour vérifier qu’elle répond à vos besoins.

## <a name="tools"></a>Outils

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro est une solution de modélisation d’entités qui prend en charge Entity Framework et Entity Framework Core. Il vous permet de définir facilement votre modèle d’entités et de le mapper à votre base de données, en utilisant Database First ou Model First : vous pouvez donc commencer à écrire des requêtes immédiatement.

[Site web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer est un concepteur ORM pour ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access et LINQ to SQL. Il prend en charge la conception visuelle de modèles EF Core, selon une approche Model First ou Database First, et la génération de code C# ou Visual Basic. 

[Site web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools est une extension Visual Studio 2017 qui expose différentes tâches EF Core au moment du design dans une interface utilisateur simple. Elle inclut l’ingénierie à rebours des classes DbContext et d’entité à partir de bases de données existantes et des [packages DAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), la gestion des migrations de base de données et les visualisations de modèles.

[Wiki GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Entity Framework Visual Editor est une extension de Visual Studio qui ajoute un concepteur ORM permettant de concevoir visuellement des classes EF 6 et EF Core. Le code étant généré à l’aide de modèles T4, il peut être personnalisé pour répondre à tous les besoins. Il prend en charge les associations d’héritage, unidirectionnelles et bidirectionnelles, les énumérations ainsi que la possibilité de colorer le code de vos classes et d’ajouter des blocs de texte pour expliquer les parties potentiellement obscures de votre conception.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory est un moteur de génération de modèles automatique pour .NET Core qui peut automatiser la génération de classes DbContext, d’entités, de configurations de mappage et de classes de dépôts à partir d’une base de données SQL Server.

[Dépôt GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Générateur Entity Framework Core de LoreSoft

Entity Framework Core Generator (efg) est un outil CLI .NET Core qui peut générer des modèles EF Core à partir d’une base de données existante, comme `dotnet ef dbcontext scaffold`, mais qui prend également en charge la [regénération](https://efg.loresoft.com/en/latest/regeneration/) de code safe à travers le remplacement de région ou l’analyse des fichiers de mappage. Cet outil prend en charge la génération de code de mappeur d’objet, de validation et de modèles de vue. 

[Tutoriel](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Extensions

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Bibliothèque de plug-ins qui permet l’enregistrement automatique des changements de données effectués par EF Core dans une table d’historique.

[Dépôt GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Port .NET Core/.NET Standard de System.Linq.Dynamic qui inclut la prise en charge asynchrone avec EF Core.
System.Linq.Dynamic provient d’un exemple Microsoft qui montre comment construire dynamiquement des requêtes LINQ à partir d’expressions de chaîne plutôt qu’à partir de code.

[Dépôt GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Extension qui permet de stocker les résultats de requêtes EF Core dans un cache de second niveau afin que les exécutions ultérieures des mêmes requêtes puissent récupérer les données directement à partir du cache sans avoir à accéder à la base de données.

[Dépôt GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Cette bibliothèque permet de récupérer les valeurs de la clé primaire (y compris les clés composites) à partir de n’importe quelle entité en tant que dictionnaire.

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Cette bibliothèque permet un accès fortement typé aux valeurs d’origine des propriétés d’entité. 

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Generator Console) est un générateur de code simple basé sur un projet de console qui s’exécute sur .NET Core et qui utilise des chaînes interpolées C# pour la génération de code. Geco inclut un générateur de modèle inverse pour EF Core avec prise en charge de la pluralisation, de la singularisation et des modèles modifiables. Il fournit également un générateur de script de données de départ, un exécuteur de scripts et un nettoyeur de base de données.

[Dépôt GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore est une version compatible avec EF Core de la bibliothèque LINQKit. LINQKit est un ensemble gratuit d’extensions à l’attention des utilisateurs avancés LINQ to SQL et Entity Framework. Il prend en charge des fonctionnalités avancées telles que la génération d’expressions de prédicat et l’utilisation de variables d’expression dans les sous-requêtes.  

[Dépôt GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq étend les fonctionnalités des fournisseurs LINQ comme Entity Framework pour permettre la réutilisation de fonctions, la réécriture des requêtes et la génération de requêtes dynamiques à l’aide de sélecteurs et de prédicats traduisibles.

[Dépôt GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Plug-in pour Microsoft.EntityFrameworkCore prenant en charge un dépôt, les modèles d’unités de travail et les bases de données multiples avec des transactions distribuées.

[Dépôt GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensions EF Core pour les opérations en bloc (Insert, Update, Delete).

[Dépôt GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Ajoute la pluralisation au moment du design sur EF Core.

[Dépôt GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Méthode d’extension simple qui obtient l’instruction SQL générée par EF Core pour une requête LINQ donnée dans des scénarios simples. La méthode ToSql est limitée aux scénarios simples, car EF Core peut générer plusieurs instructions SQL pour une seule requête LINQ et différentes instructions SQL en fonction des valeurs de paramètre.

[Dépôt GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Reprise de l’attribut [Index] pour EF Core (avec l’extension pour la génération de modèles).

[Dépôt GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Fournit un wrapper autour du fournisseur de base de données In-Memory EF Core. Il fonctionne alors plus comme un fournisseur relationnel.

[Dépôt GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implémentation de la prise en charge temporelle pour EF Core.

[Dépôt GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Cache des requêtes de second niveau hautes performances pour EF Core.

[Dépôt GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Étend votre DbContext avec des fonctionnalités telles que : Include Filter (Inclure le filtre), Auditing (Audit), Caching (Mise en cache), Query Future (Requête ultérieure), Batch Delete (Suppression par lot), Batch Update (Mise à jour par lot), et bien plus encore.

[Site web](https://entityframework-plus.net/)
[Référentiel GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Extensions d’Entity Framework

Étend votre DbContext avec des opérations en bloc hautes performances : BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, et bien plus encore.

[Site web](https://entityframework-extensions.net/)
