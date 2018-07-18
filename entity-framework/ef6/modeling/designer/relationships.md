---
title: Relations - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
caps.latest.revision: 3
ms.openlocfilehash: f924945b19dd6d73847ff3ec52c0b5a286c591bb
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121446"
---
# <a name="relationships---ef-designer"></a>Relations - Entity Framework Designer
> [!NOTE]
> Cette page fournit des informations sur la définition des relations dans votre modèle à l’aide du Concepteur EF. Pour obtenir des informations générales sur les relations dans Entity Framework et comment accéder à et manipuler des données à l’aide de relations, consultez [relations & Propriétés de Navigation](~/ef6/fundamentals/relationships.md).

Les associations définissent les relations entre les types d’entité dans un modèle. Cette rubrique montre comment mapper des associations avec Entity Framework Designer (Concepteur d’EF). L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Lorsque vous générez le modèle conceptuel, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs. Vous pouvez ignorer ces avertissements, car une fois que vous choisissez de générer la base de données à partir du modèle, les erreurs disparaîtront.

## <a name="associations-overview"></a>Vue d’ensemble d’associations

Lorsque vous concevez votre modèle à l’aide de l’Entity Framework Designer, un fichier .edmx représente votre modèle. Dans le fichier .edmx, un **Association** élément définit une relation entre deux types d’entités. Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ». La multiplicité d’une terminaison d’association peut avoir une valeur d’un (1), zéro ou un (0.. 1) ou plusieurs (\*). Ces informations sont spécifiées dans les deux enfants **fin** éléments.

Au moment de l’exécution, instances de type d’entité à une extrémité d’une association sont accessible via les propriétés de navigation ou de clés étrangères (si vous choisissez d’exposer les clés étrangères dans vos entités). Avec des clés étrangères exposées, la relation entre les entités est gérée avec un **ReferentialConstraint** élément (un élément enfant de le **Association** élément). Il est recommandé de toujours exposer les clés étrangères pour les relations dans vos entités.

> [!NOTE]
> Dans plusieurs-à-plusieurs (\*:\*) vous ne pouvez pas ajouter de clés étrangères aux entités. Dans un \*:\* relation, les informations d’association est gérée avec un objet indépendant.

Pour plus d’informations sur les éléments du langage CSDL (**ReferentialConstraint**, **Association**, etc.) consultez le [spécification CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Créer et supprimer des Associations

Création d’une association avec les mises à jour du Concepteur EF le contenu du modèle du fichier .edmx. Après avoir créé une association, vous devez créer les mappages pour l’association (décrits plus loin dans cette rubrique).

> [!NOTE]
> Cette section suppose que vous avez déjà ajouté les entités que vous souhaitez créer une association entre à votre modèle.

### <a name="to-create-an-association"></a>Pour créer une association

1.  Cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis sélectionnez **Association...** .
2.  Définissez les paramètres pour l’association dans le **ajouter une Association** boîte de dialogue.

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Vous pouvez choisir de ne pas ajouter les propriétés de navigation ou les propriétés de clé étrangère aux entités situées aux terminaisons de l’association en désactivant le ** propriété de Navigation ** et ** ajouter des propriétés de clé étrangères pour la &lt;nom de type d’entité&gt; entité ** cases à cocher. Si vous ajoutez une seule propriété de navigation, l'association n'est parcourable que dans une seule direction. Si vous n'ajoutez pas de propriétés de navigation, vous devez ajouter des propriétés de clé étrangère pour accéder aux entités au niveau des terminaisons de l'association.
    
3.  Cliquez sur **OK**.

### <a name="to-delete-an-association"></a>Pour supprimer une association

Pour supprimer une association, procédez comme suit :

-   Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez l’association de **supprimer**.

- OR :

-   Sélectionnez une ou plusieurs associations et appuyez sur la touche SUPPR.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Inclure les propriétés de clé étrangère dans vos entités (contraintes référentielles)

Il est recommandé de toujours exposer les clés étrangères pour les relations dans vos entités. Entity Framework utilise une contrainte référentielle pour identifier qu’une propriété agit comme la clé étrangère d’une relation.

Si vous avez coché la ***ajouter des propriétés de clé étrangères pour la &lt;nom de type d’entité&gt; entité*** case à cocher lorsque vous créez une relation, cette contrainte référentielle a été ajoutée pour vous.

Lorsque vous utilisez le Concepteur EF pour ajouter ou modifier une contrainte référentielle, le Concepteur EF ajoute ou modifie un **ReferentialConstraint** élément dans le contenu CSDL du fichier .edmx.

-   Double-cliquez sur l'association à modifier.
    Le **contrainte référentielle** boîte de dialogue s’affiche.
-   À partir de la **Principal** liste déroulante, sélectionnez l’entité principale dans la contrainte référentielle.
    Propriétés de clé de l’entité sont ajoutées à la **clé du principal du** liste dans la boîte de dialogue.
-   À partir de la **dépendants** liste déroulante, sélectionnez l’entité dépendante dans la contrainte référentielle.
-   Pour chaque clé principale qui a une clé dépendante, sélectionnez une clé dépendante correspondante dans les listes déroulantes dans le **clé dépendante** colonne.

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   Cliquez sur **OK**.

## <a name="create-and-edit-association-mappings"></a>créer et modifier des mappages d'association

Vous pouvez spécifier comment une association est mappé à la base de données dans le **détails de Mapping** fenêtre du Concepteur EF.

> [!NOTE]
> Vous ne pouvez mapper que des détails pour les associations qui n’ont pas une contrainte référentielle spécifiée. Si une contrainte référentielle n’est spécifiée une propriété de clé étrangère est incluse dans l’entité, puis vous pouvez utiliser les détails de mappage pour l’entité au contrôle de la clé étrangère de la colonne qui mappe à.

### <a name="create-an-association-mapping"></a>Créer un mappage d’association

-   Cliquez sur une association dans l’aire de conception et sélectionnez **mappage de Table**.
    Cette opération affiche le mappage d’association dans le **détails de Mapping** fenêtre.
-   Cliquez sur **ajouter une Table ou vue**.
    Une liste déroulante s'affiche. Elle contient toutes les tables du modèle de stockage.
-   Sélectionnez la table à laquelle l'association doit être mappée.
    Le **détails de Mapping** fenêtre affiche les deux terminaisons de l’association et les propriétés de clé pour le type d’entité à chaque **fin**.
-   Pour chaque propriété de clé, cliquez sur le **colonne** champ, puis sélectionnez la colonne à laquelle la propriété doit être mappée.

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Modifier un mappage d’association

-   Cliquez sur une association dans l’aire de conception et sélectionnez **mappage de Table**.
    Cette opération affiche le mappage d’association dans le **détails de Mapping** fenêtre.
-   Cliquez sur **est mappé à &lt;nom de la Table&gt;**.
    Une liste déroulante s'affiche. Elle contient toutes les tables du modèle de stockage.
-   Sélectionnez la table à laquelle l'association doit être mappée.
    Le **détails de Mapping** fenêtre affiche les deux terminaisons de l’association et les propriétés de clé pour le type d’entité à chaque extrémité.
-   Pour chaque propriété de clé, cliquez sur le **colonne** champ, puis sélectionnez la colonne à laquelle la propriété doit être mappée.

## <a name="edit-and-delete-navigation-properties"></a>Modifier et supprimer les propriétés de Navigation

Propriétés de navigation sont des propriétés de raccourci utilisées pour rechercher les entités situées aux terminaisons d’une association dans un modèle. Il est possible de créer des propriétés de navigation lorsque vous créez une association entre deux types d'entité.

#### <a name="to-edit-navigation-properties"></a>Pour modifier les propriétés de navigation

-   Sélectionnez une propriété de navigation sur l’aire du Concepteur d’Entity Framework.
    Informations sur la propriété de navigation s’affichent dans Visual Studio **propriétés** fenêtre.
-   Modifier les paramètres de propriété dans le **propriétés** fenêtre.

#### <a name="to-delete-navigation-properties"></a>Supprimer les propriétés de navigation

-   Si les clés étrangères ne sont pas exposées sur les types d'entité dans le modèle conceptuel, le fait de supprimer une propriété de navigation peut rendre l'association correspondante parcourable dans une seule direction ou pas parcourable du tout.
-   Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une propriété de navigation **supprimer**.
