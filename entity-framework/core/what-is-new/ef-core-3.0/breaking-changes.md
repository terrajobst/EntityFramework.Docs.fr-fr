---
title: Changements cassants dans EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 534ac95cccc03e9797ba766e601e2fe86eaf8061
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319216"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Changements cassants inclus dans EF Core 3.0 (actuellement en préversion)

> [!IMPORTANT]
> Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.

Les changements de comportement et d’API suivants sont susceptibles de casser les applications développées pour EF Core 2.2.x lors de leur mise à niveau vers la version 3.0.0.
Les changements qui, selon nous, auront une incidence uniquement sur les fournisseurs de base de données sont documentés dans [Changements ayant un impact sur les fournisseurs](../../providers/provider-log.md).
Les cassures dues aux nouvelles fonctionnalités introduites d’une préversion 3.0 à une autre ne sont pas documentées ici.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Les requêtes LINQ ne sont plus évaluées sur le client

[Suivi de problème no 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Voir également problème no 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant la version 3.0, quand EF Core ne pouvait pas convertir une expression faisant partie d’une requête en SQL ou en paramètre, elle évaluait automatiquement l’expression sur le client.
Par défaut, l’évaluation sur le client d’expressions potentiellement coûteuses déclenchait uniquement un avertissement.

**Nouveau comportement**

À compter de la version 3.0, EF Core autorise uniquement l’évaluation sur le client des expressions qui figurent dans la projection de niveau supérieur (le dernier appel `Select()` dans la requête).
Quand des expressions d’une autre partie de la requête ne peuvent pas être converties en SQL ou en paramètre, une exception est levée.

**Pourquoi ?**

L’évaluation automatique des requêtes sur le client permet à de nombreuses requêtes d’être exécutées même si certaines de leurs parties importantes ne peuvent pas être traduites.
Ce comportement peut entraîner un comportement inattendu et potentiellement dangereux qui peut devenir évident uniquement en production.
Par exemple, une condition dans un appel `Where()` qui ne peut pas être traduite peut provoquer le transfert de toutes les lignes de la table hors du serveur de base de données, et l’application du filtre sur le client.
Cette situation peut facilement passer inaperçue si la table contient uniquement quelques lignes lors du développement, mais poser davantage de problèmes quand l’application passe en production, où la table peut contenir plusieurs millions de lignes.
Les avertissements relatifs à l’évaluation sur le client étaient également trop faciles à ignorer pendant le développement.

De plus, l’évaluation automatique sur le client peut entraîner des problèmes selon lesquels l’amélioration de la traduction des requêtes pour des expressions spécifiques provoque un changement cassant involontaire entre les versions.

**Atténuations**

Si une requête ne peut pas être entièrement traduite, réécrivez-la sous une forme qui peut être traduite ou utilisez `AsEnumerable()`, `ToList()` ou autre méthode similaire pour réimporter explicitement les données sur le client, où elles peuvent ensuite être traitées à l’aide de LINQ-to-Objects.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core ne fait plus partie du framework partagé ASP.NET Core

[Annonces de suivi de problème n°325](https://github.com/aspnet/Announcements/issues/325)

Ce changement a été introduit dans ASP.NET Core 3.0-preview 1. 

**Ancien comportement**

Avant ASP.NET Core 3.0, quand vous ajoutiez une référence de package à `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, elle incluait EF Core et certains des fournisseurs de données EF Core tels que le fournisseur SQL Server.

**Nouveau comportement**

À compter de la version 3.0, le framework partagé ASP.NET Core n’inclut ni EF Core, ni aucun fournisseur de données EF Core.

**Pourquoi ?**

Avant ce changement, l’obtention d’EF Core nécessitait différentes étapes selon que l’application ciblait ASP.NET Core et SQL Server ou non. De plus, la mise à niveau d’ASP.NET Core forçait la mise à niveau d’EF Core et du fournisseur SQL Server, ce qui n’est pas toujours souhaitable.

Avec ce changement, l’expérience d’obtention d’EF Core est la même pour tous les fournisseurs, implémentations .NET prises en charge et types d’applications.
Les développeurs peuvent désormais contrôler exactement quand EF Core et les fournisseurs de données EF Core sont mis à niveau.

**Atténuations**

Pour utiliser EF Core dans une application ASP.NET Core 3.0 ou toute autre application prise en charge, ajoutez explicitement une référence de package au fournisseur de base de données EF Core que votre application utilisera.

## <a name="query-execution-is-logged-at-debug-level"></a>L’exécution de requête est enregistrée dans le journal au niveau du débogage

[Suivi de problème n°14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, l’exécution des requêtes et autres commandes était enregistrée au niveau `Info`.

**Nouveau comportement**

À compter d’EF Core 3.0, la journalisation de l’exécution des commandes et du SQL s’effectue au niveau `Debug`.

**Pourquoi ?**

Ce changement a été apporté afin de réduire le bruit au niveau de journal `Info`.

**Atténuations**

Cet événement de journalisation est défini par `RelationalEventId.CommandExecuting` avec l’ID d’événement 20100.
Pour reconnecter SQL au niveau `Info`, configurez explicitement le niveau dans `OnConfiguring` ou `AddDbContext`.
Par exemple :
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Les valeurs de clés temporaires ne sont plus définies sur les instances d’entités

[Suivi de problème n°12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Ce changement a été introduit dans EF Core 3.0-preview 2.

**Ancien comportement**

Avant EF Core 3.0, des valeurs temporaires étaient affectées à toutes les propriétés de clé qui auraient plus tard une valeur réelle générée par la base de données.
Généralement, ces valeurs temporaires étaient de grands nombres négatifs.

**Nouveau comportement**

À compter de la version 3.0, EF Core stocke la valeur de clé temporaire dans le cadre des informations de suivi de l’entité, et laisse la propriété de clé proprement dite inchangée.

**Pourquoi ?**

Ce changement a été apporté afin d’empêcher que les valeurs de clés temporaires ne deviennent à tort permanentes quand une entité qui a été suivie par une instance `DbContext` est déplacée vers une autre instance `DbContext`. 

**Atténuations**

Les applications qui affectent des valeurs de clés primaires sur des clés étrangères afin de former des associations entre des entités peuvent dépendre de l’ancien comportement si les clés primaires sont générées par le magasin et appartiennent à des entités à l’état `Added`.
Vous pouvez éviter cela en :
* N’utilisant pas de clés générées par le magasin.
* Définissant des propriétés de navigation afin d’établir des relations au lieu de définir des valeurs de clés étrangères.
* Obtenant les valeurs de clés temporaires réelles à partir des informations de suivi de l’entité.
Par exemple, `context.Entry(blog).Property(e => e.Id).CurrentValue` retournera la valeur temporaire même si la propriété `blog.Id` elle-même n’a pas été définie.

## <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges honore les valeurs de clés générées par le magasin

[Suivi de problème n°14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, une entité non suivie détectée par `DetectChanges` était suivie à l’état `Added` et insérée en tant que nouvelle ligne quand `SaveChanges` était appelée.

**Nouveau comportement**

À compter d’EF Core 3.0, si une entité utilise des valeurs de clés générées et qu’une valeur de clé est définie, alors l’entité est suivie à l’état `Modified`.
Cela signifie qu’une ligne pour l’entité est supposée exister et qu’elle sera mise à jour lors de l’appel à `SaveChanges`.
Si la valeur de clé n’est pas définie, ou si le type d’entité n’utilise pas de clés générées, alors la nouvelle entité sera quand même suivie comme `Added`, comme dans les versions précédentes.

**Pourquoi ?**

Ce changement a été apporté afin de simplifier l’utilisation des graphes d’entités déconnectées lors de l’utilisation de clés générées par le magasin.

**Atténuations**

Ce changement peut casser une application si un type d’entité est configuré pour utiliser des clés générés, mais que les valeurs de clés sont définies explicitement pour les nouvelles instances.
La solution consiste à configurer explicitement les propriétés de clés de façon à ne pas utiliser des valeurs générées.
Par exemple, avec l’API Fluent :

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Ou avec des annotations de données :

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Les suppressions en cascade se produisent désormais immédiatement par défaut

[Suivi de problème n°10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant la version 3.0, les actions en cascade appliquées par EF Core (suppression d’entités dépendantes quand un principal requis est supprimé ou quand la relation à un principal requis est interrompue) ne se produisaient qu’une fois que SaveChanges était appelée.

**Nouveau comportement**

À compter de la version 3.0, EF Core applique les actions en cascade dès que la condition de déclenchement est détectée.
Par exemple, si vous appelez `context.Remove()` pour supprimer une entité principale, toutes les dépendances requises suivies associées seront également définies immédiatement sur `Deleted`.

**Pourquoi ?**

Ce changement a été apporté afin d’améliorer l’expérience de liaison de données et les scénarios d’audit où il est important de comprendre quelles entités seront supprimées _avant_ l’appel à `SaveChanges`.

**Atténuations**

Vous pouvez restaurer le comportement précédent par le biais des paramètres sur `context.ChangedTracker`.
Par exemple :

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>Les types de requêtes sont regroupés avec les types d’entités

[Suivi de problème n°14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, les [types de requêtes](xref:core/modeling/query-types) étaient un moyen d’interroger des données qui ne définissait pas de clé primaire de façon structurée.
Autrement dit, on utilisait un type de requête pour mapper des types d’entités sans clés (le plus souvent à partir d’une vue, mais aussi éventuellement à partir d’une table), alors qu’on utilisait un type d’entité normal quand une clé était disponible (le plus souvent à partir d’une table, mais aussi éventuellement à partir d’une vue).

**Nouveau comportement**

Un type de requête devient maintenant simplement un type d’entité sans clé primaire.
Les types d’entités sans clé ont les mêmes fonctionnalités que les types de requêtes dans les versions précédentes.

**Pourquoi ?**

Ce changement a été apporté afin de réduire la confusion concernant l’objectif des types de requêtes.
Plus précisément, il s’agit de types d’entités sans clé et sont intrinsèquement en lecture seule pour cette raison, mais vous ne devez pas les utiliser au seul motif qu’un type d’entité doit être en lecture seule.
De même, ils sont souvent mappés à des vues, mais c’est uniquement car les vues définissent rarement des clés.

**Atténuations**

Les parties suivantes de l’API sont désormais obsolètes :
* **`ModelBuilder.Query<>()`** - Au lieu de cela, vous devez appeler `ModelBuilder.Entity<>().HasNoKey()` pour marquer un type d’entité comme n’ayant pas de clé.
Cela ne serait toujours pas configuré par convention, afin d’éviter une configuration incorrecte quand une clé primaire est attendue mais ne correspond pas à la convention.
* **`DbQuery<>`** - Utilisez plutôt `DbSet<>`.
* **`DbContext.Query<>()`** - Utilisez plutôt `DbContext.Set<>()`.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>L’API de configuration pour les relations de type détenu a changé

[Suivi de problème n°12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Suivi de problème n°9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Suivi de problème n°14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, la configuration de la relation détenue était effectuée directement après l’appel à `OwnsOne` ou `OwnsMany`. 

**Nouveau comportement**

À compter d’EF Core 3.0, il existe une API Fluent afin de configurer une propriété de navigation pour le propriétaire à l’aide de `WithOwner()`.
Par exemple :

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

La configuration liée à la relation entre le propriétaire et le détenu doit maintenant être chaînée après `WithOwner()` tout comme les autres relations.
En revanche, la configuration du type détenu lui-même serait toujours chaînée après `OwnsOne()/OwnsMany()`.
Par exemple :

```C#
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

**Pourquoi ?**

Ce changement a été apporté afin de créer une distinction plus claire entre la configuration du type détenu lui-même et la _relation au type détenu_.
Cela lève ainsi toute ambiguïté et toute confusion autour des méthodes telles que `HasForeignKey`.

**Atténuations**

Changez la configuration des relations de type détenu de façon à utiliser la nouvelle surface d’API, comme indiqué dans l’exemple ci-dessus.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>La convention de propriété de clé étrangère ne correspond plus au nom de la propriété principale

[Suivi de problème n°13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Considérez le modèle suivant :
```C#
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
Par exemple :

```C#
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

```C#
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

**Pourquoi ?**

Ce changement a été apporté afin d’éviter de définir de manière erronée une propriété de clé primaire sur le type détenu.

**Atténuations**

Si la propriété était censée être la clé étrangère, et par conséquent une partie de la clé primaire, configurez-la explicitement comme telle.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Chaque propriété utilise la génération indépendante de clé de type entier en mémoire

[Suivi de problème n°6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, un seul générateur de valeurs partagées était utilisé pour toutes les propriétés de clé de type entier en mémoire.

**Nouveau comportement**

À compter d’EF Core 3.0, chaque propriété de clé de type entier obtient son propre générateur de valeur lors de l’utilisation de la base de données en mémoire.
De plus, si la base de données est supprimée, la génération de clé est réinitialisée pour toutes les tables.

**Pourquoi ?**

Ce changement a été apporté afin d’aligner plus étroitement la génération de clé en mémoire à la génération de clé de base de données réelle, et afin d’améliorer la capacité à isoler les tests les uns des autres lors de l’utilisation de la base de données en mémoire.

**Atténuations**

Ceci peut casser une application qui repose sur la définition de valeurs de clé en mémoire spécifiques.
Au lieu de cela, ne vous appuyez pas sur des valeurs de clés spécifiques, ou procédez à une mise à jour pour vous aligner au nouveau comportement.

## <a name="backing-fields-are-used-by-default"></a>Les champs de stockage sont utilisés par défaut

[Suivi de problème n°12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Ce changement a été introduit dans EF Core 3.0-preview 2.

**Ancien comportement**

Avant la version 3.0, même si le champ de stockage d’une propriété était connu, par défaut EF Core lisait et écrivait toujours la valeur de propriété à l’aide des méthodes get et set de la propriété.
L’exception à cette règle concernait l’exécution des requêtes, où le champ de stockage était défini directement s’il était connu.

**Nouveau comportement**

À compter d’EF Core 3.0, si le champ de stockage d’une propriété est connu, cette propriété sera toujours lu et écrite à l’aide du champ de stockage.
Cela peut casser une application si celle-ci repose sur un comportement supplémentaire codé dans les méthodes get ou set.

**Pourquoi ?**

Ce changement a été apporté afin d’empêcher EF Core de déclencher par erreur une logique métier par défaut quand vous effectuez des opérations de base de données impliquant les entités.

**Atténuations**

Vous pouvez restaurer le comportement antérieur à la version 3.0 en configurant le mode d’accès à la propriété dans l’API Fluent modelBuilder.
Par exemple :

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Erreur si plusieurs champs de stockage compatibles sont détectés

[Suivi de problème n°12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, si plusieurs champs respectaient les règles de recherche du champ de stockage d’une propriété, l’un d’entre eux était choisi selon un ordre de priorité.
Cela pouvait entraîner l’utilisation d’un champ incorrect en cas d’ambiguïté.

**Nouveau comportement**

À compter d’EF Core 3.0, si plusieurs champs sont mis en correspondance avec la même propriété, une exception est levée.

**Pourquoi ?**

Ce changement a été apporté afin d’éviter d’utiliser en mode silencieux un champ plutôt qu’un autre quand un seul peut être correct.

**Atténuations**

Pour les propriétés comportant des champs de stockage ambigus, le champ à utiliser doit être spécifié explicitement.
Par exemple, à l’aide de l’API Fluent :

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool n’appelle plus AddLogging et AddMemoryCache

[Suivi de problème no 14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, le fait d’appeler `AddDbContext` ou `AddDbContextPool` avait également pour effet d’inscrire les services de journalisation et de mise en cache mémoire auprès de l’injection de dépendances par des appels à [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) et à [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nouveau comportement**

À compter d’EF Core 3.0, `AddDbContext` et `AddDbContextPool` n’inscriront plus ces services auprès de l’injection de dépendances.

**Pourquoi ?**

Il n’est pas nécessaire pour EF Core 3.0 que ces services se trouvent dans le conteneur d’injection de dépendances de l’application. Toutefois, `ILoggerFactory` est utilisé par EF Core même s’il est inscrit dans ce conteneur.

**Atténuations**

Si votre application a besoin de ces services, inscrivez-les explicitement auprès du conteneur d’injection de dépendances avec [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry effectue maintenant un DetectChanges local

[Suivi de problème n°13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, le fait d’appeler `DbContext.Entry` provoquait la détection des changements pour toutes les entités suivies.
Cela garantissait que l’état exposé dans `EntityEntry` était à jour.

**Nouveau comportement**

À compter d’EF Core 3.0, l’appel à `DbContext.Entry` tente uniquement de détecter les changements apportés dans l’entité donnée et dans toute entité principale suivie qui lui est associée.
Cela signifie que les changements apportés ailleurs peuvent ne pas avoir été détectés par l’appel à cette méthode, ce qui pourrait avoir des conséquences sur l’état de l’application.

Notez que si `ChangeTracker.AutoDetectChangesEnabled` a la valeur `false`, même cette détection de changement locale sera désactivée.

Les autres méthodes qui provoquent une détection des changements (par exemple `ChangeTracker.Entries` et `SaveChanges`) entraînent toujours un `DetectChanges` complet de toutes les entités suivies.

**Pourquoi ?**

Ce changement a été apporté afin d’améliorer les performances par défaut de `context.Entry`.

**Atténuations**

Appelez `ChgangeTracker.DetectChanges()` explicitement avant d’appeler `Entry` pour garantir le comportement antérieur à la version 3.0.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Les clés de tableaux d’octets et de chaînes ne sont pas générés par client par défaut

[Suivi de problème n°14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, les propriétés de clés `string` et `byte[]` pouvaient être utilisées sans définir explicitement de valeur non null.
Dans ce cas, la valeur de la clé était générée sur le client en tant que GUID, sérialisé en octets pour `byte[]`.

**Nouveau comportement**

À compter d’EF Core 3.0, une exception est levée pour signaler qu’aucune valeur de clé n’a été définie.

**Pourquoi ?**

Ce changement a été apporté car les valeurs `string`/`byte[]` générées par le client ne sont généralement pas utiles, et le comportement par défaut rendait les valeurs de clés générés de manière courante difficiles à expliquer.

**Atténuations**

Vos pouvez obtenir le comportement antérieur à la version 3.0 en spécifiant explicitement que les propriétés de clés doivent utiliser des valeurs générées si aucune autre valeur non nulle n’est définie.
Par exemple, avec l’API Fluent :

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Ou avec des annotations de données :

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory est désormais un service délimité

[Suivi de problème n°14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, `ILoggerFactory` était inscrit en tant que service singleton.

**Nouveau comportement**

À compter d’EF Core 3.0, `ILoggerFactory` est inscrit en tant que service délimité.

**Pourquoi ?**

Ce changement a été apporté afin d’autoriser l’association d’un journaliseur avec une instance `DbContext`, ce qui active d’autres fonctionnalités et élimine certains cas de comportement pathologique tels que l’explosion des fournisseurs de services internes.

**Atténuations**

Ce changement ne devrait pas avoir d’impact sur le code de l’application, sauf en cas d’inscription et d’utilisation de services personnalisés sur le fournisseur de service interne EF Core,
ce qui n’est pas courant.
Dans ces cas-là, la plupart des composants continueront à fonctionner, mais tout service singleton qui dépendait de `ILoggerFactory` devra être modifié pour obtenir le `ILoggerFactory` d’une manière différente.

Si vous faites face à ce genre de situation, veuillez soumettre un problème par le biais du [suivi des problèmes GitHub EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) pour nous dire comment vous utilisez `ILoggerFactory`, afin que nous puissions mieux comprendre comment ne rien casser à l’avenir.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo fusionnée dans IDbContextOptionsExtension

[Suivi de problème n°13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

`IDbContextOptionsExtensionWithDebugInfo` était une interface facultative supplémentaire étendue à partir de `IDbContextOptionsExtension` pour éviter de créer un changement cassant dans l’interface durant le cycle de mise en production 2.x.

**Nouveau comportement**

Les interfaces sont maintenant fusionnées dans `IDbContextOptionsExtension`.

**Pourquoi ?**

Ce changement a été apporté car les interfaces ne font conceptuellement qu’une.

**Atténuations**

Toute implémentation de `IDbContextOptionsExtension` devra être mise à jour pour prendre en charge le nouveau membre.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Les proxys à chargement différé ne partent plus du principe que les propriétés de navigation sont entièrement chargées

[Suivi de problème n°12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, une fois qu’un `DbContext` était supprimé, il n’existait aucun moyen de savoir si une propriété de navigation donnée sur une entité obtenue à partir de ce contexte était entièrement chargée.
Les serveurs proxy partaient du principe qu’une navigation de référence était chargée si elle avait une valeur non null, et qu’une navigation de collection était chargée si elle n’était pas vide.
Dans ces cas-là, toute tentative de chargement différé constituait une no-op.

**Nouveau comportement**

À compter d’EF Core 3.0, les proxys effectuent le suivi du chargement d’une propriété de navigation.
Cela signifie qu’une tentative d’accès à une propriété de navigation qui est chargée une fois que le contexte a été supprimé sera toujours une no-op, même quand la navigation chargée est vide ou null.
À l’inverse, une tentative d’accès à une propriété de navigation qui n’est pas chargée lèvera une exception si le contexte est supprimé, même si la propriété de navigation est une collection non vide.
Si cette situation se présente, cela signifie que le code d’application tente d’utiliser le chargement différé à un moment non valide, et que l’application doit être modifiée afin de ne pas tenter cette opération.

**Pourquoi ?**

Ce changement a été apporté afin de rendre le comportement cohérent et correct lors de la tentative de chargement différé sur une instance `DbContext` supprimée.

**Atténuations**

Mettez à jour le code d’application pour qu’il ne tente pas d’effectuer un chargement différé avec un contexte supprimé, ou configurez cette opération pour qu’il s’agisse d’une no-op comme décrit dans le message d’exception.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>La création excessive de fournisseurs de services internes est maintenant une erreur par défaut

[Suivi de problème n°10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, un avertissement était enregistré dans le journal quand une application créait une quantité pathologique de fournisseurs de services internes.

**Nouveau comportement**

À compter d’EF Core 3.0, cet avertissement est désormais considéré comme une erreur, et une exception est levée. 

**Pourquoi ?**

Ce changement a été apporté afin d’améliorer le code d’application avec une exposition plus explicite de ce cas pathologique.

**Atténuations**

En présence de cette erreur, le mieux consiste à tenter de comprendre la cause racine, et de cesser de créer autant de fournisseurs de services internes.
Toutefois, vous pouvez reconvertir cette erreur en avertissement (ou l’ignorer) par le biais de la configuration sur le `DbContextOptionsBuilder`.
Par exemple :

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nouveau comportement de HasOne/HasMany appelé avec une chaîne unique

[Suivi de problème no 9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Ce changement sera introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant EF Core 3.0, la façon dont était interprété le code qui appelait `HasOne` ou `HasMany` avec une seule chaîne prêtait à confusion.
Par exemple :
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Le code semble relier `Samurai` à un autre type d’entité avec la propriété de navigation `Entrance`, qui peut être privée.

En réalité, ce code tente de créer une relation avec un certain type d’entité nommé `Entrance` sans propriété de navigation.

**Nouveau comportement**

À compter d’EF Core 3.0, le code ci-dessus effectue maintenant ce qu’il semblait devoir faire avant.

**Pourquoi ?**

L’ancien comportement était très déroutant, en particulier pour qui lisait le code de configuration à la recherche d’erreurs.

**Atténuations**

Seules les applications qui configurent explicitement des relations en utilisant des chaînes comme noms de type sans spécifier explicitement la propriété de navigation sont concernées,
ce qui n’est pas courant.
Pour revenir au comportement précédent, transmettez explicitement `null` comme nom de propriété de navigation.
Par exemple :

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>L’annotation Relational:TypeMapping est désormais simplement TypeMapping

[Suivi de problème n°9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Ce changement a été introduit dans EF Core 3.0-preview 2.

**Ancien comportement**

Le nom d’annotation pour les annotations de mappage de types était « annotations ».

**Nouveau comportement**

Le nom d’annotation pour les annotations de mappage de types est désormais « TypeMapping ».

**Pourquoi ?**

Les mappages de types ne sont désormais plus utilisés uniquement pour les fournisseurs de bases de données relationnelles.

**Atténuations**

Ce changement casse uniquement les applications qui accèdent au mappage de types directement en tant qu’annotation, ce qui n’est pas courant.
La meilleure solution consiste à utiliser la surface d’API pour accéder aux mappages de types, plutôt que d’utiliser directement l’annotation.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable sur un type dérivé lève une exception 

[Suivi de problème n°11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, l’appel à `ToTable()` sur un type dérivé était ignoré, car seule la stratégie de mappage d’héritage était TPH où cela n’est pas valide. 

**Nouveau comportement**

À compter d’EF Core 3.0, et en préparation à l’ajout de la prise en charge de TPT et TPC dans une version ultérieure, l’appel à `ToTable()` sur un type dérivé lève maintenant une exception afin d’éviter tout changement inattendu du mappage à l’avenir.

**Pourquoi ?**

Actuellement le mappage d’un type dérivé à une autre table n’est pas une opération valide.
Ce changement permettra d’éviter toute cassure ultérieure quand cette opération deviendra valide.

**Atténuations**

Supprimez toutes les tentatives de mappage de types dérivés à d’autres tables.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex est remplacé par HasIndex 

[Suivi de problème n°12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offrait un moyen de configurer des colonnes utilisées avec `INCLUDE`.

**Nouveau comportement**

À compter d’EF Core 3.0, l’utilisation de `Include` sur un index est prise en charge au niveau relationnel.
Utilisez `HasIndex().ForSqlServerInclude()`.

**Pourquoi ?**

Ce changement a été apporté afin de consolider l’API pour les index avec `Includes` dans un seul emplacement pour tous les fournisseurs de bases de données.

**Atténuations**

Utilisez la nouvelle API, comme indiqué ci-dessus.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core n’envoie plus de pragma pour l’application de clé étrangère SQLite

[Suivi de problème n°12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Ce changement a été introduit dans EF Core 3.0-preview 3.

**Ancien comportement**

Avant EF Core 3.0, EF Core envoyait `PRAGMA foreign_keys = 1` quand une connexion à SQLite était ouverte.

**Nouveau comportement**

À compter d’EF Core 3.0, EF Core n’envoie plus de `PRAGMA foreign_keys = 1` quand une connexion à SQLite est ouverte.

**Pourquoi ?**

Ce changement a été apporté car EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3` par défaut, ce qui signifie que l’application de clé étrangère est activée par défaut et n’a pas besoin d’être activée explicitement chaque fois qu’une connexion est ouverte.

**Atténuations**

Les clés étrangères sont activées par défaut dans SQLitePCLRaw.bundle_e_sqlite3, qui est utilisé par défaut pour EF Core.
Pour les autres cas, vous pouvez activer les clés étrangères en spécifiant `Foreign Keys=True` dans votre chaîne de connexion.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite dépend désormais de SQLitePCLRaw.bundle_e_sqlite3

**Ancien comportement**

Avant EF Core 3.0, EF Core utilisait `SQLitePCLRaw.bundle_green`.

**Nouveau comportement**

À compter d’EF Core 3.0, EF Core utilise `SQLitePCLRaw.bundle_e_sqlite3`.

**Pourquoi ?**

Ce changement a été apporté afin que la version de SQLite utilisée sur iOS soit cohérente avec d’autres plateformes.

**Atténuations**

Pour utiliser la version de SQLite native sur iOS, configurez `Microsoft.Data.Sqlite` de façon à utiliser un autre bundle `SQLitePCLRaw`.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Les valeurs char sont maintenant stockées au format TEXT sur SQLite

[Suivi de problème no 15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Ce changement a été introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Avant, les valeurs char étaient stockées comme valeurs INTEGER sur SQLite. Par exemple, une valeur char de *A* était stockée comme valeur entière 65.

**Nouveau comportement**

Maintenant, les valeurs char sont stockées au format TEXT.

**Pourquoi ?**

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

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Les ID de migration sont maintenant générés à l’aide du calendrier de la culture invariante

[Suivi de problème no 12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

Ce changement a été introduit dans EF Core 3.0-preview 4.

**Ancien comportement**

Les ID de migration ont été générés par inadvertance à l’aide du calendrier de la culture actuelle.

**Nouveau comportement**

Maintenant, les ID de migration sont toujours générés à l’aide du calendrier de la culture invariante (grégorien).

**Pourquoi ?**

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
