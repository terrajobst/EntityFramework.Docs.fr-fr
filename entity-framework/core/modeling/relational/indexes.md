---
title: Indexes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678997"
---
# <a name="indexes"></a>Index

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Un index dans une base de données relationnelle est mappé au même concept en tant qu’index dans le cœur d’Entity Framework.

## <a name="conventions"></a>Conventions

Par convention, les index sont nommés `IX_<type name>_<property name>`. Index composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.

## <a name="data-annotations"></a>Annotations de données

Les index ne peuvent pas être configurés à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Vous pouvez également spécifier un filtre.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Lors de l’utilisation du fournisseur SQL Server EF ajoute un 'IS NOT NULL' filtrer pour toutes les colonnes qui font partie d’un index unique. Pour remplacer cette convention, vous pouvez fournir un `null` valeur.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
