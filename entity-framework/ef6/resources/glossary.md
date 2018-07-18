---
title: Entity Framework glossaire - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: cbd61838afc23dfb37cee7c624c65476c5270099
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121458"
---
# <a name="entity-framework-glossary"></a>Entity Framework glossaire
## <a name="code-first"></a>Code First
Création d’un modèle Entity Framework à l’aide de code. Le modèle peut cibler et base de données existante ou une base de données.

## <a name="context"></a>Contexte
Une classe qui représente une session avec la base de données, ce qui vous permet d’interroger et d’enregistrer des données. Un contexte dérive de la classe DbContext ou ObjectContext.

## <a name="convention-code-first"></a>Convention (Code First)
Une règle qui utilise de Entity Framework pour déduire la forme de modèle vous à partir de vos classes.

## <a name="database-first"></a>Tout d’abord la base de données
Création d’un modèle Entity Framework, à l’aide du Concepteur EF, qui cible une base de données existante.

## <a name="eager-loading"></a>Chargement hâtif
Modèle de chargement des données associées dans lequel une requête pour un type d’entité charge également les entités associées dans le cadre de la requête.

## <a name="ef-designer"></a>Entity Framework Designer
Un concepteur visuel dans Visual Studio qui vous permet de créer un modèle Entity Framework à l’aide de zones et des lignes.

## <a name="entity"></a>Entité
Classe ou objet qui représente des données d'application, telles que des clients, des produits et des commandes.

## <a name="entity-data-model"></a>Entity Data Model
Un modèle qui décrit les entités et les relations entre eux. EF utilise le modèle EDM pour décrire le modèle conceptuel par rapport à laquelle les programmes de développement. EDM s’appuie sur le modèle entité-relation introduit par la récupération d’urgence. Peter Chen. Le modèle EDM a été développé avec pour principal objectif de devenir le common data model travers une suite de technologies de serveurs et les développeurs de Microsoft. EDM est également utilisé dans le cadre du protocole OData.

## <a name="explicit-loading"></a>Chargement explicite
Modèle de chargement des données associées dans lequel les objets connexes sont chargés en appelant une API.

## <a name="fluent-api"></a>API Fluent
Une API qui peut être utilisée pour configurer un modèle Code First.

## <a name="foreign-key-association"></a>Association de clé étrangère
Une association entre entités où une propriété qui représente la clé étrangère est incluse dans la classe de l’entité dépendante. Par exemple, le produit contient une propriété CategoryId.

## <a name="identifying-relationship"></a>Relation d’identification
Relation où la clé primaire de l'entité principale fait également partie de la clé primaire de l'entité dépendante. Dans ce type de relation, l'entité dépendante ne peut pas exister dans l'entité principale.

## <a name="independent-association"></a>Association indépendante
Une association entre entités où il n’existe aucune propriété qui représente la clé étrangère dans la classe de l’entité dépendante. Par exemple, une classe de produit contient une relation à la catégorie, mais aucune propriété CategoryId. Entity Framework effectue le suivi de l’état de l’association indépendamment de l’état des entités au niveau des terminaisons d’association de deux.

## <a name="lazy-loading"></a>Chargement différé
Modèle de chargement des données associées dans lequel les objets connexes sont chargés automatiquement lors de l’accès à une propriété de navigation.

## <a name="model-first"></a>Tout d’abord de modèle
Création d’un modèle Entity Framework, à l’aide du Concepteur EF, qui est ensuite utilisé pour créer une base de données.

## <a name="navigation-property"></a>Propriété de navigation
Une propriété d’une entité qui fait référence à une autre entité. Par exemple, produit contient une propriété de navigation de catégorie et catégorie contient une propriété de navigation de produits.

## <a name="poco"></a>POCO
Acronyme de Plain-Old CLR Object. Une classe utilisateur simple qui n’a aucune dépendance avec n’importe quelle infrastructure. Dans le contexte d’EF, une classe d’entité ne dérive pas à partir d’EntityObject, implémente toutes les interfaces ou comporte des attributs définis dans EF. Ces classes d’entité sont dissociés de l’infrastructure de persistance sont dits également « ignorant la persistance ».  

## <a name="relationship-inverse"></a>Inverse de la relation
L’autre extrémité d’une relation, par exemple, le produit. Catégorie et catégorie. Produit.

## <a name="self-tracking-entity"></a>Entité de suivi automatique
Entité constituée à partir d’un modèle de génération de code qui permet au développement d’applications multicouches.

## <a name="table-per-concrete-type-tpc"></a>Table-par type concret (TPC)
Une méthode de mappage de l’héritage, où chaque type non abstrait dans la hiérarchie est mappé pour séparer la table dans la base de données.

## <a name="table-per-hierarchy-tph"></a>Table par hiérarchie (TPH)
Une méthode de mappage de l’héritage où tous les types dans la hiérarchie sont mappées à la même table dans la base de données. Un ou plusieurs colonnes sont utilisé pour identifier le type de chaque ligne de discriminateur est associé.

## <a name="table-per-type-tpt"></a>Table par type (TPT)
Une méthode de mappage de l’héritage dans lequel les propriétés communes de tous les types dans la hiérarchie sont mappées à la même table dans la base de données, mais les propriétés propres à chaque type sont mappées à une table distincte.

## <a name="type-discovery"></a>Découverte de type
Le processus d’identification des types qui doivent faire partie d’un modèle Entity Framework.
