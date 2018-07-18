---
title: Plusieurs diagrammes par modèle - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
caps.latest.revision: 3
ms.openlocfilehash: 3a022b3e44ecd4b6b62224cb6494c794397a9739
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121344"
---
# <a name="multiple-diagrams-per-model"></a>Plusieurs diagrammes par modèle
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Cette page et la vidéo montre comment fractionner un modèle en plusieurs diagrammes à l’aide d’Entity Framework Designer (Concepteur d’EF). Vous souhaiterez peut-être utiliser cette fonctionnalité lorsque votre modèle devient trop volumineux pour afficher ou modifier.

Dans les versions antérieures du Concepteur EF peut uniquement avoir un seul diagramme par le fichier EDMX. À compter de Visual Studio 2012, vous pouvez utiliser le Concepteur EF pour fractionner votre fichier EDMX dans plusieurs diagrammes.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo montre comment fractionner un modèle en plusieurs diagrammes à l’aide d’Entity Framework Designer (Concepteur d’EF). Vous souhaiterez peut-être utiliser cette fonctionnalité lorsque votre modèle devient trop volumineux pour afficher ou modifier.

**Présenté par**: Julia Kornich

**Vidéo**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Vue d’ensemble de Concepteur EF

Lorsque vous créez un modèle à l’aide de l’Assistant du Concepteur EF Entity Data Model, un fichier .edmx est créé et ajouté à votre solution. Ce fichier définit la forme de vos entités et comment elles correspondent à la base de données.

Le Concepteur EF comprend les composants suivants :

-   Une aire de conception visuelle pour la modification du modèle. Vous pouvez créer, modifier ou supprimer des entités et des associations.
-   Un **Explorateur de modèles** fenêtre qui fournit des vues de l’arborescence du modèle.  Les entités et leurs associations sont situées sous le *\[ModelName\]* dossier. Les tables de base de données et les contraintes sont situés sous le  *\[ModelName\]*. Dossier de Store.
-   Un **détails de Mapping** fenêtre pour afficher et modifier des mappages. Vous pouvez mapper des types d'entités ou des associations à des tables, colonnes ou procédures stockées de base de données. 

La fenêtre de surface de conception visuelle s’ouvre automatiquement à l’issue de l’Assistant Entity Data Model. Si l’Explorateur de modèles n’est pas visible, cliquez sur la principale aire de conception et sélectionnez **Explorateur de modèles**.

La capture d’écran suivante montre un fichier .edmx ouvert dans le Concepteur EF. La capture d’écran montre l’aire de conception visuelle (à gauche) et le **Explorateur de modèles** fenêtre (à droite).

![EFDesigner2](~/ef6/media/efdesigner2.png)

Pour annuler une opération effectuée dans le Concepteur EF, cliquez sur Ctrl-Z.

## <a name="working-with-diagrams"></a>Utilisation des diagrammes

Par défaut, le Concepteur EF crée un diagramme appelé Diagram1. Si vous avez un diagramme composé d’un grand nombre d’entités et associations, sera plus que souhaitez diviser logiquement. À compter de Visual Studio 2012, vous pouvez afficher votre modèle conceptuel dans plusieurs diagrammes.   

Lorsque vous ajoutez de nouveaux schémas, ils apparaissent sous le dossier de diagrammes dans la fenêtre Explorateur de modèles. Pour renommer un diagramme : sélectionnez le diagramme dans la fenêtre du navigateur de modèle et cliquez sur une seule fois sur le nom, tapez le nouveau nom.  Vous pouvez également cliquer sur le nom de diagramme et sélectionner **renommer**.

Le nom du diagramme s’affiche en regard du nom de fichier .edmx, dans l’éditeur Visual Studio. Par exemple Model1.edmx\[Diagram1\].

![Nom diagramme](~/ef6/media/diagramname.png)

Le contenu de diagrammes (forme et la couleur des entités et associations) est stocké dans le. edmx.diagram fichier. Pour afficher ce fichier, sélectionnez l’Explorateur de solutions et dérouler le fichier .edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Vous ne devez pas modifier le. edmx.diagram de fichiers manuellement, le contenu de ce fichier peut-être remplacé par le Concepteur EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Fractionnement d’entités et Associations dans un nouveau diagramme

Vous pouvez sélectionner les entités sur le schéma existant (maintenez la touche MAJ enfoncée pour sélectionner plusieurs entités). Cliquez sur le bouton droit de la souris et sélectionnez **déplacer vers le nouveau diagramme**. Le nouveau diagramme est créé et les entités sélectionnées et leurs associations sont déplacées vers le diagramme.

Ou bien, vous pouvez cliquez sur le dossier de diagrammes dans l’Explorateur de modèles et sélectionnez **Ajouter nouveau diagramme.** Vous pouvez ensuite glisser les entités figurant dans le dossier de Types d’entités dans l’Explorateur de modèles sur l’aire de conception.

Vous pouvez également couper ou copier des entités (à l’aide des touches Ctrl + X ou Ctrl-C) à partir d’un diagramme et Coller (à l’aide de la clé de Ctrl + V) sur l’autre. Si le diagramme dans lequel vous effectuez un collage une entité déjà contient une entité portant le même nom, une nouvelle entité est créée et ajoutée au modèle.  Par exemple : schéma 2 contient l’entité Department. Ensuite, vous collez un autre service sur le diagramme 2. L’entité Department1 est créée et ajoutée au modèle conceptuel.   

Pour inclure les entités connexes dans un diagramme, cliquez l’entité et sélectionnez **inclure connexes**. Cela va créer une copie des entités associées et des associations dans le diagramme spécifié.

## <a name="changing-the-color-of-entities"></a>Modification de la couleur d’entités

En plus du fractionnement d’un modèle dans plusieurs diagrammes, vous pouvez également modifier les couleurs de vos entités.

Pour modifier la couleur, sélectionnez une entité (ou plusieurs entités) sur l’aire de conception. Ensuite, cliquez sur le bouton droit de la souris et sélectionnez **propriétés**. Dans la fenêtre Propriétés, sélectionnez le **couleur de remplissage** propriété. Spécifiez la couleur à l’aide d’un nom de couleur valide (par exemple, rouge) ou un valide RVB (par exemple, 255, 128, 128). 

![Color](~/ef6/media/color.png)

## <a name="summary"></a>Récapitulatif

Dans cette rubrique, nous avons vu comment fractionner un modèle en plusieurs diagrammes et également comment spécifier une couleur différente pour une entité à l’aide d’Entity Framework Designer. 
