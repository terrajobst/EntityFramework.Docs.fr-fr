---
title: Index - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993215"
---
# <a name="indexes"></a>Index

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Un index dans une base de données relationnelle est mappé au même concept en tant qu’index dans le cœur d’Entity Framework.

## <a name="conventions"></a>Conventions

Par convention, les index sont nommés `IX_<type name>_<property name>`. Pour les index composites `<property name>` devient une liste séparée par des traits de soulignement de noms de propriétés.

## <a name="data-annotations"></a>Annotations de données

Index ne peuvent pas être configurés à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour configurer le nom d’un index.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Vous pouvez également spécifier un filtre.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Lors de l’utilisation du fournisseur SQL Server EF ajoute un 'IS NOT NULL' filtrer pour toutes les colonnes nullables qui font partie d’un index unique. Pour remplacer cette convention, vous pouvez fournir un `null` valeur.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
