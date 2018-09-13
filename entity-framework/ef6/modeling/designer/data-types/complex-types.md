---
title: Les Types complexes - Concepteur EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489906"
---
# <a name="complex-types---ef-designer"></a>Types complexes - Entity Framework Designer
Cette rubrique montre comment mapper des types complexes avec Entity Framework Designer (Concepteur d’EF) et comment interroger des entités qui contiennent des propriétés de type complexe.

L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Lorsque vous générez le modèle conceptuel, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs. Vous pouvez ignorer ces avertissements, car une fois que vous choisissez de générer la base de données à partir du modèle, les erreurs disparaîtront.

## <a name="what-is-a-complex-type"></a>Qu’est un Type complexe

Les types complexes sont les propriétés non scalaires des types d'entités qui permettent d'organiser les propriétés scalaires au sein des entités. À l'instar des entités, les types complexes regroupent des propriétés scalaires ou d'autres propriétés de type complexe.

Lorsque vous travaillez avec des objets qui représentent des types complexes, tenez compte des éléments suivants :

-   Les types complexes n’ont pas de clés et par conséquent ne peut pas exister indépendamment. Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.
-   Les types complexes ne peuvent pas participer aux associations et ne peut pas contenir les propriétés de navigation.
-   Propriétés de type complexe ne peut pas être **null**. Un ** InvalidOperationException ** se produit lorsque **DbContext.SaveChanges** est appelée et un objet complexe null est rencontré. Les propriétés scalaires des objets complexes peuvent être **null**.
-   Les types complexes ne peuvent pas hériter d'autres types complexes.
-   Vous devez définir le type complexe en tant qu’un **classe**. 
-   Entity Framework détecte les modifications apportées aux membres sur un objet de type complexe lorsque **DbContext.DetectChanges** est appelée. Entity Framework appelle **DetectChanges** automatiquement lorsque les membres suivants sont appelées : **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refactoriser des propriétés d’une entité en nouveau Type complexe

Si vous disposez déjà d’une entité dans votre modèle conceptuel, que vous souhaiterez peut-être refactoriser une partie des propriétés dans une propriété de type complexe.

Sur l’aire du concepteur, sélectionnez une ou plusieurs propriétés (à l’exclusion des propriétés de navigation) d’une entité, puis avec le bouton droit et sélectionnez **refactoriser -&gt; déplacer vers le nouveau Type complexe**.

![Réorganiser](~/ef6/media/refactor.png)

Un nouveau type complexe avec les propriétés sélectionnées est ajouté à la **Explorateur de modèles**. Un nom par défaut est affecté au type complexe.

Une propriété complexe du type récemment créé remplace les propriétés sélectionnées. Tous les mappages de propriété sont conservés.

![Refactoriser 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Créer un nouveau Type complexe

Vous pouvez également créer un nouveau type complexe qui ne contient-elle pas de propriétés d’une entité existante.

Avec le bouton droit le **des Types complexes** dossier dans l’Explorateur de modèles, pointez sur **Type complexe AddNew...** . Vous pouvez également sélectionner le **des Types complexes** dossier et appuyez sur la **insérer** clé de votre clavier.

![Ajouter un type complexe de nouveau](~/ef6/media/addnewcomplextype.png)

Un nouveau type complexe est ajouté au dossier avec un nom par défaut. Vous pouvez maintenant ajouter des propriétés pour le type.

## <a name="add-properties-to-a-complex-type"></a>Ajouter des propriétés à un Type complexe

Les propriétés d'un type complexe peuvent être des types scalaires ou des types complexes existants. Toutefois, les propriétés de type complexe ne peuvent pas avoir de références circulaires. Par exemple, un type complexe **OnsiteCourseDetails** ne peut pas avoir une propriété de type complexe **OnsiteCourseDetails**.

Vous pouvez ajouter une propriété à un type complexe en appliquant l'une des méthodes répertoriées ci-dessous.

-   Avec le bouton droit à un type complexe dans l’Explorateur de modèles, pointez sur **ajouter**, puis pointez sur **propriété scalaire** ou **propriété complexe**, puis sélectionnez le type de propriété souhaitée. Ou bien, vous pouvez sélectionner un type complexe et appuyez sur la **insérer** clé de votre clavier.  

    ![Ajouter des propriétés à un Type complexe](~/ef6/media/addpropertiestocomplextype.png)

    Une nouvelle propriété est ajoutée au type complexe avec un nom par défaut.

- OR :

-   Avec le bouton droit d’une propriété d’entité sur le **Entity Framework Designer** aire de conception et sélectionnez **copie**, puis cliquez sur le type complexe dans le **Explorateur de modèles** et sélectionnez  **Coller**.

## <a name="rename-a-complex-type"></a>Renommer un Type complexe

Lorsque vous renommez un type complexe, toutes les références au type sont mises à jour dans tout le projet.

-   Double-cliquez lentement sur un type complexe dans le **Explorateur de modèles**.
    Le nom est sélectionné et passe en mode édition.

- OR :

-   Avec le bouton droit à un type complexe dans le **Explorateur de modèles** et sélectionnez **renommer**.

- OR :

-   Sélectionnez un type complexe dans l'Explorateur de modèles, puis appuyez sur la touche F2.

- OR :

-   Avec le bouton droit à un type complexe dans le **Explorateur de modèles** et sélectionnez **propriétés**. Modifier le nom dans la **propriétés** fenêtre.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Ajouter un Type complexe existant à une entité et ses propriétés de la carte aux colonnes de Table

1.  Cliquez sur une entité, pointez sur **Ajouter nouveau**, puis sélectionnez **propriété complexe**.
    Une propriété de type complexe avec un nom par défaut est ajoutée à l'entité. Un type par défaut (sélectionné dans les types complexes existants) est affecté à la propriété.
2.  Affectez le type désiré à la propriété dans le **propriétés** fenêtre.
    Après avoir ajouté une propriété de type complexe à une entité, vous devez mapper ses propriétés aux colonnes de la table.
3.  Avec le bouton droit sur l’aire de conception ou dans un type d’entité le **Explorateur de modèles** et sélectionnez **mappages de Table**.
    Les mappages de table s’affichent dans le **détails de Mapping** fenêtre.
4.  Développez le **est mappé à &lt;nom de la Table&gt;**  nœud.
    Un **mappages de colonnes** nœud s’affiche.
5.  Développez le **mappages de colonnes** nœud.
    Une liste de toutes les colonnes de la table s'affiche. Les propriétés par défaut (le cas échéant) à laquelle mappent les colonnes sont répertoriés sous la **valeur/propriété** titre.
6.  Sélectionnez la colonne que vous souhaitez mapper et avec le bouton droit puis correspondant **valeur/propriété** champ.
    Une liste déroulante de toutes les propriétés scalaires s'affiche.
7.  Sélectionnez la propriété appropriée.

    ![Mapper le Type complexe](~/ef6/media/mapcomplextype.png)

8.  Répétez les étapes 6 et 7 pour chaque colonne de la table.

>[!NOTE]
> Pour supprimer un mappage de colonnes, sélectionnez la colonne que vous souhaitez mapper, puis cliquez sur le **valeur/propriété** champ. Ensuite, sélectionnez **supprimer** dans la liste déroulante.

## <a name="map-a-function-import-to-a-complex-type"></a>Mapper une importation de fonction à un Type complexe

Les importations de fonction sont basées sur les procédures stockées. Pour mapper une importation de fonction à un type complexe, les colonnes retournées par la procédure stockée correspondante doivent correspondre aux propriétés du type complexe en nombre et doivent avoir des types de stockage qui sont compatibles avec les types de propriété.

-   Double-cliquez sur une fonction importée que vous souhaitez mapper à un type complexe.

    ![Importations de fonction](~/ef6/media/functionimports.png)

-   Définissez les paramètres de la nouvelle importation de fonction comme suit :
    -   Spécifiez la procédure stockée pour laquelle vous créez une importation de fonction dans le **nom de la procédure stockée** champ. Ce champ est une liste déroulante qui affiche toutes les procédures stockées dans le modèle de stockage.
    -   Spécifiez le nom de l’importation de fonction dans le **nom du Function Import** champ.
    -   Sélectionnez **complexes** en tant que la valeur de retour tapez, puis spécifiez le type de retour complex spécifique en choisissant le type approprié dans la liste déroulante.

        ![Modifier l’importation de fonction](~/ef6/media/editfunctionimport.png)

-   Cliquez sur **OK**.
    L'entrée function import est créée dans le modèle conceptuel.

### <a name="customize-column-mapping-for-function-import"></a>Personnaliser le mappage pour l’importation de fonction de colonnes

-   Cliquez sur l’importation de fonction dans l’Explorateur de modèles et sélectionnez **mappage d’importation de fonction**.
    Le **détails de Mapping** fenêtre apparaît et affiche le mappage par défaut pour l’importation de fonction. Les flèches indiquent les mappages entre valeurs de colonne et valeurs de propriété. Par défaut, les noms de colonne sont supposés être les mêmes que ceux de la propriété du type complexe. Les noms de colonne par défaut s'affichent en gris.
-   Si nécessaire, changez les noms des colonnes pour qu'ils correspondent aux noms des colonnes retournés par la procédure stockée correspondant à l'importation de fonction.

## <a name="delete-a-complex-type"></a>Supprimer un Type complexe

Lorsque vous supprimez un type complexe, le type est supprimé du modèle conceptuel, et les mappages pour toutes les instances du type sont supprimés. Toutefois, les références au type ne sont pas mises à jour. Par exemple, si une entité a une propriété de type complexe de type ComplexType1 et que ComplexType1 est supprimé dans le **Explorateur de modèles**, la propriété d’entité correspondante n’est pas mis à jour. Le modèle ne sera pas validé, car elle contient une entité qui fait référence à un type complexe supprimé. Vous pouvez mettre à jour ou supprimer des références à des types complexes supprimés à l'aide du Concepteur d'entités.

-   Cliquez sur un type complexe dans l’Explorateur de modèles et sélectionnez **supprimer**.

- OR :

-   Sélectionnez un type complexe dans l'Explorateur de modèles, puis appuyez sur la touche Suppr de votre clavier.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Requête pour les entités qui contient les propriétés de Type complexe

Le code suivant montre comment exécuter une requête qui retourne une collection d’entités des objets de type qui contiennent une propriété de type complexe.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
