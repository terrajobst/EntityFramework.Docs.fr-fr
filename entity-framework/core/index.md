---
title: Vue d’ensemble d’Entity Framework Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655620"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core est une version légère, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) et multiplateforme de la très connue technologie d’accès aux données Entity Framework.

EF Core peut servir de mappeur relationnel/objet (O/RM), permettant aux développeurs .NET de travailler avec une base de données à l’aide d’objets .NET, et éliminant la nécessité de la plupart du code d’accès aux données qu’ils doivent généralement écrire.

EF Core prend en charge de nombreux moteurs de base de données ; consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations.

## <a name="the-model"></a>Modèle

Avec EF Core, l’accès aux données est effectué à l’aide d’un modèle. Un modèle est composé de classes d’entités et d’un objet de contexte représentant une session avec la base de données, ce qui permet d’interroger et d’enregistrer des données. Pour en savoir plus, consultez [Création d’un modèle](modeling/index.md).

Vous pouvez générer un modèle à partir d’une base de données existante, coder manuellement un modèle en fonction de votre base de données ou utiliser [EF Migrations](managing-schemas/migrations/index.md) pour créer une base de données à partir de votre modèle et la faire évoluer au même rythme que celui-ci.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Interrogation

Les instances de vos classes d’entité sont récupérées de la base de données à l’aide de LINQ (Language Integrated Query). Pour en savoir plus, consultez [Interrogation des données](querying/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Enregistrement de données

Les données sont créées, supprimées et modifiées dans la base de données à l’aide d’instances de vos classes d’entité. Pour en savoir plus, consultez [Enregistrement de données](saving/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Étapes suivantes

Pour des tutoriels d’introduction, consultez [Bien démarrer avec Entity Framework Core](get-started/index.md).
