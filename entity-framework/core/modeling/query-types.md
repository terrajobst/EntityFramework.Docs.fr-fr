---
title: Types de requêtes - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="query-types"></a>Types de requêtes
> [!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.1

En plus des types d’entité peut contenir un modèle EF Core _types de requête_, qui peut être utilisé pour effectuer des requêtes de base de données sur des données qui n’est pas mappées aux types d’entités.

Types de requêtes présentent de nombreuses similitudes avec les types d’entités :

- Ils peuvent également être ajoutés au modèle soit dans `OnModelCreating`, ou via une propriété « set » sur une dérivée _DbContext_.
- Ils prennent en charge plusieurs capacités de mappage même, comme l’héritage de mappage, les propriétés de navigation (voir limitations ci-dessous), puis, dans des magasins relationnels, la possibilité de configurer les objets de base de données cible et les colonnes via les méthodes de l’API fluent ou de l’annotation de données.

Toutefois ils sont différents d’entité types dans ce qu’il :

- Ne nécessitent pas une clé à définir.
- Ne sont jamais suivies pour les modifications sur le _DbContext_ et par conséquent sont jamais insérées, mises à jour ou de suppression sur la base de données.
- Ne sont jamais détectés par convention.
- Prennent uniquement en charge un sous-ensemble des fonctionnalités de mappage de navigation - plus précisément :
  - Ils ne peuvent jamais agir en tant que l’extrémité principale d’une relation.
  - Ils ne peuvent contenir que des propriétés de navigation de référence en pointant sur les entités.
  - Entités ne peut pas contenir de propriétés de navigation pour les types de requêtes.
- Sont traitées sur le _ModelBuilder_ à l’aide de la `Query` méthode plutôt que la `Entity` (méthode).
- Sont mappées sur le _DbContext_ via les propriétés de type `DbQuery<T>` au lieu de `DbSet<T>`
- Sont mappées à des objets de base de données à l’aide de la `ToView` méthode, plutôt que `ToTable`.
- Peut être mappé à un _définition de requête_ - une définition de requête est une requête secondaire déclarée dans le modèle qui fait Office de source de données pour un type de requête.

Parmi les principaux scénarios d’utilisation pour les types de requêtes, citons :

- Agissant en tant que type de retour pour ad hoc `FromSql()` requêtes.
- Mappage de vues de base de données.
- Mappage des tables qui n’ont pas de clé primaire définie.
- Mappage pour les requêtes définies dans le modèle.

> [!TIP]
> Mappage d’un type de requête à un objet de base de données est obtenue grâce à la `ToView` API fluent. Du point de vue du noyau de EF, l’objet de base de données spécifiée dans cette méthode est un _vue_, c'est-à-dire qu’il est traité comme une source de la requête en lecture seule et ne peut pas être la cible d’une mise à jour, insérer ou supprimer les opérations. Toutefois, cela ne signifie pas que l’objet de base de données est réellement nécessaire pour la vue de base de données, il peut également être une table de base de données qui sera traitée comme étant en lecture seule. À l’inverse, pour les types d’entité, EF Core suppose qu’un objet de base de données spécifié dans le `ToTable` méthode peut être traitée comme un _table_, ce qui signifie qu’il peut être utilisé en tant que requête source, mais également ciblés par la mise à jour, supprimer et insérer opérations. En fait, vous pouvez spécifier le nom d’une vue de base de données dans `ToTable` et tout devrait fonctionner correctement tant que la vue est configurée pour être mis à jour sur la base de données.

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser le Type de requête pour interroger une vue de base de données.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) sur GitHub.

Tout d’abord, nous définissons un modèle simple de Blog et Post :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Ensuite, nous définissons une vue de base de données simple qui permet d’interroger le nombre de messages associée à chaque blog :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Ensuite, nous définissons une classe pour contenir le résultat de la vue de base de données :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Ensuite, nous configurons le type de requête dans _OnModelCreating_ à l’aide de la `modelBuilder.Query<T>` API.
API de configuration fluent standard nous permet de configurer le mappage pour le Type de requête :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Enfin, nous pouvons interrogez la vue de base de données de la façon standard :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Notez que nous avons également de définir une propriété de niveau de requête (DbQuery) d’agir en tant que racine pour les requêtes sur ce type de contexte.
