---
title: Index-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416394"
---
# <a name="indexes"></a>Index

Les index sont un concept commun entre de nombreux magasins de données. Bien que leur implémentation dans le magasin de données puisse varier, elles sont utilisées pour rendre les recherches basées sur une colonne (ou un ensemble de colonnes) plus efficaces.

Les index ne peuvent pas être créés à l’aide d’annotations de données. Vous pouvez utiliser l’API Fluent pour spécifier un index sur une seule colonne comme suit :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

Vous pouvez également spécifier un index sur plusieurs colonnes :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> Par Convention, un index est créé dans chaque propriété (ou ensemble de propriétés) utilisée comme clé étrangère.
>
> EF Core ne prend en charge qu’un seul index par jeu de propriétés distinct. Si vous utilisez l’API Fluent pour configurer un index sur un ensemble de propriétés pour lequel un index est déjà défini, soit par Convention, soit par configuration précédente, vous allez modifier la définition de cet index. Cela est utile si vous souhaitez configurer davantage un index qui a été créé par Convention.

## <a name="index-uniqueness"></a>Unicité de l’index

Par défaut, les index ne sont pas uniques : plusieurs lignes peuvent avoir la même valeur (s) pour le jeu de colonnes de l’index. Vous pouvez rendre un index unique comme suit :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

Toute tentative d’insertion de plusieurs entités avec les mêmes valeurs pour le jeu de colonnes de l’index entraîne la levée d’une exception.

## <a name="index-name"></a>Nom d’index

Par Convention, les index créés dans une base de données relationnelle sont nommés `IX_<type name>_<property name>`. Pour les index composites, `<property name>` devient une liste de noms de propriétés séparés par un trait de soulignement.

Vous pouvez utiliser l’API Fluent pour définir le nom de l’index créé dans la base de données :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>Filtre d’index

Certaines bases de données relationnelles vous permettent de spécifier un index filtré ou partiel. Cela vous permet d’indexer uniquement un sous-ensemble des valeurs d’une colonne, en réduisant la taille de l’index et en améliorant l’utilisation des performances et de l’espace disque. Pour plus d’informations sur les index filtrés SQL Server, [consultez la documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).

Vous pouvez utiliser l’API Fluent pour spécifier un filtre sur un index, fourni sous la forme d’une expression SQL :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

Lorsque vous utilisez le SQL Server le fournisseur EF ajoute un filtre `'IS NOT NULL'` pour toutes les colonnes Nullable qui font partie d’un index unique. Pour remplacer cette Convention, vous pouvez fournir une valeur `null`.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Colonnes incluses

Certaines bases de données relationnelles vous permettent de configurer un ensemble de colonnes qui sont incluses dans l’index, mais qui ne font pas partie de sa « clé ». Cela peut améliorer considérablement les performances des requêtes lorsque toutes les colonnes de la requête sont incluses dans l’index en tant que colonnes clés ou non-clés, car la table elle-même n’a pas besoin d’être accessible. Pour plus d’informations sur SQL Server colonnes incluses, [consultez la documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).

Dans l’exemple suivant, la colonne `Url` fait partie de la clé d’index, de sorte que tout filtrage de requête sur cette colonne peut utiliser l’index. En outre, les requêtes qui accèdent uniquement aux colonnes `Title` et `PublishedOn` n’ont pas besoin d’accéder à la table et s’exécutent plus efficacement :

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
