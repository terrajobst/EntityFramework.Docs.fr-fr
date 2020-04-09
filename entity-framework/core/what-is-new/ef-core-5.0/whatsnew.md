---
title: Nouveautés dans EF Core 5.0
description: Aperçu des nouvelles fonctionnalités dans EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634279"
---
# <a name="whats-new-in-ef-core-50"></a>Nouveautés dans EF Core 5.0

EF Core 5.0 est actuellement en développement.
Cette page contiendra un aperçu des modifications intéressantes introduites dans chaque aperçu.

Cette page ne duplique pas le [plan pour EF Core 5.0](plan.md).
Le plan décrit les thèmes généraux de EF Core 5.0, y compris tout ce que nous prévoyons d’inclure avant d’expédier la version finale.

Nous ajouterons des liens d’ici à la documentation officielle telle qu’elle sera publiée.

## <a name="preview-2"></a>Préversion 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Utilisez un attribut CMD pour spécifier un champ de sauvegarde de propriété

Un attribut Cmd peut maintenant être utilisé pour spécifier le champ d’appui d’une propriété.
Cet attribut permet à EF Core d’écrire et de lire à partir du champ d’accompagnement comme cela se produirait normalement, même lorsque le champ de soutien ne peut pas être trouvé automatiquement.
Par exemple :

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

La documentation est suivie par question [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).

### <a name="complete-discriminator-mapping"></a>Cartographie complète des discriminateteurs

EF Core utilise une colonne discriminatrice pour [la cartographie TPH d’une hiérarchie successorale](/ef/core/modeling/inheritance).
Certaines améliorations de performance sont possibles tant que EF Core connaît toutes les valeurs possibles pour le discriminateur.
EF Core 5.0 met maintenant en œuvre ces améliorations.

Par exemple, les versions précédentes d’EF Core généreraient toujours cette SQL pour une requête restituant tous les types dans une hiérarchie :

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

EF Core 5.0 générera désormais ce qui suit lorsqu’une cartographie complète des discriminateurs sera configurée :

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Ce sera le comportement par défaut à partir de l’aperçu 3.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Amélioration des performances dans Microsoft.Data.Sqlite

Nous avons apporté deux améliorations de performance pour SQLIte :

* Récupérer des données binaires et de chaînes avec GetBytes, GetChars et GetTextReader est maintenant plus efficace en faisant usage de SqliteBlob et des flux.
* L’initialisation de SqliteConnection est maintenant paresseuse.

Ces améliorations sont dans le ADO.NET fournisseur Microsoft.Data.Sqlite et donc également améliorer les performances en dehors de EF Core.

## <a name="preview-1"></a>Preview 1

### <a name="simple-logging"></a>Enregistrement simple

Cette fonctionnalité ajoute des `Database.Log` fonctionnalités similaires à celles d’EF6.
C’est-à-dire qu’il fournit un moyen simple d’obtenir des journaux à partir de EF Core sans avoir besoin de configurer n’importe quel type de cadre d’enregistrement externe.

La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentation supplémentaire est suivie par question [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Moyen simple d’obtenir généré SQL

EF Core 5.0 `ToQueryString` introduit la méthode d’extension, qui retournera la SQL que EF Core générera lors de l’exécution d’une requête LINQ.

La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 9 janvier 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

La documentation supplémentaire est suivie par question [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Utilisez un attribut CMD pour indiquer qu’une entité n’a pas de

Un type d’entité peut maintenant être configuré comme n’ayant aucune clé en utilisant la nouvelle `KeylessAttribute`.
Par exemple :

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

La documentation est suivie par question [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>La chaîne de connexion ou de connexion peut être modifiée sur DbContext initialisé

Il est maintenant plus facile de créer une instance DbContext sans aucune connexion ou chaîne de connexion.
En outre, la chaîne de connexion ou de connexion peut maintenant être mutée sur l’instance contextuelle.
Cette fonctionnalité permet au même exemple de contexte de se connecter dynamiquement à différentes bases de données.

La documentation est suivie par question [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxies de suivi des changements

EF Core peut maintenant générer des procurations de temps d’exécution qui implémentent automatiquement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) et [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Ceux-ci signalent ensuite les changements de valeur sur les propriétés de l’entité directement à EF Core, évitant la nécessité de numériser pour les changements.
Cependant, les procurations viennent avec leur propre ensemble de limitations, de sorte qu’ils ne sont pas pour tout le monde.

La documentation est suivie par question [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Vues améliorées de débaillement

Les vues de débogé sont un moyen facile d’examiner les internes de EF Core lors de la débogage des questions.
Une vue de débogé pour le modèle a été mise en œuvre il ya quelque temps.
Pour EF Core 5.0, nous avons rendu la vue modèle plus facile à lire et ajouté une nouvelle vue de débbug pour les entités suivies dans le gestionnaire de l’État.

La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 12 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

La documentation supplémentaire est suivie par question [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Amélioration de la manipulation de la sémantique nulle de base de données

Les bases de données relationnelles traitent généralement NULL comme une valeur inconnue et ne sont donc pas égales à tout autre NULL.
Bien que le CMD traite l’in null comme une valeur définie, qui se compare à l’égalité à tout autre null.
EF Core par défaut traduit les requêtes de sorte qu’ils utilisent la sémantique nulle C.
EF Core 5.0 améliore considérablement l’efficacité de ces traductions.

La documentation est suivie par question [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Propriétés d’indexeur

EF Core 5.0 prend en charge la cartographie des propriétés de l’indexeur C.
Ces propriétés permettent aux entités d’agir comme sacs de propriété où les colonnes sont cartographiées aux propriétés nommées dans le sac.

La documentation est suivie par question [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Génération de contraintes de contrôle pour les cartographies enum

EF Core 5.0 Les migrations peuvent désormais générer des contraintes CHECK pour les cartographies des propriétés enum.
Par exemple :

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

La documentation est suivie par question [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>IsRelational (en)

Une `IsRelational` nouvelle méthode a été ajoutée `IsSqlServer` `IsSqlite`en `IsInMemory`plus de l’existant , , et .
Cette méthode peut être utilisée pour tester si le DbContext utilise n’importe quel fournisseur de base de données relationnelle.
Par exemple :

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

La documentation est suivie par question [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos optimiste concordance avec ETags

Le fournisseur de bases de données Azure Cosmos DB prend désormais en charge la concurrence optimiste à l’aide d’ETags.
Utilisez le constructeur de modèles dans OnModelCreating pour configurer un ETag :

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges lancera `DbUpdateConcurrencyException` alors un conflit de concurrence, qui [peut être manipulé](https://docs.microsoft.com/ef/core/saving/concurrency) pour mettre en œuvre des retries, etc.

La documentation est suivie par question [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Traductions de requête pour plus de constructions DateTime

Les requêtes contenant la nouvelle construction dateTime sont maintenant traduites.

De plus, les fonctions suivantes de SQL Server sont maintenant cartographiées :

* DateDiffWeek (en anglais seulement)
* DateDeparts

Par exemple :

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Traductions de requête pour plus de constructions de tableaux d’byte

Les requêtes utilisant contient, Longueur, SéquenceEqual, etc. sur les propriétés byte[] sont maintenant traduites en SQL.

La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentation supplémentaire est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Traduction de requête pour Reverse

Les requêtes utilisant `Reverse` sont maintenant traduites.
Par exemple :

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Traduction de requête pour les opérateurs bitwise

Les requêtes utilisant des opérateurs bitwise sont maintenant traduites dans plus de cas Par exemple :

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Traduction de requête pour cordes sur Cosmos

Les requêtes qui utilisent les méthodes de chaîne Contient, Démarrez et FinsAveavent sont maintenant traduites lors de l’utilisation du fournisseur Azure Cosmos DB.

La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
