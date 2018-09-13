---
title: Raccourcis clavier du Concepteur de Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490244"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Raccourcis de clavier du concepteur Entity Framework
Cette page fournit une liste de raccourcis clavier qui sont disponibles dans les différents écrans de l’Entity Framework Tools pour Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Assistant ADO.NET Entity Data Model

### <a name="step-one-choose-model-contents"></a>Étape 1 : Choisissez le contenu du modèle

![Un Assistant](~/ef6/media/wizardone.png)

| Raccourci  | Action                                                     | Notes                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Déplacer vers l’écran suivant                                        | Non disponible pour toutes les sélections de contenu du modèle. |
| **Alt + f** | Terminer l’Assistant                                              | Non disponible pour toutes les sélections de contenu du modèle. |
| **ALT + w** | Déplacer le focus vers le « ce que le modèle devrait contenir ? » volet. |                                                     |

### <a name="step-two-choose-your-connection"></a>Étape 2 : Choisir votre connexion

![Assistant deux](~/ef6/media/wizardtwo.png)

| Raccourci  | Action                                                     | Notes                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Déplacer vers l’écran suivant                                        |                                                         |
| **ALT + p** | Déplacer vers l’écran précédent                                    |                                                         |
| **ALT + w** | Déplacer le focus vers le « ce que le modèle devrait contenir ? » volet. |                                                         |
| **Alt + c** | Ouvrez la fenêtre « Propriétés de connexion »                    | Permet la définition d’une nouvelle connexion de base de données. |
| **Alt + e** | Exclure les données sensibles de la chaîne de connexion          |                                                         |
| **Alt + i** | Inclure les données sensibles dans la chaîne de connexion            |                                                         |
| **Alt + s** | Activer/désactiver l’option « Enregistrer les paramètres de connexion dans App.Config » |                                                         |

### <a name="step-three-choose-your-version"></a>Étape 3 : Choisir votre Version

![Assistant trois](~/ef6/media/wizardthree.png)

| Raccourci  | Action                                             | Notes                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Déplacer vers l’écran suivant                                |                                                                                       |
| **ALT + p** | Déplacer vers l’écran précédent                            |                                                                                       |
| **ALT + w** | Déplacer le focus vers la sélection de la version Entity Framework | Permet de spécifier une autre version d’Entity Framework pour une utilisation dans le projet. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Étape 4 : Choisir vos objets de base de données et les paramètres

![Assistant quatre](~/ef6/media/wizardfour.png)

| Raccourci  | Action                                                                                    | Notes                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | Terminer l’Assistant                                                                             |                                                                     |
| **ALT + p** | Déplacer vers l’écran précédent                                                                   |                                                                     |
| **ALT + w** | Déplacer le focus vers le volet de sélection d’objets de base de données                                            | Permet de spécification d’objets de base de données pour être inverse conçu.    |
| **Alt + s** | Activer/désactiver le « mettre au pluriel ou au singulier les noms d’objets générés « option                       |                                                                     |
| **ALT + k** | Activer/désactiver l’option « Inclure les colonnes clés étrangères dans le modèle »                              | Non disponible pour toutes les sélections de contenu du modèle.                 |
| **Alt + i** | Activer/désactiver l’option « Importer sélectionnés des procédures stockées et fonctions dans le modèle d’entité » | Non disponible pour toutes les sélections de contenu du modèle.                 |
| **ALT + m** | Place le focus sur le champ de texte « Modèle Namespace »                                        | Non disponible pour toutes les sélections de contenu du modèle.                 |
| **Espace** | Sélection à bascule sur l’élément                                                               | Si l’élément a des enfants, tous les éléments enfants seront également basculer |
| **Gauche**  | Réduire l’arborescence enfant                                                                       |                                                                     |
| **Droite** | Développez l’arborescence enfant                                                                         |                                                                     |
| **À distance**    | Accédez à l’élément précédent dans l’arborescence                                                      |                                                                     |
| **Vers le bas**  | Accédez à l’élément suivant dans l’arborescence                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Aire du Concepteur EF

![Aire du Concepteur](~/ef6/media/designersurface.png)

| Raccourci                                                                                | Action                      | Notes                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Entrez/espace**                                                                         | Basculer la sélection            | Active ou désactive la sélection sur l’objet qui a le focus.                                                                                                                                                                                         |
| **Échap**                                                                                 | Annuler la sélection            | Annule la sélection actuelle.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Tout Sélectionner                  | Sélectionne toutes les formes sur l’aire de conception.                                                                                                                                                                                       |
| **Flèche haut**                                                                            | Monter                     | Déplace les entités d’un incrément de grille sélectionnés. <br/> Si, dans une liste, déplace vers le sous-champ frère précédent.                                                                                                                            |
| **Flèche vers le bas**                                                                          | Descendre                   | Déplace l’entité vers le bas un incrément de grille sélectionnée. <br/> Si, dans une liste, déplace vers le sous-champ frère suivant.                                                                                                                              |
| **Flèche gauche**                                                                          | Déplacer à gauche                   | Déplace l’entité gauche un incrément de grille sélectionnée. <br/> Si, dans une liste, déplace vers le sous-champ frère précédent.                                                                                                                          |
| **Flèche droite**                                                                         | Déplacer vers la droite                  | Déplace l’incrément de grille droite une entité sélectionnée. <br/> Si, dans une liste, déplace vers le sous-champ frère suivant.                                                                                                                             |
| **Maj + flèche gauche**                                                                  | Forme de taille de gauche             | Réduit la largeur de l’entité sélectionnée par un incrément de grille.                                                                                                                                                                     |
| **Maj + flèche droite**                                                                 | Forme de taille appropriée            | Augmente la largeur de l’entité sélectionnée par un incrément de grille.                                                                                                                                                                   |
| **Accueil**                                                                                | Premiers homologues                  | Déplace le focus et la sélection au premier objet sur l’aire de conception au même niveau de pair.                                                                                                                                         |
| **Fin**                                                                                 | Dernière homologue                   | Déplace le focus et la sélection vers le dernier objet sur l’aire de conception au même niveau de pair.                                                                                                                                          |
| **CTRL + origine**                                                                         | Premiers homologues (focus)          | Identique au premier homologue, mais déplace le focus au lieu de déplacer la sélection.                                                                                                                                                          |
| **CTRL + fin**                                                                          | Dernière homologue (focus)           | Identique à la dernière de l’homologue, mais déplace le focus au lieu de déplacer la sélection.                                                                                                                                                           |
| **Tab**                                                                                 | Homologue suivant                   | Déplace le focus et la sélection vers l’objet suivant sur l’aire de conception au même niveau de pair.                                                                                                                                          |
| **Maj+Tab**                                                                           | Homologue précédente               | Déplace le focus et la sélection à l’objet précédent sur l’aire de conception au même niveau de pair.                                                                                                                                      |
| **Ctrl + Alt + Tab**                                                                        | Homologue suivant (focus)           | Identique à suivant de l’homologue, mais déplace le focus au lieu de déplacer la sélection.                                                                                                                                                           |
| **Alt + Ctrl + Maj + Tab**                                                                  | Homologue précédente (focus)       | Identique au précédent homologue, mais déplace le focus au lieu de déplacer la sélection.                                                                                                                                                       |
| **&lt;**                                                                                | Remonter                      | Se déplace vers l’objet suivant sur la surface de conception au niveau supérieure dans la hiérarchie. S’il n’existe aucune forme au-dessus de cette forme dans la hiérarchie (autrement dit, l’objet est placé directement sur l’aire de conception), le diagramme est sélectionné. |
| **&gt;**                                                                                | Descendre                     | Se déplace vers l’objet de relation contenant-contenu suivant sur la conception surface un niveau en dessous celle-ci dans la hiérarchie. S’il n’existe aucun objet de relation contenant-contenu, il s’agit d’une absence d’opération.                                                                              |
| **CTRL + &lt;**                                                                         | Remonter (focus)              | Identique à remonter la commande, mais déplace le focus sans la sélection.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Descendent (focus)             | Identique à descendent commande, mais déplace le focus sans la sélection.                                                                                                                                                                         |
| **MAJ + fin**                                                                         | Suivez à connecté         | À partir d’une entité, se déplace à une entité qui n’est connectée à cette entité.                                                                                                                                                               |
| **Suppr**                                                                                 | Supprimer                      | Supprimer un objet ou un connecteur à partir du diagramme.                                                                                                                                                                                     |
| **Ins**                                                                                 | Insert                      | Ajoute une nouvelle propriété à une entité lorsque l’en-tête de compartiment de « Propriétés scalaires » ou une propriété proprement dite est sélectionnée.                                                                                                           |
| **Page préc.**                                                                               | Diagramme de défilement des           | Fait défiler vers l’aire de conception, incréments égaux à 75 % de la hauteur de l’aire de conception actuellement visible.                                                                                                                    |
| **Page suiv.**                                                                             | Diagramme de défilement vers le bas         | Fait défiler l’aire de conception.                                                                                                                                                                                                    |
| **Maj + Pg vers le bas**                                                                     | Diagramme de défilement droite        | Fait défiler vers l’aire de conception vers la droite.                                                                                                                                                                                            |
| **MAJ + Page préc.**                                                                       | Diagramme de défilement gauche         | Fait défiler vers l’aire de conception vers la gauche.                                                                                                                                                                                             |
| **F2**                                                                                  | Entrer en mode édition             | Raccourci clavier standard pour entrer en mode édition pour un contrôle de texte.                                                                                                                                                               |
| **MAJ + F10**                                                                         | Afficher le menu contextuel       | Raccourci clavier standard pour afficher le menu contextuel d’un élément sélectionné.                                                                                                                                                          |
| **CTRL + MAJ + souris clic gauche**  <br/> **CTRL + MAJ + molette de souris vers l’avant**  | Zoom sémantique dans            | Effectue un zoom avant sur la zone de l’affichage des diagrammes sous le pointeur de la souris.                                                                                                                                                                 |
| **CTRL + MAJ + la souris avec le bouton droit cliquez sur** <br/> **CTRL + MAJ + molette de souris vers l’arrière** | Zoom sémantique Out           | Effectue un zoom arrière à partir de la zone de l’affichage des diagrammes sous le pointeur de la souris. Il ré-concentre le diagramme lorsque vous effectuer un zoom arrière la trop loin du centre de diagramme actuel.                                                                          |
| **CTRL + MAJ + '+'** <br/> **CTRL + molette de souris vers l’avant**                        | Zoom avant                     | Effectue un zoom avant sur le centre de la vue de diagramme.                                                                                                                                                                                         |
| **CTRL + MAJ + '-'** <br/> **CTRL + molette de souris vers l’arrière**                       | Zoom arrière                    | Effectue un zoom arrière à partir de la zone sélectionnée de la vue de diagramme. Il ré-concentre le diagramme lorsque vous effectuer un zoom arrière la trop loin du centre de diagramme actuel.                                                                                            |
| **CTRL + MAJ + dessiner un rectangle avec le bouton gauche de la souris vers le bas**                  | Zone de zoom                   | Effectue un zoom centrée sur la zone que vous avez sélectionné. Lorsque vous maintenez le contrôle + MAJ enfoncées, vous verrez que le curseur se transforme en loupe, ce qui vous permet de définir la zone pour effectuer un zoom dans.                        |
| **Touche du Menu contextuel + suis '**                                                              | Ouvrez la fenêtre Détails de mappage | Ouvre la fenêtre Détails de mappage pour modifier les mappages pour l’entité sélectionnée                                                                                                                                                               |

## <a name="mapping-details-window"></a>Fenêtre Détails de mappage

![Raccourcis des détails de mappage](~/ef6/media/mappingdetailsshortcuts.png)

| Raccourci                  | Action         | Notes                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tab**                   | Basculez le contexte | Bascule entre la zone de fenêtre principale et de la barre d’outils sur la gauche                                                                     |
| **Touches de direction**            | Navigation     | Vous déplacer dans les lignes, ou à droite et gauche sur les colonnes de la zone de fenêtre principale. Déplacer entre les boutons dans la barre d’outils sur la gauche. |
| **Entrée** <br/> **Espace** | Sélectionner         | Sélectionne un bouton dans la barre d’outils sur la gauche.                                                                                          |
| **ALT + flèche vers le bas**      | Ouvrir la liste      | Une liste déroulante si une cellule est sélectionnée qui a une zone de liste déroulante.                                                                     |
| **Entrée**                 | Liste, sélectionnez    | Sélectionne un élément dans une zone de liste déroulante.                                                                                               |
| **Échap**                   | Liste de fermeture     | Ferme une zone de liste déroulante.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navigation de Visual Studio

Entity Framework fournit également un nombre d’actions qui peuvent avoir des raccourcis clavier personnalisés mappés (aucuns raccourcis ne sont mappés par défaut). Pour créer ces raccourcis peuvent être personnalisés, cliquez sur le menu Outils, puis sur Options.  Sous l’environnement, choisissez le clavier.  Défiler la liste du milieu jusqu'à ce que vous pouvez sélectionner la commande souhaitée, entrez le raccourci dans la zone de texte « Appuyez sur les touches de raccourci » et cliquez sur attribuer. Les raccourcis possibles sont les suivantes :

| Raccourci                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
