---
title: Jetons d’accès concurrentiel-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781142"
---
# <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel

> [!NOTE]
> Cette page décrit comment configurer des jetons d’accès concurrentiel. Consultez [gestion des conflits d’accès concurrentiel](../saving/concurrency.md) pour obtenir une explication détaillée du fonctionnement du contrôle d’accès concurrentiel sur EF Core et des exemples montrant comment gérer les conflits d’accès concurrentiel dans votre application.

Les propriétés configurées en tant que jetons d’accès concurrentiel permettent d’implémenter le contrôle d’accès concurrentiel optimiste.

## <a name="configuration"></a>Configuration

### <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Horodateur/rowversion

Un timestamp/rowversion est une propriété pour laquelle une nouvelle valeur est générée automatiquement par la base de données chaque fois qu’une ligne est insérée ou mise à jour. La propriété est également traitée comme un jeton d’accès concurrentiel, s’assurant que vous recevez une exception si une ligne que vous mettez à jour a changé depuis que vous l’avez interrogée. Les détails précis dépendent du fournisseur de base de données utilisé ; par SQL Server, une propriété *Byte []* est généralement utilisée, qui sera configurée en tant que colonne *rowversion* dans la base de données.

Vous pouvez configurer une propriété pour qu’elle soit de type timestamp/rowversion comme suit :

### <a name="data-annotationstabdata-annotations"></a>[Annotations de données](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[ ! code-CSharp [main] (.. /.. /.. /samples/core/Modeling/FluentAPI/Timestamp.cs ? Name = horodatage & en surbrillance = 9, 17]

***
