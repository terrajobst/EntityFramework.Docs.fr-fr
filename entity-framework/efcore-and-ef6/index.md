---
title: Comparer EF Core et EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997122"
---
# <a name="compare-ef-core--ef6"></a>Comparer EF Core et EF6

Il existe deux versions différentes d’Entity Framework : Entity Framework Core et Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) est une technologie d’accès aux données éprouvée, connue depuis de nombreuses années pour sa stabilité et la richesse de ses fonctionnalités. Elle a été publiée pour la première fois en 2008, dans le cadre de .NET Framework 3.5 SP1 et de Visual Studio 2008 SP1. Depuis la version EF4.1, elle est disponible en tant que [package EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/), l’un des packages les plus populaires sur NuGet.org.

EF6 continue d’être un produit pris en charge et de bénéficier de correctifs de bogues et d’améliorations mineures pendant un certain temps.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) est une version légère, extensible et multiplateforme d’Entity Framework. EF Core introduit de nombreuses améliorations et nouvelles fonctionnalités par rapport à EF6. Par ailleurs, EF Core est une nouvelle base de code, qui n’a pas la maturité d’EF6.

EF Core conserve l’expérience de développement d’EF6, et la plupart des API de niveau supérieur demeurent les mêmes ; l’utilisation d’EF Core est donc très proche de celle d’EF6. Parallèlement, EF Core repose sur un tout nouvel ensemble de composants de base. Cela signifie qu’EF Core n’hérite pas automatiquement de toutes les fonctionnalités d’EF6. Certaines de ces fonctionnalités apparaîtront dans les prochaines versions, tandis que d’autres, moins couramment utilisées, ne seront pas implémentées dans EF Core.

Le nouveau cœur extensible et léger nous a permis d’ajouter des fonctionnalités à EF Core qui ne seront pas implémentées dans EF6 (telles que les clés secondaires, les mises à jour par lot et l’évaluation client/base de données mixte dans les requêtes LINQ).
