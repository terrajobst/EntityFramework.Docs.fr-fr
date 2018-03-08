---
title: "Types de requêtes - EF Core"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a>Types de requêtes
> [!NOTE]
> Cette fonctionnalité est une nouveauté dans EF Core 2.1

Types de requêtes sont des types de résultats de requête en lecture seule qui peuvent être ajoutés au modèle EF principal. Types de requêtes activer l’interrogation d’ad hoc (par exemple, les types anonymes), mais sont plus flexibles, car ils peuvent avoir la configuration de mappage spécifiée.

Ils sont conceptuellement semblables aux Types d’entités qui :

- Ils sont des types POCO c# qui sont ajoutés au modèle, soit dans ```OnModelCreating``` à l’aide de la ```ModelBuilder.Query``` (méthode), ou via une propriété DbContext « set » (pour les types de requête une telle propriété est typée en tant que ```DbQuery<T>``` plutôt que ```DbSet<T>```).
- Ils prennent en charge une grande partie des mêmes fonctionnalités de mappage en tant que types d’entité normale. Par exemple, mappage d’héritage navigations (voir limitiations ci-dessous) et, sur des magasins relationnels, la possibilité de configurer les objets de schéma de base de données cible via ```ToTable```, ```HasColumn``` les méthodes api fluent (ou des annotations de données).

Types de requêtes sont différents d’entité types dans ce qu’ils :

- Ne nécessitent pas une clé à définir.
- Ne sont jamais suivies par le dispositif de suivi des modifications.
- Ne sont jamais détectés par convention.
- Prennent en charge uniquement un sous-ensemble des fonctionnalités de mappage de navigation - spécifiquement, ils ne peuvent jamais agir en tant que l’extrémité principale d’une relation.
- Peut être mappé à un _définition de requête_ -une requête est une requête secondaire qui fait Office de source de données pour un Type de requête.

Parmi les principaux scénarios d’utilisation pour les types de requêtes, citons :

- Mappage de vues de base de données.
- Mappage des tables qui n’ont pas de clé primaire définie.
- Agissant en tant que type de retour pour ad hoc ```FromSql()``` requêtes.
- Mappage pour les requêtes définies dans le modèle.

> [!TIP]
> Mappage d’un type de requête à une vue de base de données est obtenue grâce à la ```ToTable``` API fluent.

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

Ensuite, nous configurons le type de requête dans _OnModelCreating_ à l’aide de la ```modelBuilder.Query<T>``` API.
API de configuration fluent standard nous permet de configurer le mappage pour le Type de requête :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Enfin, nous pouvons interrogez la vue de base de données de la façon standard :

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Notez que nous avons également de définir une propriété de niveau de requête (DbQuery) d’agir en tant que racine pour les requêtes sur ce type de contexte.
