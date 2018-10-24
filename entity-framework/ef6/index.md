---
title: Vue d’ensemble - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 1efadf4484a13d5df2a2f11aad3d0e8f9ceff543
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315631"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) est un mappeur Objet Relationnel (O/RM) éprouvé pour .NET qui bénéficie de nombreuses années de développement de fonctionnalités et de stabilisation.

Tout comme un O/RM, EF6 réduit la différence d’impédance entre les mondes Relationnel et Objet. Cela permet aux développeurs d’écrire des applications qui interagissent avec des données stockées dans des bases de données relationnelles à l’aide d’objets .NET fortement typés représentant le domaine de l’application. De cette façon, ils n’ont plus besoin d’écrire une grande partie du code « de raccordement » qui sert à accéder aux données.

EF6 implémente de nombreuses fonctionnalités O/RM populaires :
- Mappage de classes d’entité [OCT](~/ef6/resources/glossary.md#poco) qui ne dépendent pas d’un type EF
- Détection automatique des changements
- Résolution d’identité et unité de travail
- Chargement hâtif, différé et explicite
- Traduction des requêtes fortement typées à l’aide de LINQ (Language Integrated Query)
- Fonctionnalités de mappage complètes, notamment la prise en charge des points suivants :
  - Relations un-à-un, un-à-plusieurs et plusieurs-à-plusieurs
  - Héritage (table par hiérarchie, table par type et table par classe concrète)
  - Types complexes
  - Procédures stockées
- Un concepteur visuel pour créer des modèles d’entité.
- Une expérience « Code First » pour créer des modèles d’entité en écrivant du code.
- Les modèles peuvent être générés à partir de bases de données existantes et modifiés à la main, ou créés à partir de zéro et utilisés pour générer de nouvelles bases de données.
- Intégration aux modèles d’application .NET Framework, y compris ASP.NET, et au moyen de la liaison de données avec WPF et WinForms.
- Connectivité de base de données basée sur ADO.NET et nombreux fournisseurs disponibles pour établir la connexion à SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.

## <a name="should-i-use-ef6-or-ef-core"></a>Dois-je utiliser EF6 ou EF Core ?

EF Core est une version plus moderne, légère et extensible d’Entity Framework, qui a des avantages et des fonctionnalités très similaires à EF6.
EF Core est le résultat d’une réécriture complète et contient de nombreuses nouvelles fonctionnalités non disponibles dans EF6, même s’il lui manque encore certaines des fonctionnalités de mappage les plus avancées d’EF6.
Si l’ensemble des fonctionnalités correspond à vos besoins, vous pouvez utiliser EF Core dans les nouvelles applications.
La section [Comparer EF Core et EF6](xref:efcore-and-ef6/index) examine ce choix plus en détail.

## <a name="get-startedef6get-startedmd"></a>[Bien démarrer](~/ef6/get-started.md)

Ajoutez le package NuGet EntityFramework à votre projet ou installez Entity Framework Tools pour Visual Studio. Ensuite, regardez les vidéos, lisez les tutoriels et la documentation avancée pour vous aider à tirer le meilleur parti d’EF6.

## <a name="past-entity-framework-versions"></a>Versions précédentes d’Entity Framework

Il s’agit de la documentation de la dernière version d’Entity Framework 6, bien qu’une grande partie s’applique aussi aux versions précédentes.
Consultez les rubriques [Nouveautés](~/ef6/what-is-new/index.md) et [Versions précédentes](~/ef6/what-is-new/past-releases.md) pour obtenir la liste complète des versions EF et des fonctionnalités introduites par chacune.
