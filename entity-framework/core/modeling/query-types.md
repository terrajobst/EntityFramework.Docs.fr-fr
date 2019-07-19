---
title: Types de requêtes - EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330797"
---
# <a name="query-types"></a>Types de requêtes
> [!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.1

En plus des types d’entité, un modèle EF Core peut contenir _types de requête_, qui peut être utilisé pour effectuer des requêtes de base de données sur des données qui n’est pas mappées aux types d’entité.

## <a name="compare-query-types-to-entity-types"></a>Comparer des types de requête aux types d’entité

Types de requêtes sont comme les types d’entités qui elles :

- Peuvent être ajoutées au modèle, soit dans `OnModelCreating` ou via une propriété « set » sur une dérivée _DbContext_.
- Prend en charge la plupart des mêmes fonctionnalités de mappage, comme les propriétés de navigation et de mappage de l’héritage. Sur des magasins relationnels, ils peuvent configurer les objets de base de données cible et les colonnes par le biais de méthodes de l’API fluent ou des annotations de données.

Toutefois, ils sont différents à partir de l’entité types dans ce qu’il :

- Ne nécessitent pas une clé à définir.
- Ne sont jamais suivis pour les modifications sur le _DbContext_ et par conséquent sont jamais insérées, mises à jour ou supprimées sur la base de données.
- Ne sont jamais découverts par convention.
- Prennent uniquement en charge un sous-ensemble des fonctionnalités de mappage de navigation - en particulier :
  - Ils ne peuvent jamais agir en tant que la terminaison principale d’une relation.
  - Ils peuvent contenir uniquement des propriétés de navigation de référence qui pointe vers des entités.
  - Entités ne peut pas contenir les propriétés de navigation aux types de requêtes.
- Sont traitées sur le _ModelBuilder_ à l’aide de la `Query` méthode plutôt que la `Entity` (méthode).
- Sont mappées sur le _DbContext_ via les propriétés de type `DbQuery<T>` au lieu de `DbSet<T>`
- Sont mappées aux objets de base de données en utilisant le `ToView` (méthode), plutôt que `ToTable`.
- Peut être mappé à un _définition de requête_ : une définition de requête est une requête secondaire déclarée dans le modèle qui fait Office de source de données pour un type de requête.

## <a name="usage-scenarios"></a>Scénarios d'utilisation

Les principaux scénarios d’utilisation pour les types de requêtes sont notamment :

- Agissant comme le type de retour pour ad hoc `FromSql()` requêtes.
- Mappage avec les vues de base de données.
- Mappage de tables qui n’ont pas d’une clé primaire définie.
- Mappage à des requêtes définies dans le modèle.

## <a name="mapping-to-database-objects"></a>Mappage aux objets de base de données

Mappage d’un type de requête à un objet de base de données est obtenue à l’aide de la `ToView` API fluent. Du point de vue d’EF Core, l’objet de base de données spécifié dans cette méthode est un _vue_, ce qui signifie qu’il est traité comme une source de la requête en lecture seule et ne peut pas être la cible de mise à jour, insérer ou supprimer des opérations. Toutefois, cela ne signifie pas que l’objet de base de données soit réellement nécessaire pour être une vue de base de données, il peut également être une table de base de données qui est traitée comme étant en lecture seule. À l’inverse, pour les types d’entité, EF Core suppose qu’un objet de base de données spécifiée dans le `ToTable` méthode peut être traitée comme un _table_, ce qui signifie qu’il peut être utilisé en tant que requête source, mais également ciblé par la mise à jour, supprimer et insérer opérations. En fait, vous pouvez spécifier le nom d’une vue de base de données dans `ToTable` et tout devrait fonctionner correctement tant que la vue est configurée pour être mis à jour sur la base de données.

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser le Type de requête pour interroger une vue de base de données.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) sur GitHub.

Tout d’abord, nous définissons un modèle simple de Blog et Post :

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

Ensuite, nous définissons une vue de base de données simple qui nous permettra d’interroger le nombre de messages associée à chaque blog :

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

Ensuite, nous définissons une classe pour contenir le résultat de la vue de base de données :

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

Ensuite, nous configurer le type de requête dans _OnModelCreating_ à l’aide de la `modelBuilder.Query<T>` API.
Nous utilisons des API de configuration fluent standard pour configurer le mappage pour le Type de requête :

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

Ensuite, nous configurons `DbContext` le pour inclure `DbQuery<T>`les éléments suivants:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

Enfin, nous pouvons interroger la vue de base de données de manière standard :

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> Notez que nous avons également défini une propriété de niveau requête (DbQuery) d’agir en tant que racine pour les requêtes sur ce type de contexte.
