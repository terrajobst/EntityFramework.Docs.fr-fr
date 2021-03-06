---
title: Types d’entité sans clé-EF Core
description: Comment configurer les types d’entité sans clé à l’aide de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 520c9ed93240c05deee36fa527a3757490fd7082
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417315"
---
# <a name="keyless-entity-types"></a>Types d’entité sans clé

> [!NOTE]
> Cette fonctionnalité a été ajoutée à EF Core 2,1 sous le nom des types de requêtes. Dans EF Core 3,0, le concept a été renommé en types d’entité sans clé.

En plus des types d’entités standard, un modèle de EF Core peut contenir des _types d’entité sans_clé, qui peuvent être utilisés pour exécuter des requêtes de base de données sur des données qui ne contiennent pas de valeurs de clés.

## <a name="keyless-entity-types-characteristics"></a>Caractéristiques des types d’entité sans clé

Les types d’entité sans clé prennent en charge un grand nombre des mêmes fonctionnalités de mappage que les types d’entités standard, tels que le mappage d’héritage et les propriétés de navigation. Sur des magasins relationnels, ils peuvent configurer les objets de base de données cible et les colonnes par le biais de méthodes de l’API fluent ou des annotations de données.

Toutefois, ils diffèrent des types d’entités standard en ce qu’ils :

- Impossible d’avoir une clé définie.
- Ne sont jamais suivis pour les modifications apportées à _DbContext_ et, par conséquent, ne sont jamais insérées, mises à jour ou supprimées dans la base de données.
- Ne sont jamais découverts par convention.
- Ne prennent en charge qu’un sous-ensemble de fonctionnalités de mappage de navigation, notamment :
  - Ils ne peuvent jamais agir en tant que la terminaison principale d’une relation.
  - Ils n’ont peut-être pas de navigation vers les entités détenues
  - Ils peuvent uniquement contenir des propriétés de navigation de référence pointant vers des entités normales.
  - Les entités ne peuvent pas contenir de propriétés de navigation pour les types d’entités Keyless.
- Doit être configuré avec `.HasNoKey()` appel de méthode.
- Peut être mappé à une _requête de définition_. Une requête de définition est une requête déclarée dans le modèle qui joue le rôle d’une source de données pour un type d’entité sans clé.

## <a name="usage-scenarios"></a>Scénarios d’usage

Voici quelques-uns des principaux scénarios d’utilisation pour les types d’entité sans clé :

- Servir de type de retour pour les [requêtes SQL brutes](xref:core/querying/raw-sql).
- Mappage à des vues de base de données qui ne contiennent pas de clé primaire.
- Mappage de tables qui n’ont pas d’une clé primaire définie.
- Mappage à des requêtes définies dans le modèle.

## <a name="mapping-to-database-objects"></a>Mappage aux objets de base de données

Le mappage d’un type d’entité clé-inférieur à un objet de base de données s’effectue à l’aide de l’API Fluent `ToTable` ou `ToView`. Du point de vue de EF Core, l’objet de base de données spécifié dans cette méthode est une _vue_, ce qui signifie qu’elle est traitée comme une source de requête en lecture seule et ne peut pas être la cible d’opérations de mise à jour, d’insertion ou de suppression. Toutefois, cela ne signifie pas que l’objet de base de données est réellement requis pour être une vue de base de données. Il peut également s’agir d’une table de base de données qui sera traitée en lecture seule. À l’inverse, pour les types d’entités standard, EF Core suppose qu’un objet de base de données spécifié dans la méthode `ToTable` peut être traité comme une _table_, ce qui signifie qu’il peut être utilisé comme source de requête, mais également ciblé par les opérations Update, DELETE et insert. En fait, vous pouvez spécifier le nom d’une vue de base de données dans `ToTable` et tout doit fonctionner correctement tant que la vue est configurée pour être mise à jour sur la base de données.

> [!NOTE]
> `ToView` suppose que l’objet existe déjà dans la base de données et qu’il n’est pas créé par des migrations.

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser les types d’entités Keyless pour interroger une vue de base de données.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) sur GitHub.

Tout d’abord, nous définissons un modèle simple de Blog et Post :

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Ensuite, nous définissons une vue de base de données simple qui nous permettra d’interroger le nombre de messages associée à chaque blog :

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Ensuite, nous définissons une classe pour contenir le résultat de la vue de base de données :

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Ensuite, vous configurez le type d’entité « sans clé » dans _OnModelCreating_ à l’aide de l’API `HasNoKey`.
Nous utilisons l’API de configuration Fluent pour configurer le mappage pour le type d’entité « sans clé » :

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Ensuite, nous configurons le `DbContext` pour inclure le `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Enfin, nous pouvons interroger la vue de base de données de manière standard :

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Notez que nous avons également défini une propriété de requête au niveau du contexte (DbSet) pour agir en tant que racine pour les requêtes sur ce type.
