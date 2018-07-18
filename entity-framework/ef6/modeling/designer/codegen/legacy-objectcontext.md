---
title: Retour à ObjectContext dans Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121164"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Retour à ObjectContext dans Entity Framework Designer
Avec une version précédente d’un modèle créé avec le Concepteur EF de Entity Framework génère un contexte dérivé ObjectContext et les classes d’entité dérivé à partir d’EntityObject.

En commençant par EF4.1 recommandé de passer en un modèle de génération de code qui génère un contexte dérivant des classes d’entité DbContext et POCO.

Dans Visual Studio 2012, vous obtenez code DbContext généré par défaut pour tous les nouveaux modèles créés avec le Concepteur EF. Les modèles existants continueront à générer du code d’ObjectContext en fonction, sauf si vous décidez d’échange pour le Générateur de code en fonction de DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Retour à la génération de Code d’ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Désactiver la génération de Code de DbContext

Génération des classes DbContext et POCO dérivées est gérée par deux fichiers .tt dans votre projet, si vous développez le fichier .edmx dans l’Explorateur de solutions, vous verrez ces fichiers. Supprimez ces deux fichiers de votre projet.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

Si vous utilisez VB.NET, vous devrez sélectionner le **afficher tous les fichiers** bouton pour afficher les fichiers imbriqués.

![ShowAllFiles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Réactiver la génération de Code de ObjectContext

Ouvrir dans le Concepteur EF, de modèle avec le bouton droit sur une section vide de l’aire de conception et sélectionnez **propriétés**.

Dans la modification de la fenêtre Propriétés la **stratégie de génération de Code** de **aucun** à **par défaut**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
