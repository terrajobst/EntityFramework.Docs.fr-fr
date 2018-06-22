---
title: Jetons d’accès concurrentiel - EF Core
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745478"
---
# <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel

> [!NOTE]
> Cette page explique comment configurer des jetons d’accès concurrentiel. Consultez [gère les conflits d’accès concurrentiel](../saving/concurrency.md) pour une explication détaillée du fonctionne du contrôle d’accès concurrentiel sur EF principaux et des exemples illustrant comment gérer les conflits d’accès concurrentiel dans votre application.

Propriétés configurées en tant que jetons d’accès concurrentiel sont utilisées pour implémenter un contrôle d’accès concurrentiel optimiste.

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
