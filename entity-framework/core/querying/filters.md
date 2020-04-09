---
title: 'Filtres de requête globale : EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417728"
---
# <a name="global-query-filters"></a>Filtres de requête globale

> [!NOTE]
> Cette fonctionnalité date de la publication d’EF Core 2.0.

Les filtres de requête globale permet aux prédicats de requête LINQ (expression booléenne généralement passée à l’opérateur de requête LINQ *Where*) d’être appliqués sur des types d’entité dans le modèle de métadonnées (généralement dans *OnModelCreating*). Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces types d’entités, y compris ceux référencés indirectement, par exemple à l’aide d’Include ou de références de propriété de navigation directe. Voici deux applications courantes de cette fonctionnalité :

* **Suppression réversible** : un type d’entité définit une propriété *IsDeleted*.
* **Multi-location** - Un type d’entité définit une propriété *TenantId.*

## <a name="example"></a>Exemple

L’exemple suivant montre comment utiliser les filtres de requête globale pour implémenter des comportements de suppression réversible et d’architecture multilocataire dans un modèle de création de blogs simple.

> [!TIP]
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) sur GitHub.

Tout d’abord définissez les entités :

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Notez la déclaration d’un champ _tenantId_ sur l’entité _Blog_. Celui-ci doit servir à associer chaque instance de blog à un client spécifique. Une propriété _IsDeleted_ est également définie sur le type d’entité _Post_. Elle est utilisée pour déterminer si une instance _Post_ a été « supprimée de façon réversible ». Autrement dit, l’instance est marquée comme supprimée sans que les données sous-jacentes soient réellement supprimées.

Ensuite, configurez les filtres de requête dans _OnModelCreating_ à l’aide de l’API `HasQueryFilter`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Les expressions de prédicat passées aux appels _HasQueryFilter_ seront désormais automatiquement appliquées à toutes les requêtes LINQ pour ces types.

> [!TIP]
> Notez l’utilisation d’un champ de niveau d’instance DbContext : `_tenantId` permet de définir le client en cours. Les filtres au niveau du modèle utilisent la valeur de l’instance de contexte correcte (c’est-à-dire celle qui exécute la requête).

> [!NOTE]
> Il n’est actuellement pas possible de définir plusieurs filtres de requête sur la même entité - seul le dernier sera appliqué. Cependant, vous pouvez définir un filtre unique avec plusieurs conditions à l’aide de l’opérateur logique _ET_ [ `&&` (en C](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Désactivation des filtres

Les filtres peuvent être désactivés pour des requêtes LINQ individuelles à l’aide de l’opérateur `IgnoreQueryFilters()`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limites

Les filtres de requête globale présentent les limitations suivantes :

* Les filtres ne peuvent être définis que pour le type d’entité racine d’une hiérarchie d’héritage.
