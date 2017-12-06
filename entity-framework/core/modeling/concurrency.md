---
title: "Jetons d’accès concurrentiel - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel

Si une propriété est configurée comme un jeton d’accès concurrentiel EF Vérifiez qu’aucun autre utilisateur n’a modifié cette valeur dans la base de données lors de l’enregistrement des modifications apportées à cet enregistrement. EF utilise un modèle d’accès concurrentiel optimiste, ce qui signifie qu’il sera Supposons que la valeur n’a pas changé et essayez d’enregistrer les données, mais s’il détecte que la valeur a été modifiée de lever.

Nous pouvons par exemple vouloir configurer `LastName` sur `Person` un jeton d’accès concurrentiel. Cela signifie que si un utilisateur tente d’enregistrer certaines modifications apportées à un `Person`, mais un autre utilisateur a modifié la `LastName` alors une exception sera levée. Cela peut être souhaitable afin que votre application peut inviter l’utilisateur pour vous assurer de que cet enregistrement représente toujours la même personne réelle avant d’enregistrer leurs modifications.

> [!NOTE]
> Cette page explique comment configurer des jetons d’accès concurrentiel. Consultez [concurrence de la gestion des](../saving/concurrency.md) pour obtenir des exemples d’utilisation de l’accès concurrentiel optimiste dans votre application.

## <a name="how-concurrency-tokens-work-in-ef"></a>Fonctionnement des jetons d’accès concurrentiel dans EF

Magasins de données peuvent appliquer des jetons d’accès concurrentiel en vérifiant que tout enregistrement en cours de mise à jour ou supprimé toujours a la même valeur pour le jeton d’accès concurrentiel qui a été attribué lorsque le contexte de chargée des données à l’origine à partir de la base de données.

Par exemple, bases de données relationnelles y parvenir en incluant le jeton d’accès concurrentiel dans le `WHERE` clause de n’importe quel `UPDATE` ou `DELETE` commandes et en vérifiant le nombre de lignes qui ont été affectés. Si le jeton d’accès concurrentiel correspond toujours à une ligne mettra à jour. Si la valeur dans la base de données a changé, aucune ligne n’est mis à jour.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Conventions

Par convention, les propriétés ne sont jamais configurées en tant que jetons d’accès concurrentiel.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les Annotations de données pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Version de ligne / l’horodatage

Un horodatage est une propriété où une nouvelle valeur est générée par la base de données chaque fois qu’une ligne est insérée ou mise à jour. La propriété est également traitée comme un jeton d’accès concurrentiel. Ainsi, que vous obtiendrez une exception si un autre utilisateur a modifié une ligne que vous essayez de mettre à jour, car il est interrogée pour les données.

Cette opération est le fournisseur de base de données utilisé. Pour SQL Server, timestamp est généralement utilisé sur un *byte []* propriété, qui sera le programme d’installation en tant qu’un *ROWVERSION* colonne dans la base de données.

### <a name="conventions"></a>Conventions

Par convention, les propriétés ne sont jamais configurées en tant que les horodatages.

### <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer une propriété comme un horodatage.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un horodatage.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
