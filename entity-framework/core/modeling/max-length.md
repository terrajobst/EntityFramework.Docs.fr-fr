---
title: Longueur maximale-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197226"
---
# <a name="maximum-length"></a>Longueur maximale

La configuration d’une longueur maximale fournit une indication au magasin de données sur le type de données approprié à utiliser pour une propriété donnée. La longueur maximale s’applique uniquement aux types de données de `string` tableau `byte[]`, tels que et.

> [!NOTE]  
> Entity Framework n’effectue aucune validation de longueur maximale avant de transmettre des données au fournisseur. Il revient au fournisseur ou au magasin de données de valider le cas échéant. Par exemple, lorsque vous ciblez SQL Server, le dépassement de la longueur maximale entraîne une exception, car le type de données de la colonne sous-jacente n’autorise pas le stockage des données excédentaires.

## <a name="conventions"></a>Conventions

Par Convention, il est laissé au fournisseur de base de données de choisir un type de données approprié pour les propriétés. Pour les propriétés qui ont une longueur, le fournisseur de base de données choisit généralement un type de données qui autorise la longueur de données la plus longue. Par exemple, Microsoft SQL Server utilise `nvarchar(max)` pour `string` les propriétés (ou `nvarchar(450)` si la colonne est utilisée comme clé).

## <a name="data-annotations"></a>Annotations de données

Vous pouvez utiliser les annotations de données pour configurer une longueur maximale pour une propriété. Dans cet exemple, le ciblage SQL Server cela entraînerait l' `nvarchar(500)` utilisation du type de données.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer une longueur maximale pour une propriété. Dans cet exemple, le ciblage SQL Server cela entraînerait l' `nvarchar(500)` utilisation du type de données.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
