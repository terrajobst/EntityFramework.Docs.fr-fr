---
title: Outils et extensions - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 1edc7a20f54b2d26f899c93e98dfaf6d62c29f86
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490725"
---
# <a name="ef-core-tools--extensions"></a>Outils et extensions EF Core

Les outils et les extensions fournissent des fonctionnalités supplémentaires pour Entity Framework Core.

> [!IMPORTANT]  
> Les extensions sont générées par différentes sources et ne sont pas gérées dans le cadre du projet Entity Framework Core. Quand vous envisagez une extension tierce, veillez à évaluer notamment la qualité, la gestion des licences, la compatibilité et la prise en charge pour vérifier qu’elle répond à vos besoins.

## <a name="tools"></a>Outils

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro est une solution de modélisation d’entités qui prend en charge Entity Framework et Entity Framework Core. Il vous permet de définir facilement votre modèle d’entités et de le mapper à votre base de données, en utilisant Database First ou Model First : vous pouvez donc commencer à écrire des requêtes immédiatement.

[site web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer est un concepteur ORM pour ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access et LINQ to SQL. Vous pouvez utiliser les approches Model First et Database First pour concevoir votre modèle ORM, et pour générer du code C# ou Visual Basic .NET pour ce modèle. Il introduit de nouvelles approches pour la conception de modèles ORM, accélère la productivité et facilite le développement d’applications de base de données.

[site web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Extensions Visual Studio 2017+. Vous pouvez rétroconcevoir des classes DbContext et POCO à partir d’une base de données existante ou d’un projet de base de données SQL Server, et visualiser et examiner votre DbContext de différentes manières.

[Wiki GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Entity Framework Visual Editor

Extension de Visual Studio 2017 qui ajoute un concepteur ORM permettant de concevoir visuellement des classes Entity Framework 6, Core 2.0 et Core 2.1. Le code est généré à l’aide de modèles T4, donc il peut être totalement personnalisé pour répondre à tous les besoins. Les associations d’héritage, unidirectionnelles et bidirectionnelles sont toutes prises en charge, tout comme les énumérations, la possibilité de colorer le code de vos classes et l’ajout de blocs de texte pour expliquer les parties potentiellement obscures de votre conception.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a>Extensions

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Plug-in pour Microsoft.EntityFrameworkCore qui prend en charge l’enregistrement automatique de l’historique de modification des données.

[Dépôt GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Extensions LINQ dynamiques pour Microsoft.EntityFrameworkCore qui ajoutent la prise en charge asynchrone

 [Dépôt GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Tentative de capture de quelques bonnes pratiques dans une API qui prend en charge les tests, notamment un petit framework pour rechercher les requêtes N+1.

[Dépôt GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Bibliothèque de mise en cache de second niveau. La mise en cache de second niveau est un cache de requêtes. Les résultats des commandes EF sont stockés dans le cache, de façon que les mêmes commandes EF récupèrent leurs données auprès du cache au lieu d’être réexécutées sur la base de données.

[Dépôt GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Charge et enregistre les graphes des entités détachées entières (l’entité avec ses entités enfants et ses listes). Inspiré par [GraphDiff](https://github.com/refactorthis/GraphDiff/). L’idée est également ajouter des plug-ins pour simplifier certaines tâches répétitives, comme l’audit et la pagination.

[Dépôt GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Récupérez la clé primaire (y compris les clés composites) de n’importe quelle entité sous la forme d’un dictionnaire.

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Wrappers d’extension réactifs pour des versions observables à chaud des entités Entity Framework.

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Ajoutez des déclencheurs à vos entités avec des événements d’insertion, de mise à jour et de suppression. Il existe trois événements pour chacun de ceux-ci : avant, après et en cas d’échec.

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Obtenez un accès typé à OriginalValue des propriétés de votre entité. Les propriétés simples et complexes sont prises en charge ; la navigation et les collections ne le sont pas.

[Dépôt GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco fournit un générateur de modèles inverses avec prise en charge de la pluralisation/singularisation et des modèles modifiables basés sur des chaînes interpolées C# 6.0, s’exécutant sur .NET Core. Il fournit également un générateur de scripts d’amorçage avec des scripts SQL Merge et un exécuteur de scripts.

[Dépôt GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore est un ensemble gratuit d’extensions pour les utilisateurs avancés LINQ to SQL et EntityFrameworkCore. Prend en charge Include(...) et IDbAsync.

[Dépôt GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore fournit des extensions pratiques pour l’utilisation de fournisseurs LINQ, comme Entity Framework, qui prennent en charge seulement un sous-ensemble mineur des fonctions .NET, la réutilisation de fonctions, la réécriture des requêtes (en les rendant même si nécessaire capables de traiter les valeurs null) et la génération de requêtes dynamiques avec des sélecteurs et des prédicats traduisibles.

[Dépôt GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Plug-in pour Microsoft.EntityFrameworkCore prenant en charge un référentiel, les modèles d’unités de travail et les bases de données multiples avec des transactions distribuées.

[Dépôt GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Chargement différé pour EF Core 1.1

[Dépôt GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensions EntityFrameworkCore pour les opérations en bloc (insertion, mise à jour, suppression).

[Dépôt GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Ajoute la pluralisation au moment du design sur EF Core.

[Dépôt GitHub](https://github.com/bricelam/EFCore.Pluralizer)
