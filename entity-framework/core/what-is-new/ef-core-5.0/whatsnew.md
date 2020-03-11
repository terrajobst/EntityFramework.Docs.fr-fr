---
title: Nouveautés de EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417965"
---
# <a name="whats-new-in-ef-core-50"></a>Nouveautés de EF Core 5,0

EF Core 5,0 est actuellement en cours de développement.
Cette page contient une vue d’ensemble des changements intéressants introduits dans chaque version préliminaire.
Le premier aperçu de EF Core 5,0 est provisoirement attendu dans le premier trimestre de 2020.

Cette page ne duplique pas le [plan pour EF Core 5,0](plan.md).
Le plan décrit les thèmes globaux pour EF Core 5,0, y compris tout ce que nous envisageons d’inclure avant d’expédier la version finale.

Nous ajouterons des liens à partir d’ici à la documentation officielle au fur et à mesure de sa publication.

## <a name="preview-1-not-yet-shipped"></a>Version préliminaire 1 (pas encore expédiée)

### <a name="simple-logging"></a>Journalisation simple

Cette fonctionnalité ajoute des fonctionnalités similaires à celles de `Database.Log` dans EF6.
Autrement dit, il offre un moyen simple d’obtenir des journaux à partir de EF Core sans avoir besoin de configurer tout type d’infrastructure de journalisation externe.

La documentation préliminaire est incluse dans l' [État EF hebdomadaire du 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Une documentation supplémentaire est suivie par le [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)du problème.

### <a name="simple-way-to-get-generated-sql"></a>Moyen simple d’utiliser le SQL généré

EF Core 5,0 introduit la méthode d’extension `ToQueryString` qui retourne le SQL que EF Core générera lors de l’exécution d’une requête LINQ.

La documentation préliminaire est incluse dans l' [État EF Weekly pour le 9 janvier 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Une documentation supplémentaire est suivie par le [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)du problème.

### <a name="enhanced-debug-views"></a>Vues de débogage améliorées

Les vues de débogage sont un moyen simple de consulter les éléments internes d’EF Core lors du débogage des problèmes.
Une vue de débogage a été implémentée pour le modèle il y a quelque temps.
Pour EF Core 5,0, nous avons facilité la lecture et l’ajout d’une nouvelle vue de débogage pour les entités suivies dans le gestionnaire d’État.

La documentation préliminaire est incluse dans l' [État EF Weekly pour le 12 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Une documentation supplémentaire est suivie par le [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)du problème.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>La connexion ou la chaîne de connexion peut être modifiée lors de l’DbContext initialisé

Il est désormais plus facile de créer une instance DbContext sans connexion ou chaîne de connexion.
En outre, la connexion ou la chaîne de connexion peut désormais être mutée sur l’instance de contexte.
Cela permet à la même instance de contexte de se connecter de manière dynamique à différentes bases de données.

La documentation est suivie d’un problème [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxys de suivi des modifications

EF Core pouvez désormais générer des proxies d’exécution qui implémentent automatiquement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) et [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Celles-ci rerapportent ensuite les modifications de valeur sur les propriétés d’entité directement à EF Core, évitant ainsi la nécessité d’analyser les modifications.
Toutefois, les proxies sont fournis avec leur propre ensemble de limitations, donc ils ne le sont pas pour tout le monde.

La documentation est suivie d’un problème [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="improved-handling-of-database-null-semantics"></a>Amélioration de la gestion de la sémantique null de la base de données

En général, les bases de données relationnelles traitent la valeur NULL comme une valeur inconnue et, par conséquent, n’est pas égale à une autre valeur NULL.
C#, en revanche, traite la valeur NULL comme une valeur définie qui est comparée à toute autre valeur null.
EF Core par défaut traduit les requêtes afin qu’elles utilisent C# la sémantique null.
EF Core 5,0 améliore considérablement l’efficacité de ces traductions.

La documentation est suivie d’un problème [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Propriétés de l’indexeur

EF Core 5,0 prend en charge C# le mappage des propriétés de l’indexeur.
Cela permet aux entités d’agir en tant que conteneurs de propriétés où les colonnes sont mappées à des propriétés nommées dans le conteneur.

La documentation est suivie d’un problème [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Génération de contraintes de validation pour les mappages d’énumération

EF Core migrations 5,0 peut désormais générer des contraintes de validation pour les mappages de propriété d’énumération.
Par exemple :

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

La documentation est suivie d’un problème [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="query-translations-for-more-datetime-constructs"></a>Traductions de requêtes pour d’autres constructions DateTime

Les requêtes contenant la nouvelle construction DataTime sont désormais traduites.
En outre, la fonction SQL Server DateDiffWeek est maintenant mappée.

La documentation est suivie d’un problème [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Traductions de requêtes pour d’autres constructions de tableau d’octets

Les requêtes utilisant les propriétés Contains, Length, SequenceEqual, etc. sur Byte [] sont désormais traduites en SQL.

La documentation préliminaire est incluse dans l' [État EF hebdomadaire du 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Une documentation supplémentaire est suivie par le [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)du problème.

### <a name="query-translation-for-reverse"></a>Traduction de requête pour l’inverse

Les requêtes qui utilisent des `Reverse` sont désormais traduites.
Par exemple :

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

La documentation est suivie d’un problème [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Traduction des requêtes pour les opérateurs au niveau du bit

Les requêtes utilisant des opérateurs de bits sont maintenant traduites dans d’autres cas, par exemple :

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

La documentation est suivie d’un problème [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Traduction des requêtes pour les chaînes sur Cosmos

Les requêtes qui utilisent les méthodes de chaîne Contains, StartsWith et EndsWith sont désormais traduites lors de l’utilisation du fournisseur Azure Cosmos DB.

La documentation est suivie d’un problème [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
