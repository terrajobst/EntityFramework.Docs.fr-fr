---
title: Nouvelles fonctionnalités d’EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: a71aa01e81d9830d7b9e6cb01c200851100a15df
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628429"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nouvelles fonctionnalités incluses dans EF Core 3.0 (actuellement en préversion)

> [!IMPORTANT]
> Notez que les fonctionnalités et les plannings des versions ultérieures sont susceptibles de changer à tout moment, et même si cette page est régulièrement mise à jour, elle risque de ne pas toujours refléter nos projets les plus récents.

La liste suivante présente les nouvelles fonctionnalités majeures planifiées pour EF Core 3.0.
La plupart de ces fonctionnalités ne sont pas dans la préversion actuelle, mais seront disponibles à mesure que nous progresserons vers la version RTM.

La raison est que nous nous concentrons d’abord sur l’implémentation des [changements cassants](xref:core/what-is-new/ef-core-3.0/breaking-changes) planifiés.
La plupart de ces changements cassants sont des améliorations apportées à EF Core.
Beaucoup d’autres sont nécessaires pour révéler d’autres améliorations. 

Pour une liste complète des corrections de bogues et des améliorations en cours de développement, vous pouvez visualiser [cette requête dans notre suivi des problèmes](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Améliorations LINQ 

[Suivi de problème n°12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.

LINQ permet d’écrire des requêtes de base de données sans abandonner son langage de prédilection, en tirant parti de riches informations de type pour bénéficier d’IntelliSense et du contrôle de type au moment de la compilation.
Cependant, LINQ offre également la possibilité d’écrire un nombre illimité de requêtes complexes, ce qui a toujours représenté un défi de taille pour les fournisseurs LINQ.
Dans les premières versions d’EF Core, nous avons résolu ce problème d’une part en identifiant les parties traduisibles en SQL d’une requête, et d’autre part en exécutant le reste de la requête en mémoire sur le client.
Si l’exécution côté client est souhaitable dans certaines situations, elle peut aboutir dans la majorité des cas à des requêtes inefficaces, non identifiées comme telles avant le déploiement en production de l’application.
Dans EF Core 3.0, nous avons l’intention de changer en profondeur le fonctionnement de notre implémentation LINQ et nos modes de test.
L’objectif est multiple : la rendre plus robuste (par exemple, pour éviter d’interrompre les requêtes dans les versions de correctif), traduire correctement plus d’expressions en SQL, générer des requêtes efficaces dans davantage de cas et empêcher celles qui ne le sont pas de passer inaperçues.

## <a name="cosmos-db-support"></a>Prise en charge de Cosmos DB 

[Suivi de problème n°8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Cette fonctionnalité est dans la préversion actuelle, mais n’est pas encore finalisée. 

Nous travaillons sur un fournisseur Cosmos DB pour EF Core, pour permettre aux développeurs habitués au modèle de programmation EF de cibler facilement Azure Cosmos DB comme base de données d’application.
L’objectif est de rendre certains des avantages de Cosmos DB (par exemple la distribution mondiale, la disponibilité « Always On », la scalabilité élastique et la faible latence) encore plus accessibles aux développeurs .NET.
Le fournisseur proposera la plupart des fonctionnalités d’EF Core, comme le suivi automatique des modifications, les conversions de valeurs et LINQ, sur l’API SQL dans Cosmos DB.
Nous avons commencé ce travail avant EF Core 2.2, et [nous avons publié quelques préversions du fournisseur](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Nous prévoyons de continuer à développer le fournisseur en parallèle d’EF Core 3.0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Les entités dépendantes qui partagent la table avec le principal sont maintenant facultatives

[Suivi du problème no 9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Cette fonctionnalité sera introduite dans EF Core 3.0-preview 4.

Considérez le modèle suivant :
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

À partir d’EF Core 3.0, si `OrderDetails` est détenu par `Order` ou explicitement mappé à la même table, il est possible d’ajouter un `Order` sans `OrderDetails` et toutes les propriétés `OrderDetails` à l’exception de la clé primaire sont mappées à des colonnes de type nullable.

Pendant l’interrogation, EF Core définit `OrderDetails` sur `null` si une de ses propriétés obligatoires n’a pas de valeur, ou s’il n’a pas de propriété obligatoire en plus de la clé primaire et que toutes les propriétés sont `null`.

## <a name="c-80-support"></a>Prise en charge de C# 8.0

[Suivi de problème n°12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Suivi de problème n°10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle.

Nous voulons que nos clients bénéficient avec EF Core d’une partie des [nouvelles fonctionnalités de C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) comme les flux asynchrones (notamment `await foreach`) et les types référence nullables.

## <a name="reverse-engineering-of-database-views"></a>Ingénierie à rebours des vues de base de données

[Suivi de problème n°1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Cette fonctionnalité n’est pas dans la préversion actuelle.

Les [types de requêtes](xref:core/modeling/query-types), introduits dans EF Core 2.1 et considérés comme des types d’entités sans clé dans EF Core 3.0, représentent des données qui peuvent être lues à partir de la base de données mais ne peuvent pas être mises à jour.
Cette caractéristique fait d’eux un excellent choix pour les vues de base de données dans la plupart des scénarios. Nous envisageons donc d’automatiser la création des types d’entités sans clé lors de l’ingénierie à rebours des vues de bases de données.

## <a name="property-bag-entities"></a>Entités conteneurs des propriétés

[Suivi de problème n°13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) et [n°9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle. 

cette fonctionnalité permet de créer des entités qui stockent les données dans des propriétés indexées au lieu de propriétés standard, et d’utiliser des instances de la même classe .NET (potentiellement aussi simple que `Dictionary<string, object>`) pour représenter différents types d’entités dans le même modèle EF Core.
Cette fonctionnalité est un tremplin pour prendre en charge les relations plusieurs-à-plusieurs sans entité de jointure ([problème no 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)) – l’une des améliorations les plus demandées pour EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 sur .NET Core

[Suivi de problème EF6 n°271](https://github.com/aspnet/EntityFramework6/issues/271)

Le travail sur cette fonctionnalité a commencé, mais elle n’est pas dans la préversion actuelle. 

nous sommes conscients du fait que de nombreuses applications utilisent d’anciennes versions d’EF et qu’une migration vers EF Core dans l’unique objectif de tirer parti de .NET Core représente parfois un effort considérable.
C’est pourquoi nous adapterons la prochaine version d’EF 6 de façon à ce qu’elle s’exécute sur .NET Core 3.0.
L’objectif est de faciliter le portage d’applications avec aussi peu de modifications que possible.
Il va y avoir certaines limitations. Par exemple :
- Les nouveaux fournisseurs devront fonctionner avec les autres bases de données, en plus de la prise en charge de SQL Server incluse sur .NET Core.
- La prise en charge spatiale avec SQL Server ne sera pas activée.

Notez également qu’il n’y a pas de nouvelles fonctionnalités planifiées pour EF 6 à ce stade.
