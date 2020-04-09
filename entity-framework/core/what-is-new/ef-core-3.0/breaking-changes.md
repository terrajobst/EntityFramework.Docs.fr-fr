---
title: Changements cassants dans EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417460"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>Changements de rupture inclus dans EF Core 3.0

L’API et les changements de comportement suivants ont le potentiel de briser les applications existantes lors de leur mise à niveau à 3.0.0.
Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](xref:core/providers/provider-log).

## <a name="summary"></a>Résumé

| **Modification critique**                                                                                               | **Impact** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Les requêtes LINQ ne sont plus évaluées sur le client](#linq-queries-are-no-longer-evaluated-on-the-client)         | Élevé       |
| [EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0](#netstandard21) | Élevé      |
| [L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core](#dotnet-ef) | Élevé      |
| [DetectChanges honore les valeurs de clés générées par le magasin](#dc) | Élevé      |
| [FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés](#fromsql) | Élevé      |
| [Les types de requêtes sont regroupés avec les types d’entités](#qt) | Élevé      |
| [Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core](#no-longer) | Moyenne      |
| [Les suppressions en cascade se produisent désormais immédiatement par défaut](#cascade) | Moyenne      |
| [Le chargement avide d’entités connexes se fait maintenant en une seule requête](#eager-loading-single-query) | Moyenne      |
| [La sémantique de DeleteBehavior.Restrict est désormais plus propre](#deletebehavior) | Moyenne      |
| [L’API de configuration pour les relations de type détenu a changé](#config) | Moyenne      |
| [Chaque propriété utilise la génération indépendante de clé de type entier en mémoire](#each) | Moyenne      |
| [Les requêtes sans suivi ne procèdent plus à la résolution de l’identité](#notrackingresolution) | Moyenne      |
| [Changements apportés à l’API de métadonnées](#metadata-api-changes) | Moyenne      |
| [Modifications de l’API de métadonnées spécifiques au fournisseur](#provider) | Moyenne      |
| [UseRowNumberForPaging a été supprimé](#urn) | Moyenne      |
| [De la méthodeSql lorsqu’il est utilisé avec la procédure stockée ne peut pas être composé](#fromsqlsproc) | Moyenne      |
| [Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête](#fromsql) | Faible      |
| [~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli](#qe) | Faible      |
| [Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités](#tkv) | Faible      |
| [Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives](#de) | Faible      |
| [Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété](#aes) | Faible      |
| [Les entités possédées ne peuvent pas être interrogées sans que le propriétaire utilise une requête de suivi](#owned-query) | Faible      |
| [Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés](#ip) | Faible      |
| [La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale](#fkp) | Faible      |
| [La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope](#dbc) | Faible      |
| [Les champs de stockage sont utilisés par défaut](#backing-fields-are-used-by-default) | Faible      |
| [Erreur si plusieurs champs de stockage compatibles sont détectés](#throw-if-multiple-compatible-backing-fields-are-found) | Faible      |
| [Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ](#field-only-property-names-should-match-the-field-name) | Faible      |
| [AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache](#adddbc) | Faible      |
| [AddEntityFrameworkMD ajoute IMemoryCache avec une limite de taille](#addentityframework-adds-imemorycache-with-a-size-limit) | Faible      |
| [DbContext.Entry effectue maintenant un DetectChanges local](#dbe) | Faible      |
| [Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut](#string-and-byte-array-keys-are-not-client-generated-by-default) | Faible      |
| [ILoggerFactory est désormais un service délimité](#ilf) | Faible      |
| [Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Faible      |
| [La création excessive de fournisseurs de services internes est maintenant une erreur par défaut](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Faible      |
| [Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique](#nbh) | Faible      |
| [Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask](#rtnt) | Faible      |
| [L’annotation Relational:TypeMapping est désormais simplement TypeMapping](#rtt) | Faible      |
| [ToTable sur un type dérivé lève une exception](#totable-on-a-derived-type-throws-an-exception) | Faible      |
| [EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite](#pragma) | Faible      |
| [Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3](#sqlite3) | Faible      |
| [Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite](#guid) | Faible      |
| [Les valeurs char sont maintenant stockées au format TEXT sur SQLite](#char) | Faible      |
| [Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante](#migid) | Faible      |
| [Les infos/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension](#xinfo) | Faible      |
| [LogQueryPossibleExceptionWithAggregateOperator a été renommé](#lqpe) | Faible      |
| [Clarifier les noms de contrainte de clé étrangère dans l’API](#clarify) | Faible      |
| [IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques](#irdc2) | Faible      |
| [Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency](#dip) | Faible      |
| [SQLitePCL.raw mis à jour vers la version 2.0.0](#SQLitePCL) | Faible      |
| [NetTopologySuite mis à jour vers la version 2.0.0](#NetTopologySuite) | Faible      |
| [Microsoft.Data.SqlClient est utilisé au lieu de System.Data.SqlClient](#SqlClient) | Faible      |
| [Plusieurs relations autoréférencées ambiguës doivent être configurées](#mersa) | Faible      |
| [DbFunction.Schema étant null ou chaîne vide configure pour être dans le schéma par défaut du modèle](#udf-empty-string) | Faible      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Les requêtes LINQ ne sont plus évaluées sur le client

[Les #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
de suivi[voient également la question #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Vieux comportement**

Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.
Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.

**Nouveau comportement**

À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).
Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.

**Pourquoi**

L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.
Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.
Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.
Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.
Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.

De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.

**Atténuations**

Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3.0 cible .NET Standard 2.1 plutôt que .NET Standard 2.0

Suivi de problème no 15498

> [!IMPORTANT] 
> EF Core 3.1 vise à nouveau .NET Standard 2.0. Cela ramène le soutien pour .NET Framework.

**Vieux comportement**

Avant la version 3.0, EF Core ciblait .NET Standard 2.0 et s’exécutait sur toutes les plateformes qui prennent en charge cette norme, y compris .NET Framework.

**Nouveau comportement**

À partir de la version 3.0, EF Core cible .NET Standard 2.1 et s’exécute sur toutes les plateformes qui prennent en charge cette norme. Cela n'inclut pas .NET Framework.

**Pourquoi**

Cela fait partie d’une décision stratégique dans les technologies .NET de concentrer l’énergie sur .NET Core et d’autres plateformes .NET modernes, telles que Xamarin.

**Atténuations**

Utilisez EF Core 3.1.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core

[Annonces de suivi du problème n° 325](https://github.com/aspnet/Announcements/issues/325)

**Vieux comportement**

Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.

**Nouveau comportement**

À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.

**Pourquoi**

Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non. De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.

Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.
Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.

**Atténuations**

Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>L’outil en ligne de commande EF Core, dotnet ef, ne fait plus partie du SDK .NET Core

[Suivi du problème n° 14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Vieux comportement**

Avant la version 3.0, l’outil `dotnet ef` était inclus dans le SDK .NET Core et prêt à l’emploi depuis la ligne de commande d’un projet sans nécessiter d’étapes supplémentaires. 

**Nouveau comportement**

À compter de la version 3.0, le SDK .NET n’inclut pas l’outil `dotnet ef`, donc vous pouvez explicitement l’installer pour pouvoir l’utiliser comme outil local ou global. 

**Pourquoi**

Ce changement nous permet de distribuer et mettre à jour `dotnet ef` sous forme d’outil CLI .NET normal sur NuGet, ce qui est cohérent avec le fait qu’EF Core 3.0 est également toujours distribué sous forme de package NuGet.

**Atténuations**

Pour pouvoir gérer des migrations ou générer un `DbContext`, installez `dotnet-ef` comme outil général :

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Vous pouvez également obtenir un outil local quand vous restaurez les dépendances d’un projet qui le déclare en tant que dépendance d’outil à l’aide d’un [fichier manifeste d’outil](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql et ExecuteSqlAsync ont été renommés

[Suivi du problème n<c0>o </c0>10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Vieux comportement**

Avant EF Core 3.0, ces noms de méthode étaient surchargés pour être utilisés avec une chaîne normale ou une chaîne devant être interpolée dans SQL et dans des paramètres.

**Nouveau comportement**

À compter d’EF Core 3.0, utilisez `FromSqlRaw`, `ExecuteSqlRaw` et `ExecuteSqlRawAsync` pour créer une requête paramétrable, où les paramètres sont passés séparément à partir de la chaîne de requête.
Par exemple :

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Utilisez `FromSqlInterpolated`, `ExecuteSqlInterpolated` et `ExecuteSqlInterpolatedAsync` pour créer une requête paramétrable, où les paramètres sont passés dans le cadre d’une chaîne de requête interpolée.
Par exemple :

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Notez que les deux requêtes ci-dessus produisent le même SQL paramétrable avec les mêmes paramètres SQL.

**Pourquoi**

Les surcharges de méthode comme celle-ci rendent très facile un appel accidentel à la méthode de chaîne brute quand l’intention est d’appeler la méthode de chaîne interpolée, et inversement.
De ce fait, les requêtes ne sont plus paramétrables alors qu’elles devraient l’être.

**Atténuations**

Utilisez plutôt les nouveaux noms de méthode.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>De la méthodeSql lorsqu’il est utilisé avec la procédure stockée ne peut pas être composé

[#15392 de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Vieux comportement**

Avant EF Core 3.0, la méthode FromSql a essayé de détecter si le SQL passé peut être composé sur. Il a fait l’évaluation des clients lorsque la SQL n’était pas composable comme une procédure stockée. La requête suivante a fonctionné en exécutant la procédure stockée sur le serveur et en faisant FirstOrDefault du côté du client.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Nouveau comportement**

À partir de EF Core 3.0, EF Core n’essaiera pas d’analyser la SQL. Donc, si vous composez après FromSqlRaw / FromSqlInterpolated, puis EF Core composera le SQL en provoquant des sous-requêtes. Donc, si vous utilisez une procédure stockée avec la composition, alors vous obtiendrez une exception pour la syntaxe SQL invalide.

**Pourquoi**

EF Core 3.0 ne prend pas en charge l’évaluation automatique des clients, car il s’agissait d’erreurs sujettes comme expliqué [ici](#linq-queries-are-no-longer-evaluated-on-the-client).

**Limitation des risques**

Si vous utilisez une procédure stockée dans FromSqlRaw/FromSqlInterpolated, vous savez qu’elle ne peut pas être composée, de sorte que vous pouvez ajouter __AsEnumerable /AsAsyncEnumerable__ juste après l’appel de méthode FromSql pour éviter toute composition sur le côté serveur.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>Les méthodes FromSql peuvent uniquement être spécifiées sur les racines de requête

Suivi de problème no 15704

**Vieux comportement**

Avant EF Core 3.0, la méthode `FromSql` pouvant être spécifiée n’importe où dans la requête.

**Nouveau comportement**

À partir d’EF Core 3.0, les nouvelles méthodes `FromSqlRaw` et `FromSqlInterpolated` (qui remplacent `FromSql`) peuvent être spécifiées uniquement sur les racines de requête, par exemple, directement sur le `DbSet<>`. Une erreur de compilation survient si vous tentez de les spécifier à un autre emplacement.

**Pourquoi**

La spécification de `FromSql` n’importe où autre que sur un `DbSet` n’avait aucune signification ni valeur ajoutée et pouvait entraîner une ambiguïté dans certains scénarios.

**Atténuations**

Les appels `FromSql` doivent être déplacés pour être directement sur le `DbSet` auquel ils s’appliquent.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Les requêtes sans suivi ne procèdent plus à la résolution de l’identité

Suivi de problème no 13518

**Vieux comportement**

Avant EF Core 3.0, la même instance d’entité était utilisée pour chaque occurrence d’une entité avec un type et un ID donnés. Cela correspond au comportement des requêtes de suivi. Par exemple, cette requête :

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
retournait la même instance `Category` pour chaque `Product` associé à la catégorie donnée.

**Nouveau comportement**

À compter de EF Core 3.0, différentes instances d’entité sont créées lorsqu’une entité avec un type et un ID donnés est rencontrée à différents emplacements dans le graphe retourné. Par exemple, la requête ci-dessus retourne à présent une nouvelle instance `Category` pour chaque `Product` même si deux produits sont associés à la même catégorie.

**Pourquoi**

La résolution de l’identité (autrement dit, le fait de déterminer qu’une entité a les mêmes type et ID qu’une entité précédemment rencontrée) améliore les performances et la surcharge de la mémoire. Cela agit généralement à l’opposé de la raison pour laquelle aucune requête de suivi n’est utilisée en premier lieu. En outre, même si la résolution de l’identité peut parfois être utile, elle n’est pas nécessaire si les entités doivent être sérialisées et envoyées à un client, ce qui est courant pour les requêtes sans suivi.

**Atténuations**

Utilisez une requête de suivi si la résolution de l’identité est requise.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~L’exécution de requêtes est enregistrée au niveau du débogage ~~Rétabli

[Suivi du problème n<c0>o </c0>14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Nous avons rétabli ce changement car la nouvelle configuration dans EF Core 3.0 permet la spécification du niveau d’enregistrement d’un événement par l’application. Par exemple, pour basculer l’enregistrement de SQL vers `Debug`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext` :
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités

[Suivi du problème n<c0>o </c0>12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Vieux comportement**

Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.
Généralement, ces valeurs temporaires étaient de grands nombres négatifs.

**Nouveau comportement**

À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.

**Pourquoi**

Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`. 

**Atténuations**

Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.
Vous pouvez éviter cela en :
* N’utilisant pas de clés générées par le magasin.
* Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.
* Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.
Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges honore les valeurs de clés générées par le magasin

[Suivi du problème n<c0>o </c0>14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Vieux comportement**

Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.

**Nouveau comportement**

À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.
Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.
Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.

**Pourquoi**

Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.

**Atténuations**

Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.
La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.
Par exemple, avec l’API Fluent :

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Ou avec des annotations de données :

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Les suppressions en cascade se produisent désormais immédiatement par défaut

[Suivi du problème n<c0>o </c0>10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Vieux comportement**

Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.

**Nouveau comportement**

À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.
Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.

**Pourquoi**

Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant l’appel à _ `SaveChanges`.

**Atténuations**

Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangeTracker`.
Par exemple :

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>Le chargement avide d’entités connexes se fait maintenant en une seule requête

[Problème de suivi #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Vieux comportement**

Avant 3.0, le chargement `Include` enthousiaste des navigations de collecte par l’intermédiaire des opérateurs a fait générer plusieurs requêtes sur la base de données relationnelle, une pour chaque type d’entité connexe.

**Nouveau comportement**

En commençant par 3.0, EF Core génère une seule requête avec JOINs sur les bases de données relationnelles.

**Pourquoi**

L’émission de multiples requêtes pour mettre en œuvre une seule requête linQ a causé de nombreux problèmes, y compris le rendement négatif que plusieurs allers-retours de base de données étaient nécessaires, et les problèmes de cohérence de données que chaque requête pourrait observer un état différent de la base de données.

**Atténuations**

Bien que techniquement ce n’est pas un changement de rupture, il pourrait avoir `Include` un effet considérable sur les performances de l’application quand une seule requête contient un grand nombre d’opérateurs sur les navigations de collecte. [Consultez ce commentaire](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) pour plus d’informations et pour réécrire les requêtes d’une manière plus efficace.

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>La sémantique de DeleteBehavior.Restrict est désormais plus propre

[Suivi de problème no 12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Vieux comportement**

Avant la version 3.0, `DeleteBehavior.Restrict` créait les clés étrangères dans la base de données avec la sémantique `Restrict`, et il modifiait les corrections internes d’une manière non évidente.

**Nouveau comportement**

À compter de la version 3.0, `DeleteBehavior.Restrict` garantit la création des clés étrangères avec la sémantique `Restrict` (autrement dit, sans levée de cascades lors d’une violation de contrainte), sans impact sur les corrections internes EF.

**Pourquoi**

Ce changement a été apporté pour améliorer l’expérience et permettre une utilisation intuitive de `DeleteBehavior`, sans effets secondaires inattendus.

**Atténuations**

Vous pouvez restaurer le comportement précédent par le biais de `DeleteBehavior.ClientNoAction`.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Les types de requêtes sont regroupés avec les types d’entités

[Suivi du problème n<c0>o </c0>14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Vieux comportement**

Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/keyless-entity-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.
Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).

**Nouveau comportement**

Un type de requête devient maintenant simplement un type d’entité sans clé primaire.
Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.

**Pourquoi**

Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.
Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.
De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.

**Atténuations**

Les parties suivantes de l’API sont désormais obsolètes :
* **`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Au lieu de cela doit être appelé pour marquer un type d’entité comme n’ayant pas de clés.
Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.
* **`DbQuery<>`**- `DbSet<>` Au lieu de cela devrait être utilisé.
* **`DbContext.Query<>()`**- `DbContext.Set<>()` Au lieu de cela devrait être utilisé.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>L’API de configuration pour les relations de type détenu a changé

[Suivi de la question #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[problème de suivi #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[problème de suivi #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Vieux comportement**

Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`. 

**Nouveau comportement**

À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.
Par exemple :

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.
En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.
Par exemple :

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

De plus, l’appel à `Entity()`, `HasOne()` ou `Set()` avec un type détenu comme cible lève désormais une exception.

**Pourquoi**

Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.
Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.

**Atténuations**

Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives

[Suivi du problème n<c0>o </c0>9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Vieux comportement**

Considérez le modèle suivant :
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, une instance `OrderDetails` est toujours nécessaire pour ajouter un nouveau `Order`.


**Nouveau comportement**

À partir de la version 3.0, EF Core permet d’ajouter un `Order` sans `OrderDetails` et mappe toutes les propriétés de `OrderDetails` à l’exception de la clé primaire aux colonnes de type nullable.
Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.

**Atténuations**

Si votre modèle a une table qui partage des dépendances avec toutes les colonnes facultatives, mais que la navigation qui pointe vers lui n’est pas censée être `null`, l’application doit être modifiée pour gérer les situations où la navigation est `null`. Si ce n’est pas possible, une propriété obligatoire doit être ajoutée au type d’entité ou au moins une propriété doit avoir une valeur non-`null`.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Toutes les entités qui partagent une table avec une colonne de jeton de concurrence doivent la mapper à une propriété

[Suivi du problème n<c0>o </c0>14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Vieux comportement**

Considérez le modèle suivant :
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
Avant EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, la mise à jour de `OrderDetails` seulement ne met pas à jour la valeur de `Version` sur le client et la prochaine mise à jour échoue.


**Nouveau comportement**

À compter de la version 3.0, EF Core propage la nouvelle valeur `Version` sur `Order` s’il est propriétaire de `OrderDetails`. Sinon, une exception est levée pendant la validation de modèle.

**Pourquoi**

Ce changement a été effectué pour éviter la péremption de la valeur du jeton de concurrence quand une seule des entités mappées à la même table est mise à jour.

**Atténuations**

Toutes les entités partageant la table doivent inclure une propriété mappée à la colonne de jeton de concurrence. Vous pouvez en créer une dans un état fantôme :
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Les entités possédées ne peuvent pas être interrogées sans que le propriétaire utilise une requête de suivi

[#18876 de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Vieux comportement**

Avant EF Core 3.0, les entités détenues pouvaient être interrogées comme toute autre navigation.

```csharp
context.People.Select(p => p.Address);
```

**Nouveau comportement**

À partir de 3.0, EF Core lancera si une requête de suivi projette une entité détenue sans le propriétaire.

**Pourquoi**

Les entités possédées ne peuvent pas être manipulées sans le propriétaire, de sorte que dans la grande majorité des cas les interroger de cette façon est une erreur.

**Atténuations**

Si l’entité détenue doit être suivie pour être modifiée d’une manière ou d’une autre plus tard, le propriétaire doit être inclus dans la requête.

Sinon, `AsNoTracking()` ajoutez un appel :

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Les propriétés héritées de types non mappés sont maintenant mappées à une seule colonne pour tous les types dérivés

[Suivi du problème n<c0>o </c0>13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Vieux comportement**

Considérez le modèle suivant :
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

Avant EF Core 3.0, la propriété `ShippingAddress` est mappée à des colonnes distinctes pour `BulkOrder` et `Order` par défaut.

**Nouveau comportement**

À compter de la version 3.0, EF Core crée uniquement une colonne pour `ShippingAddress`.

**Pourquoi**

L’ancien comportement n’était pas attendu.

**Atténuations**

La propriété peut toujours être mappée explicitement à une colonne distincte sur les types dérivés :

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale

[Suivi du problème n<c0>o </c0>13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Vieux comportement**

Considérez le modèle suivant :
```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
Avant EF Core 3.0, la propriété `CustomerId` était utilisée pour la clé étrangère par convention.
Toutefois, si `Order` est un type détenu, cela fait également de `CustomerId` la clé primaire, ce qui ne correspond généralement pas aux attentes.

**Nouveau comportement**

À compter de la version 3.0, EF Core ne tente pas d’utiliser des propriétés pour les clés étrangères par convention si elles ont le même nom que la propriété principale.
Les modèles de nom du type de principal concaténé au nom de la propriété de principal, et de nom de navigation concaténé au nom de propriété de principal sont toujours mis en correspondance.
Par exemple :

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Pourquoi**

Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.

**Atténuations**

Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>La connexion de base de données est maintenant fermée si elle n’est plus utilisée avant la fin de TransactionScope

[Suivi du problème n<c0>o </c0>14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Vieux comportement**

Avant EF Core 3.0, si le contexte ouvre la connexion à l’intérieur d’un `TransactionScope`, la connexion reste ouverte quand le `TransactionScope` actuel est actif.

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

**Nouveau comportement**

À compter de la version 3.0, EF Core ferme la connexion dès qu’il ne l’utilise plus.

**Pourquoi**

Ce changement permet d’utiliser plusieurs contextes dans le même `TransactionScope`. Le nouveau comportement correspond également à EF6.

**Atténuations**

Si la connexion doit rester ouverte, un appel explicite à `OpenConnection()` garantit qu’EF Core ne la ferme pas prématurément :

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Chaque propriété utilise la génération indépendante de clé de type entier en mémoire

[Suivi du problème n<c0>o </c0>6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Vieux comportement**

Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.

**Nouveau comportement**

À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.
De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.

**Pourquoi**

Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.

**Atténuations**

Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.
Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.

### <a name="backing-fields-are-used-by-default"></a>Les champs de stockage sont utilisés par défaut

[Suivi du problème n<c0>o </c0>12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Vieux comportement**

Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.
L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.

**Nouveau comportement**

Depuis EF Core 3.0, si le champ de stockage d’une propriété est connu, alors EF Core lit et écrit toujours cette propriété à l’aide du champ de stockage.
Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.

**Pourquoi**

Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.

**Atténuations**

Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété sur `ModelBuilder`.
Par exemple :

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Erreur si plusieurs champs de stockage compatibles sont détectés

[Suivi du problème n<c0>o </c0>12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Vieux comportement**

Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.
Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.

**Nouveau comportement**

À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.

**Pourquoi**

Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.

**Atténuations**

Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.
Par exemple, à l’aide de l’API Fluent :

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Les noms de propriété de type « champ uniquement » doivent correspondre au nom du champ

**Vieux comportement**

Avant EF Core 3.0, une propriété pouvait être spécifiée par une valeur de chaîne et si aucune propriété avec ce nom n’a été trouvée sur le type .NET alors EF Core essaierait de l’assortir à un champ en utilisant des règles de convention.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nouveau comportement**

À compter d’EF Core 3.0, une propriété de type « champ uniquement » doit correspondre exactement au nom du champ.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Pourquoi**

Ce changement a été apporté pour éviter d’utiliser un même champ pour deux propriétés portant un nom similaire. De plus, les règles de correspondance des propriétés de type « champ uniquement » sont désormais les mêmes que pour les propriétés mappées aux propriétés CLR.

**Atténuations**

Les propriétés de type « champ uniquement » doivent porter le même nom que le champ auquel elles sont mappées.
Dans une version future de EF Core après 3.0, nous prévoyons de ré-activer explicitement la configuration d’un nom de champ qui est différent du nom de propriété (voir question [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)) :

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache

[Suivi du problème n<c0>o </c0>14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Vieux comportement**

Avant EF Core `AddDbContext` 3.0, appeler ou `AddDbContextPool` également enregistrer des services d’enregistrement et de mise en cache de mémoire avec DI par le biais d’appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nouveau comportement**

À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.

**Pourquoi**

Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application. Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.

**Atténuations**

Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFrameworkMD ajoute IMemoryCache avec une limite de taille

[#12905 de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Vieux comportement**

Avant EF Core 3.0, les méthodes d’appel `AddEntityFramework*` enregistreraient également les services de mise en cache de mémoire avec DI sans limite de taille.

**Nouveau comportement**

En commençant par EF Core `AddEntityFramework*` 3.0, enregistrera un service IMemoryCache avec une limite de taille. Si d’autres services ajoutés par la suite dépendent d’IMemoryCache, ils peuvent rapidement atteindre la limite par défaut, ce qui entraîne des exceptions ou des performances dégradées.

**Pourquoi**

L’utilisation d’IMemoryCache sans limite pourrait entraîner une utilisation incontrôlée de la mémoire s’il y a un bogue dans la logique de mise en cache des requêtes ou si les requêtes sont générées dynamiquement. Avoir une limite par défaut atténue une attaque DoS potentielle.

**Atténuations**

Dans la `AddEntityFramework*` plupart des `AddDbContext` cas, l’appel n’est pas nécessaire si ou `AddDbContextPool` est appelé ainsi. Par conséquent, la meilleure `AddEntityFramework*` atténuation est de supprimer l’appel.

Si votre application a besoin de ces services, enregistrez une implémentation IMemoryCache explicitement avec le conteneur DI à l’avance à l’aide [d’AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry effectue maintenant un DetectChanges local

[Suivi du problème n<c0>o </c0>13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Vieux comportement**

Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.
Cela garantissait que l’état exposé dans `EntityEntry` était à jour.

**Nouveau comportement**

À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.
Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.

Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.

Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.

**Pourquoi**

Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.

**Atténuations**

Appelez `ChangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut

[Suivi du problème n<c0>o </c0>14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Vieux comportement**

Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.
Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.

**Nouveau comportement**

À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.

**Pourquoi**

Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.

**Atténuations**

Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.
Par exemple, avec l’API Fluent :

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Ou avec des annotations de données :

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory est désormais un service délimité

[Suivi du problème n<c0>o </c0>14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Vieux comportement**

Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.

**Nouveau comportement**

À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.

**Pourquoi**

Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.

**Atténuations**

Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,
ce qui n’est pas courant.
Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.

Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées

[Suivi du problème n<c0>o </c0>12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Vieux comportement**

Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.
Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.
Dans ces cas-là, toute tentative de chargement différé constituait une no-op.

**Nouveau comportement**

À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.
Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.
À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.
Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.

**Pourquoi**

Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.

**Atténuations**

Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>La création excessive de fournisseurs de services internes est maintenant une erreur par défaut

[Suivi du problème n<c0>o </c0>10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Vieux comportement**

Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.

**Nouveau comportement**

À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée. 

**Pourquoi**

Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.

**Atténuations**

En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.
Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.
Par exemple :

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique

[Suivi du problème n<c0>o </c0>9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Vieux comportement**

Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.
Par exemple :
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.

En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.

**Nouveau comportement**

À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.

**Pourquoi**

L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.

**Atténuations**

Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,
ce qui n’est pas courant.
Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.
Par exemple :

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Le type de retour Task pour plusieurs méthodes asynchrones a été remplacé par ValueTask

[Suivi du problème n<c0>o </c0>15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Vieux comportement**

Les méthodes asynchrones suivantes retournaient précédemment un `Task<T>` :

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (et classes dérivées)

**Nouveau comportement**

Les méthodes mentionnées précédemment retournent maintenant un `ValueTask<T>` sur le même `T` qu’avant.

**Pourquoi**

Ce changement réduit le nombre d’allocations de tas induits par l’appel de ces méthodes, ce qui améliore les performances générales.

**Atténuations**

Les applications qui sont simplement en attente des API ci-dessus doivent uniquement être recompilées, vous n’avez pas besoin de changer la source.
Une utilisation plus complexe (par exemple, le passage du `Task` retourné à `Task.WhenAny()`) nécessite généralement que le `ValueTask<T>` retourné soit converti en `Task<T>` en appelant `AsTask()` sur lui.
Notez que cette opération annule la réduction d’allocation apportée par ce changement.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>L’annotation Relational:TypeMapping est désormais simplement TypeMapping

[Suivi du problème n<c0>o </c0>9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Vieux comportement**

Le nom d’annotation pour les annotations de mappage de types était « annotations ».

**Nouveau comportement**

Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».

**Pourquoi**

Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.

**Atténuations**

Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.
La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable sur un type dérivé lève une exception 

[Suivi du problème n<c0>o </c0>11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Vieux comportement**

Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide. 

**Nouveau comportement**

À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.

**Pourquoi**

Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.
Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.

**Atténuations**

Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex est remplacé par HasIndex 

[Suivi du problème n<c0>o </c0>12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Vieux comportement**

Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.

**Nouveau comportement**

À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.
Utilisez `HasIndex().ForSqlServerInclude()`.

**Pourquoi**

Ce changement a été apporté afin de consolider l’API pour les index avec `Include` dans un seul emplacement pour tous les fournisseurs de bases de données.

**Atténuations**

Utilisez la nouvelle API, comme indiqué ci-dessus.

### <a name="metadata-api-changes"></a>Changements apportés à l’API de métadonnées

Suivi du problème no 214

**Nouveau comportement**

Les propriétés suivantes ont été converties en méthodes d’extension :

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Pourquoi**

Ce changement simplifie l’implémentation des interfaces mentionnées précédemment.

**Atténuations**

Utilisez les nouvelles méthodes d’extension.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Modifications de l’API de métadonnées spécifiques au fournisseur

Suivi du problème no 214

**Nouveau comportement**

Les méthodes d’extension spécifiques au fournisseur seront aplaties :

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Pourquoi**

Ce changement simplifie l’implémentation des méthodes d’extension mentionnées précédemment.

**Atténuations**

Utilisez les nouvelles méthodes d’extension.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite

[Suivi du problème n<c0>o </c0>12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Vieux comportement**

Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.

**Nouveau comportement**

À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.

**Pourquoi**

Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.

**Atténuations**

Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.
Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3

**Vieux comportement**

Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.

**Nouveau comportement**

À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.

**Pourquoi**

Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.

**Atténuations**

Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Les valeurs GUID sont maintenant stockées au format TEXT sur SQLite

[Suivi du problème #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Vieux comportement**

Avant, les valeurs GUID étaient stockées comme valeurs BLOB sur SQLite.

**Nouveau comportement**

Maintenant, les valeurs Guid sont stockées au format TEXT.

**Pourquoi**

Le format binaire des valeurs GUID n’est pas normalisé. Le stockage des valeurs au format TEXT rend la base de données plus compatible avec d’autres technologies.

**Atténuations**

Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite peut toujours lire les valeurs GUID dans les colonnes BLOB et TEXT. Toutefois, en raison du changement du format par défaut pour les paramètres et les constantes, vous devrez probablement apporter des modifications dans la plupart des scénarios utilisant des valeurs GUID.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Les valeurs char sont maintenant stockées au format TEXT sur SQLite

[Suivi du problème n<c0>o </c0>15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Vieux comportement**

Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite. Par exemple, une valeur char de *A* était stockée comme valeur entière 65.

**Nouveau comportement**

Maintenant, les valeurs char sont stockées au format TEXT.

**Pourquoi**

Le stockage des valeurs au format TEXT est plus naturel et rend la base de données plus compatible avec d’autres technologies.

**Atténuations**

Vous pouvez migrer des bases de données existantes vers le nouveau format en exécutant SQL comme suit.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

Dans EF Core, vous pouvez également continuer à utiliser le comportement précédent en configurant un convertisseur de valeur sur ces propriétés.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite est aussi toujours capable de lire les valeurs de caractère à partir de colonnes INTEGER et TEXT, donc certains scénarios peuvent ne nécessiter aucune action.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante

[Suivi du problème n<c0>o </c0>12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Vieux comportement**

Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.

**Nouveau comportement**

Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).

**Pourquoi**

L’ordre des migrations est important lors de la mise à jour de la base de données ou de la résolution des conflits de fusion. L’utilisation du calendrier invariant permet d’éviter les problèmes de classement qui peuvent se produire si les membres de l’équipe ont des calendriers système différents.

**Atténuations**

Ce changement affecte tous ceux qui utilisent un calendrier non grégorien où l’année est en avance sur le calendrier grégorien (comme le calendrier bouddhiste thaïlandais). Les ID de migration existants devront être mis à jour afin que les nouvelles migrations soient classées après les migrations existantes.

Vous trouverez les ID de migration dans l’attribut Migration des fichiers de concepteur des migrations.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Le tableau de l’historique Migrations doit également être mis à jour.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging a été supprimé

Suivi de problème no 16400

**Vieux comportement**

Avant EF Core 3.0, `UseRowNumberForPaging` pouvait être utilisé pour générer SQL pour la pagination qui est compatible avec SQL Server 2008.

**Nouveau comportement**

À compter de EF Core 3.0, EF génère uniquement SQL pour la pagination qui est uniquement compatible avec les versions de SQL Server ultérieures. 

**Pourquoi**

Nous effectuons cette modification, car [SQL Server 2008 n’est plus un produit pris en charge](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) et la mise à jour de cette fonctionnalité pour fonctionner avec les modifications de requête effectuées dans EF Core 3.0 est un travail significatif.

**Atténuations**

Nous vous recommandons d’effectuer la mise à jour vers une version plus récente de SQL Server ou d’utiliser un niveau de compatibilité plus élevé, afin que le SQL généré soit pris en charge. Cela dit, si vous ne pouvez pas faire cela, veuillez [commenter le problème de suivi](https://github.com/aspnet/EntityFrameworkCore/issues/16400) en incluant des détails. Nous pourrions revisiter cette décision en fonction des commentaires.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Les info/métadonnées d’extension ont été supprimées de IDbContextOptionsExtension

Suivi du problème no 16119

**Vieux comportement**

`IDbContextOptionsExtension` contenait des méthodes permettant de fournir des métadonnées relatives à l’extension.

**Nouveau comportement**

Ces méthodes ont été déplacées vers une nouvelle classe de base abstraite `DbContextOptionsExtensionInfo`, qui est retournée à partir d’une nouvelle propriété `IDbContextOptionsExtension.Info`.

**Pourquoi**

Sur les versions de 2.0 à 3.0, nous devions ajouter ou modifier ces méthodes plusieurs fois.
Les sortir dans une nouvelle classe de base abstraite simplifie l’apport de ces types de changements sans résiliation des extensions existantes.

**Atténuations**

Mettre à jour des extensions pour suivre le nouveau modèle.
Des exemples se trouvent dans la plupart des implémentations de `IDbContextOptionsExtension` pour différents types d’extensions dans le code source EF Core.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator a été renommé

[Suivi du problème #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**changement**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a été renommé en `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Pourquoi**

Aligne le nom de cet événement d’avertissement avec tous les autres événements d’avertissement.

**Atténuations**

Utilisez le nouveau nom. (Notez que le numéro d’ID événement n’a pas changé.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Clarifier les noms de contrainte de clé étrangère dans l’API

[Suivi du problème #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Vieux comportement**

Avant EF Core 3.0, les noms de contrainte de clé étrangère étaient désignés simplement par le terme « nom ». Par exemple :

```csharp
var constraintName = myForeignKey.Name;
```

**Nouveau comportement**

À compter d’EF Core 3.0, les noms de contrainte de clé étrangère sont désignés par « noms de contrainte ». Par exemple :

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Pourquoi**

Ce changement rend le nommage plus cohérent dans ce domaine. Il clarifie également le fait qu’il s’agit du nom de la contrainte de clé étrangère et pas du nom de la colonne ou de la propriété sur laquelle la clé étrangère est définie.

**Atténuations**

Utilisez le nouveau nom.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync ont été rendues publiques

Suivi du problème no 15997

**Vieux comportement**

Avant EF Core 3.0, ces méthodes étaient protégées.

**Nouveau comportement**

À compter d’EF Core 3.0, ces méthodes sont publiques.

**Pourquoi**

Ces méthodes sont utilisées par EF pour déterminer si une base de données est créée mais vide. Cela peut également être utile depuis l’extérieur d’EF lorsque vous déterminez s’il faut appliquer des migrations ou pas.

**Atténuations**

Modifiez l’accessibilité de n’importe quel remplacements.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft.EntityFrameworkCore.Design est désormais un package DevelopmentDependency

Suivi du problème no 11506

**Vieux comportement**

Avant EF Core 3.0, Microsoft.EntityFrameworkCore.Design était un package NuGet normal dont l’assembly pouvait être référencée par des projets qui en dépendaient.

**Nouveau comportement**

À compter d’EF Core 3.0, il s’agit d’un package DevelopmentDependency. Cela signifie que la dépendance ne s’écoulera pas de façon transitive dans d’autres projets, et que vous ne pouvez plus, par défaut, référencer son assemblage.

**Pourquoi**

Ce package est uniquement destiné à être utilisé au moment du design. Les applications déployées ne doivent pas y faire référence. Le fait de faire de ce package un DevelopmentDependency renforce cette recommandation.

**Atténuations**

Si vous avez besoin de référencer ce paquet pour remplacer le comportement de l’EF Core en temps de conception, vous pouvez mettre à jour les métadonnées de l’élément PackageReference dans votre projet.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Si le package est référencé de manière transitive via Microsoft.EntityFrameworkCore.Tools, vous devez ajouter un PackageReference explicite au package pour modifier ses métadonnées. Une telle référence explicite doit être ajoutée à tout projet où les types de l’emballage sont nécessaires.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw mis à jour vers la version 2.0.0

Suivi du problème no 14824

**Vieux comportement**

Microsoft.EntityFrameworkCore.Sqlite dépendait précédemment de la version 1.1.12 de SQLitePCL.raw.

**Nouveau comportement**

Nous avons mis à jour notre package pour dépendre de la version 2.0.0.

**Pourquoi**

La version 2.0.0 de SQLitePCL.raw cible .NET Standard 2.0. Elle ciblait précédemment .NET Standard 1.1 qui nécessitait une fermeture volumineuse de packages transitifs pour fonctionner.

**Atténuations**

SQLitePCL.raw version 2.0.0 inclut des changements cassants. Voir les [notes de sortie](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) pour plus de détails.

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite mis à jour vers la version 2.0.0

Suivi de problème no 14825

**Vieux comportement**

Les packages spatiaux dépendaient précédemment de la version 1.15.1 de NetTopologySuite.

**Nouveau comportement**

Nous avons mis à jour notre package pour dépendre de la version 2.0.0.

**Pourquoi**

La version 2.0.0 de NetTopologySuite vise à résoudre plusieurs problèmes de facilité d’utilisation rencontrés par les utilisateurs EF Core.

**Atténuations**

NetTopologySuite version 2.0.0 inclut des changements cassants. Voir les [notes de sortie](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) pour plus de détails.

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>Microsoft.Data.SqlClient est utilisé au lieu de System.Data.SqlClient

[#15636 de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Vieux comportement**

Microsoft.EntityFrameworkCore.SqlServer dépendait auparavant de System.Data.SqlClient.

**Nouveau comportement**

Nous avons mis à jour notre package pour dépendre de Microsoft.Data.SqlClient.

**Pourquoi**

Microsoft.Data.SqlClient est le moteur phare de l’accès aux données pour SQL Server à l’avenir, et System.Data.SqlClient ne fait plus l’objet du développement.
Certaines fonctionnalités importantes, telles que Always Encrypted, ne sont disponibles que sur Microsoft.Data.SqlClient.

**Atténuations**

Si votre code entraîne une dépendance directe à l’égard de System.Data.SqlClient, vous devez le modifier pour référencer Microsoft.Data.SqlClient à la place; comme les deux paquets maintiennent un degré très élevé de compatibilité API, cela ne devrait être qu’un simple paquet et changement d’espace de nom.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Plusieurs relations autoréférencées ambiguës doivent être configurées 

Suivi de problème no 13573

**Vieux comportement**

Un type d’entité avec plusieurs propriétés de navigation unidirectionnelle autoréférencées et les clés étrangères correspondantes a été incorrectement configuré en tant que relation unique. Par exemple :

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**Nouveau comportement**

Ce scénario est maintenant détecté dans la génération de modèle et une exception est levée, indiquant que le modèle est ambigu.

**Pourquoi**

Le modèle résultant est ambigu et probablement erroné dans ce cas.

**Atténuations**

Utilisez la configuration complète de la relation. Par exemple :

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema étant null ou chaîne vide configure pour être dans le schéma par défaut du modèle

[#12757 de suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Vieux comportement**

Un DbFunction configuré avec le schéma comme une chaîne vide a été traité comme fonction intégrée sans schéma. Par exemple, le `DatePart` code suivant `DATEPART` cartographiera la fonction CLR à la fonction intégrée sur SqlServer.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Nouveau comportement**

Toutes les cartes DbFunction sont considérées comme cartographiées sur des fonctions définies par l’utilisateur. Par conséquent, la valeur de chaîne vide mettrait la fonction à l’intérieur du schéma par défaut pour le modèle. Ce qui pourrait être le schéma configuré explicitement via l’API `modelBuilder.HasDefaultSchema()` fluide ou `dbo` autrement.

**Pourquoi**

Auparavant, le schéma étant vide était un moyen de traiter cette fonction est intégrée, mais cette logique est seulement applicable pour SqlServer où les fonctions intégrées n’appartiennent à aucun schéma.

**Atténuations**

Configurez la traduction de DbFunction manuellement pour la cartographier à une fonction intégrée.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
