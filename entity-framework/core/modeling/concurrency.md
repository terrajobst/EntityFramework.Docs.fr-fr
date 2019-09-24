---
title: Jetons d’accès concurrentiel-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197454"
---
# <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel

> [!NOTE]
> Cette page décrit comment configurer des jetons d’accès concurrentiel. Consultez [gestion des conflits d’accès concurrentiel](../saving/concurrency.md) pour obtenir une explication détaillée du fonctionnement du contrôle d’accès concurrentiel sur EF Core et des exemples montrant comment gérer les conflits d’accès concurrentiel dans votre application.

Les propriétés configurées en tant que jetons d’accès concurrentiel permettent d’implémenter le contrôle d’accès concurrentiel optimiste.

## <a name="conventions"></a>Conventions

Par Convention, les propriétés ne sont jamais configurées comme des jetons d’accès concurrentiel.

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les annotations de données pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un jeton d’accès concurrentiel.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Horodateur/version de ligne

Un horodatage est une propriété où une nouvelle valeur est générée par la base de données chaque fois qu’une ligne est insérée ou mise à jour. La propriété est également traitée comme un jeton d’accès concurrentiel. Cela garantit que vous obtiendrez une exception si quiconque a modifié une ligne que vous essayez de mettre à jour depuis que vous avez interrogé les données.

La façon dont cela est accompli est le fournisseur de base de données qui est utilisé. Par SQL Server, timestamp est généralement utilisé sur une propriété *Byte []* , qui est configurée en tant que colonne *rowversion* dans la base de données.

### <a name="conventions"></a>Conventions

Par Convention, les propriétés ne sont jamais configurées comme horodateurs.

### <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser des annotations de données pour configurer une propriété en tant qu’horodateur.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une propriété en tant qu’horodateur.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
