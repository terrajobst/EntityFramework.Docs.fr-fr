---
title: 'Filtres de requête globale : EF Core'
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "42447765"
---
# <a name="global-query-filters"></a>Filtres de requête globale

Les filtres de requête globale permet aux prédicats de requête LINQ (expression booléenne généralement passée à l’opérateur de requête LINQ *Where*) d’être appliqués sur des types d’entité dans le modèle de métadonnées (généralement dans *OnModelCreating*). Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces types d’entités, y compris ceux référencés indirectement, par exemple à l’aide d’Include ou de références de propriété de navigation directe. Voici deux applications courantes de cette fonctionnalité :

* **Suppression réversible** : un type d’entité définit une propriété *IsDeleted*.
* **Architecture multilocataire** : un type d’entité définit une propriété *TenantId*.

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser les filtres de requête globale pour implémenter des comportements de suppression réversible et d’architecture multilocataire dans un modèle de création de blogs simple.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) sur GitHub.

Tout d’abord définissez les entités :

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

Notez la déclaration d’un champ __tenantId_ sur l’entité _Blog_. Celui-ci doit servir à associer chaque instance de blog à un client spécifique. Une propriété _IsDeleted_ est également définie sur le type d’entité _Post_. Elle est utilisée pour déterminer si une instance _Post_ a été « supprimée de façon réversible ». Autrement dit, l’instance est marquée comme supprimée sans que les données sous-jacentes soient réellement supprimées.

Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de l’API ```HasQueryFilter```.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Les expressions de prédicat passées aux appels _HasQueryFilter_ seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.

> [!TIP]
> Notez l’utilisation d’un champ de niveau d’instance DbContext : ```_tenantId``` permet de définir le client en cours. Les filtres au niveau du modèle utilisent la valeur de l’instance de contexte correcte (c’est-à-dire celle qui exécute la requête).

## <a name="disabling-filters"></a>Désactivation des filtres

Les filtres peuvent être désactivés pour des requêtes LINQ individuelles à l’aide de l’opérateur ```IgnoreQueryFilters()```.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitations

Les filtres de requête globale présentent les limitations suivantes :

* Les filtres ne peuvent pas contenir de références à des propriétés de navigation.
* Les filtres ne peuvent être définis que pour le type d’entité racine d’une hiérarchie d’héritage.
