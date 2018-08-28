---
title: Jetons d’accès concurrentiel - EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994224"
---
# <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel

> [!NOTE]
> Cette page décrit comment configurer des jetons d’accès concurrentiel. Consultez [gère les conflits d’accès concurrentiel](../saving/concurrency.md) pour une explication détaillée du fonctionne du contrôle d’accès concurrentiel dans EF Core et des exemples illustrant comment gérer les conflits d’accès concurrentiel dans votre application.

Propriétés configurées en tant que jetons d’accès concurrentiel sont utilisées pour implémenter le contrôle d’accès concurrentiel optimiste.

## <a name="conventions"></a>Conventions

Par convention, les propriétés ne sont jamais configurées en tant que jetons d’accès concurrentiel.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les Annotations de données pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Version de l’horodatage/ligne

Un horodatage est une propriété où une nouvelle valeur est générée par la base de données chaque fois qu’une ligne est insérée ou mise à jour. La propriété est également traitée comme un jeton d’accès concurrentiel. Ainsi, que vous obtiendrez une exception si quelqu'un d’autre a modifié une ligne que vous essayez de mettre à jour dans la mesure où vous l’interrogé pour les données.

Cette opération revient au fournisseur de base de données utilisé. Pour SQL Server, timestamp est généralement utilisé sur un *byte []* propriété, qui sera le programme d’installation comme un *ROWVERSION* colonne dans la base de données.

### <a name="conventions"></a>Conventions

Par convention, les propriétés ne sont jamais configurées en tant qu’horodatages.

### <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des Annotations de données pour configurer une propriété comme un horodatage.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un horodatage.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
