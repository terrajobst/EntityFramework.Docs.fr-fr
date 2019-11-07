---
title: Index-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655699"
---
# <a name="indexes"></a>Index

Les index sont un concept commun entre de nombreux magasins de données. Bien que leur implémentation dans le magasin de données puisse varier, elles sont utilisées pour rendre les recherches basées sur une colonne (ou un ensemble de colonnes) plus efficaces.

## <a name="conventions"></a>Conventions

Par Convention, un index est créé dans chaque propriété (ou ensemble de propriétés) utilisée comme clé étrangère.

## <a name="data-annotations"></a>Annotations de données

Les index ne peuvent pas être créés à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier un index sur une seule propriété. Par défaut, les index sont non uniques.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Vous pouvez également spécifier qu’un index doit être unique, ce qui signifie que deux entités ne peuvent pas avoir la même valeur pour la ou les propriétés spécifiées.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Vous pouvez également spécifier un index sur plusieurs colonnes.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Il n’existe qu’un seul index par jeu de propriétés distinct. Si vous utilisez l’API Fluent pour configurer un index sur un ensemble de propriétés pour lequel un index est déjà défini, soit par Convention, soit par configuration précédente, vous allez modifier la définition de cet index. Cela est utile si vous souhaitez configurer davantage un index qui a été créé par Convention.
