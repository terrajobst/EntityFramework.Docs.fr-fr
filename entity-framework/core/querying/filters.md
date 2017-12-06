---
title: "Filtres de requête globale - EF Core"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Filtres de requête globale

Les filtres de requête globale sont des prédicats de requête LINQ (une expression booléenne est généralement passé dans le LINQ *où* opérateur de requête) appliqué aux Types d’entités dans le modèle de métadonnées (généralement dans *OnModelCreating*). Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces Types d’entités, y compris les références de propriété de navigation référencée indirectement, par exemple via l’utilisation d’inclure ou direct de Types d’entités. Certaines applications courantes de cette fonctionnalité sont :

* **La suppression réversible** -un Type d’entité définit une *IsDeleted* propriété.
* **Une architecture mutualisée** -un Type d’entité définit une *TenantId* propriété.

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser des filtres de requête globale pour implémenter des comportements de requête soft-suppression et d’une architecture mutualisée dans un modèle de création de blogs simple.

> [!TIP]
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) sur GitHub.

Tout d’abord définir les entités :

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Notez la déclaration d’un __tenantId_ champ sur la _Blog_ entité. Celui-ci doit servir à associer chaque instance de Blog à un client spécifique. Également défini est un _IsDeleted_ propriété sur le _Post_ type d’entité. Il est utilisé pour effectuer le suivi de l’un _Post_ instance a été « supprimé ». Par exemple L’instance est marquée comme supprimée withouth supprimer physiquement les données sous-jacentes.

Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de la ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Les expressions de prédicat passé à la _HasQueryFilter_ appels seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.

> [!TIP]
> Notez l’utilisation d’un champ de niveau instance DbContext : ```_tenantId``` permet de définir le client en cours. Les filtres au niveau du modèle utilise la valeur de l’instance de contexte correct. Par exemple L’instance qui exécute la requête.

## <a name="disabling-filters"></a>La désactivation des filtres

Les filtres peuvent être désactivées pour les requêtes LINQ à l’aide de la ```IgnoreQueryFilters()``` opérateur.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitations

Les filtres de requête globale présentent les limitations suivantes :

* Les filtres ne peut pas contenir de références à des propriétés de navigation.
* Filtres peuvent être définis uniquement pour la racine du Type d’entité d’une hiérarchie d’héritage.
