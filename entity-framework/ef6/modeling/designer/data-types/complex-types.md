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
# <a name="complex-types---ef-designer"></a><span data-ttu-id="45bf3-102">Types complexes - Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="45bf3-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="45bf3-103">Cette rubrique montre comment mapper des types complexes avec Entity Framework Designer (Concepteur d’EF) et comment interroger des entités qui contiennent des propriétés de type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="45bf3-104">L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="45bf3-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="45bf3-106">Lorsque vous générez le modèle conceptuel, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="45bf3-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="45bf3-107">Vous pouvez ignorer ces avertissements, car une fois que vous choisissez de générer la base de données à partir du modèle, les erreurs disparaîtront.</span><span class="sxs-lookup"><span data-stu-id="45bf3-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="45bf3-108">Qu’est un Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-108">What is a Complex Type</span></span>

<span data-ttu-id="45bf3-109">Les types complexes sont les propriétés non scalaires des types d'entités qui permettent d'organiser les propriétés scalaires au sein des entités.</span><span class="sxs-lookup"><span data-stu-id="45bf3-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="45bf3-110">À l'instar des entités, les types complexes regroupent des propriétés scalaires ou d'autres propriétés de type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="45bf3-111">Lorsque vous travaillez avec des objets qui représentent des types complexes, tenez compte des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="45bf3-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="45bf3-112">Les types complexes n’ont pas de clés et par conséquent ne peut pas exister indépendamment.</span><span class="sxs-lookup"><span data-stu-id="45bf3-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="45bf3-113">Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="45bf3-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="45bf3-114">Les types complexes ne peuvent pas participer aux associations et ne peut pas contenir les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="45bf3-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="45bf3-115">Propriétés de type complexe ne peut pas être **null**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="45bf3-116">Un \*\* InvalidOperationException \*\* se produit lorsque **DbContext.SaveChanges** est appelée et un objet complexe null est rencontré.</span><span class="sxs-lookup"><span data-stu-id="45bf3-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="45bf3-117">Les propriétés scalaires des objets complexes peuvent être **null**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="45bf3-118">Les types complexes ne peuvent pas hériter d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="45bf3-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="45bf3-119">Vous devez définir le type complexe en tant qu’un **classe**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="45bf3-120">Entity Framework détecte les modifications apportées aux membres sur un objet de type complexe lorsque **DbContext.DetectChanges** est appelée.</span><span class="sxs-lookup"><span data-stu-id="45bf3-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="45bf3-121">Entity Framework appelle **DetectChanges** automatiquement lorsque les membres suivants sont appelées : **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="45bf3-122">Refactoriser des propriétés d’une entité en nouveau Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="45bf3-123">Si vous disposez déjà d’une entité dans votre modèle conceptuel, que vous souhaiterez peut-être refactoriser une partie des propriétés dans une propriété de type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="45bf3-124">Sur l’aire du concepteur, sélectionnez une ou plusieurs propriétés (à l’exclusion des propriétés de navigation) d’une entité, puis avec le bouton droit et sélectionnez **refactoriser -&gt; déplacer vers le nouveau Type complexe**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Réorganiser](~/ef6/media/refactor.png)

<span data-ttu-id="45bf3-126">Un nouveau type complexe avec les propriétés sélectionnées est ajouté à la **Explorateur de modèles**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="45bf3-127">Un nom par défaut est affecté au type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-127">The complex type is given a default name.</span></span>

<span data-ttu-id="45bf3-128">Une propriété complexe du type récemment créé remplace les propriétés sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="45bf3-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="45bf3-129">Tous les mappages de propriété sont conservés.</span><span class="sxs-lookup"><span data-stu-id="45bf3-129">All property mappings are preserved.</span></span>

![Refactoriser 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="45bf3-131">Créer un nouveau Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-131">Create a New Complex Type</span></span>

<span data-ttu-id="45bf3-132">Vous pouvez également créer un nouveau type complexe qui ne contient-elle pas de propriétés d’une entité existante.</span><span class="sxs-lookup"><span data-stu-id="45bf3-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="45bf3-133">Avec le bouton droit le **des Types complexes** dossier dans l’Explorateur de modèles, pointez sur **Type complexe AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="45bf3-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="45bf3-134">Vous pouvez également sélectionner le **des Types complexes** dossier et appuyez sur la **insérer** clé de votre clavier.</span><span class="sxs-lookup"><span data-stu-id="45bf3-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Ajouter un type complexe de nouveau](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="45bf3-136">Un nouveau type complexe est ajouté au dossier avec un nom par défaut.</span><span class="sxs-lookup"><span data-stu-id="45bf3-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="45bf3-137">Vous pouvez maintenant ajouter des propriétés pour le type.</span><span class="sxs-lookup"><span data-stu-id="45bf3-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="45bf3-138">Ajouter des propriétés à un Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="45bf3-139">Les propriétés d'un type complexe peuvent être des types scalaires ou des types complexes existants.</span><span class="sxs-lookup"><span data-stu-id="45bf3-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="45bf3-140">Toutefois, les propriétés de type complexe ne peuvent pas avoir de références circulaires.</span><span class="sxs-lookup"><span data-stu-id="45bf3-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="45bf3-141">Par exemple, un type complexe **OnsiteCourseDetails** ne peut pas avoir une propriété de type complexe **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="45bf3-142">Vous pouvez ajouter une propriété à un type complexe en appliquant l'une des méthodes répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="45bf3-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="45bf3-143">Avec le bouton droit à un type complexe dans l’Explorateur de modèles, pointez sur **ajouter**, puis pointez sur **propriété scalaire** ou **propriété complexe**, puis sélectionnez le type de propriété souhaitée.</span><span class="sxs-lookup"><span data-stu-id="45bf3-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="45bf3-144">Ou bien, vous pouvez sélectionner un type complexe et appuyez sur la **insérer** clé de votre clavier.</span><span class="sxs-lookup"><span data-stu-id="45bf3-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Ajouter des propriétés à un Type complexe](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="45bf3-146">Une nouvelle propriété est ajoutée au type complexe avec un nom par défaut.</span><span class="sxs-lookup"><span data-stu-id="45bf3-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="45bf3-147">OR :</span><span class="sxs-lookup"><span data-stu-id="45bf3-147">OR -</span></span>

-   <span data-ttu-id="45bf3-148">Avec le bouton droit d’une propriété d’entité sur le **Entity Framework Designer** aire de conception et sélectionnez **copie**, puis cliquez sur le type complexe dans le **Explorateur de modèles** et sélectionnez  **Coller**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="45bf3-149">Renommer un Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-149">Rename a Complex Type</span></span>

<span data-ttu-id="45bf3-150">Lorsque vous renommez un type complexe, toutes les références au type sont mises à jour dans tout le projet.</span><span class="sxs-lookup"><span data-stu-id="45bf3-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="45bf3-151">Double-cliquez lentement sur un type complexe dans le **Explorateur de modèles**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="45bf3-152">Le nom est sélectionné et passe en mode édition.</span><span class="sxs-lookup"><span data-stu-id="45bf3-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="45bf3-153">OR :</span><span class="sxs-lookup"><span data-stu-id="45bf3-153">OR -</span></span>

-   <span data-ttu-id="45bf3-154">Avec le bouton droit à un type complexe dans le **Explorateur de modèles** et sélectionnez **renommer**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="45bf3-155">OR :</span><span class="sxs-lookup"><span data-stu-id="45bf3-155">OR -</span></span>

-   <span data-ttu-id="45bf3-156">Sélectionnez un type complexe dans l'Explorateur de modèles, puis appuyez sur la touche F2.</span><span class="sxs-lookup"><span data-stu-id="45bf3-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="45bf3-157">OR :</span><span class="sxs-lookup"><span data-stu-id="45bf3-157">OR -</span></span>

-   <span data-ttu-id="45bf3-158">Avec le bouton droit à un type complexe dans le **Explorateur de modèles** et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="45bf3-159">Modifier le nom dans la **propriétés** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="45bf3-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="45bf3-160">Ajouter un Type complexe existant à une entité et ses propriétés de la carte aux colonnes de Table</span><span class="sxs-lookup"><span data-stu-id="45bf3-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="45bf3-161">Cliquez sur une entité, pointez sur **Ajouter nouveau**, puis sélectionnez **propriété complexe**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="45bf3-162">Une propriété de type complexe avec un nom par défaut est ajoutée à l'entité.</span><span class="sxs-lookup"><span data-stu-id="45bf3-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="45bf3-163">Un type par défaut (sélectionné dans les types complexes existants) est affecté à la propriété.</span><span class="sxs-lookup"><span data-stu-id="45bf3-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="45bf3-164">Affectez le type désiré à la propriété dans le **propriétés** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="45bf3-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="45bf3-165">Après avoir ajouté une propriété de type complexe à une entité, vous devez mapper ses propriétés aux colonnes de la table.</span><span class="sxs-lookup"><span data-stu-id="45bf3-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="45bf3-166">Avec le bouton droit sur l’aire de conception ou dans un type d’entité le **Explorateur de modèles** et sélectionnez **mappages de Table**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="45bf3-167">Les mappages de table s’affichent dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="45bf3-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="45bf3-168">Développez le **est mappé à &lt;nom de la Table&gt;**  nœud.</span><span class="sxs-lookup"><span data-stu-id="45bf3-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="45bf3-169">Un **mappages de colonnes** nœud s’affiche.</span><span class="sxs-lookup"><span data-stu-id="45bf3-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="45bf3-170">Développez le **mappages de colonnes** nœud.</span><span class="sxs-lookup"><span data-stu-id="45bf3-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="45bf3-171">Une liste de toutes les colonnes de la table s'affiche.</span><span class="sxs-lookup"><span data-stu-id="45bf3-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="45bf3-172">Les propriétés par défaut (le cas échéant) à laquelle mappent les colonnes sont répertoriés sous la **valeur/propriété** titre.</span><span class="sxs-lookup"><span data-stu-id="45bf3-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="45bf3-173">Sélectionnez la colonne que vous souhaitez mapper et avec le bouton droit puis correspondant **valeur/propriété** champ.</span><span class="sxs-lookup"><span data-stu-id="45bf3-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="45bf3-174">Une liste déroulante de toutes les propriétés scalaires s'affiche.</span><span class="sxs-lookup"><span data-stu-id="45bf3-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="45bf3-175">Sélectionnez la propriété appropriée.</span><span class="sxs-lookup"><span data-stu-id="45bf3-175">Select the appropriate property.</span></span>

    ![Mapper le Type complexe](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="45bf3-177">Répétez les étapes 6 et 7 pour chaque colonne de la table.</span><span class="sxs-lookup"><span data-stu-id="45bf3-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="45bf3-178">Pour supprimer un mappage de colonnes, sélectionnez la colonne que vous souhaitez mapper, puis cliquez sur le **valeur/propriété** champ.</span><span class="sxs-lookup"><span data-stu-id="45bf3-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="45bf3-179">Ensuite, sélectionnez **supprimer** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="45bf3-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="45bf3-180">Mapper une importation de fonction à un Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="45bf3-181">Les importations de fonction sont basées sur les procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="45bf3-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="45bf3-182">Pour mapper une importation de fonction à un type complexe, les colonnes retournées par la procédure stockée correspondante doivent correspondre aux propriétés du type complexe en nombre et doivent avoir des types de stockage qui sont compatibles avec les types de propriété.</span><span class="sxs-lookup"><span data-stu-id="45bf3-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="45bf3-183">Double-cliquez sur une fonction importée que vous souhaitez mapper à un type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importations de fonction](~/ef6/media/functionimports.png)

-   <span data-ttu-id="45bf3-185">Définissez les paramètres de la nouvelle importation de fonction comme suit :</span><span class="sxs-lookup"><span data-stu-id="45bf3-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="45bf3-186">Spécifiez la procédure stockée pour laquelle vous créez une importation de fonction dans le **nom de la procédure stockée** champ.</span><span class="sxs-lookup"><span data-stu-id="45bf3-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="45bf3-187">Ce champ est une liste déroulante qui affiche toutes les procédures stockées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="45bf3-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="45bf3-188">Spécifiez le nom de l’importation de fonction dans le **nom du Function Import** champ.</span><span class="sxs-lookup"><span data-stu-id="45bf3-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="45bf3-189">Sélectionnez **complexes** en tant que la valeur de retour tapez, puis spécifiez le type de retour complex spécifique en choisissant le type approprié dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="45bf3-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Modifier l’importation de fonction](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="45bf3-191">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-191">Click **OK**.</span></span>
    <span data-ttu-id="45bf3-192">L'entrée function import est créée dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="45bf3-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="45bf3-193">Personnaliser le mappage pour l’importation de fonction de colonnes</span><span class="sxs-lookup"><span data-stu-id="45bf3-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="45bf3-194">Cliquez sur l’importation de fonction dans l’Explorateur de modèles et sélectionnez **mappage d’importation de fonction**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="45bf3-195">Le **détails de Mapping** fenêtre apparaît et affiche le mappage par défaut pour l’importation de fonction.</span><span class="sxs-lookup"><span data-stu-id="45bf3-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="45bf3-196">Les flèches indiquent les mappages entre valeurs de colonne et valeurs de propriété.</span><span class="sxs-lookup"><span data-stu-id="45bf3-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="45bf3-197">Par défaut, les noms de colonne sont supposés être les mêmes que ceux de la propriété du type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="45bf3-198">Les noms de colonne par défaut s'affichent en gris.</span><span class="sxs-lookup"><span data-stu-id="45bf3-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="45bf3-199">Si nécessaire, changez les noms des colonnes pour qu'ils correspondent aux noms des colonnes retournés par la procédure stockée correspondant à l'importation de fonction.</span><span class="sxs-lookup"><span data-stu-id="45bf3-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="45bf3-200">Supprimer un Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-200">Delete a Complex Type</span></span>

<span data-ttu-id="45bf3-201">Lorsque vous supprimez un type complexe, le type est supprimé du modèle conceptuel, et les mappages pour toutes les instances du type sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="45bf3-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="45bf3-202">Toutefois, les références au type ne sont pas mises à jour.</span><span class="sxs-lookup"><span data-stu-id="45bf3-202">However, references to the type are not updated.</span></span> <span data-ttu-id="45bf3-203">Par exemple, si une entité a une propriété de type complexe de type ComplexType1 et que ComplexType1 est supprimé dans le **Explorateur de modèles**, la propriété d’entité correspondante n’est pas mis à jour.</span><span class="sxs-lookup"><span data-stu-id="45bf3-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="45bf3-204">Le modèle ne sera pas validé, car elle contient une entité qui fait référence à un type complexe supprimé.</span><span class="sxs-lookup"><span data-stu-id="45bf3-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="45bf3-205">Vous pouvez mettre à jour ou supprimer des références à des types complexes supprimés à l'aide du Concepteur d'entités.</span><span class="sxs-lookup"><span data-stu-id="45bf3-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="45bf3-206">Cliquez sur un type complexe dans l’Explorateur de modèles et sélectionnez **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="45bf3-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="45bf3-207">OR :</span><span class="sxs-lookup"><span data-stu-id="45bf3-207">OR -</span></span>

-   <span data-ttu-id="45bf3-208">Sélectionnez un type complexe dans l'Explorateur de modèles, puis appuyez sur la touche Suppr de votre clavier.</span><span class="sxs-lookup"><span data-stu-id="45bf3-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="45bf3-209">Requête pour les entités qui contient les propriétés de Type complexe</span><span class="sxs-lookup"><span data-stu-id="45bf3-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="45bf3-210">Le code suivant montre comment exécuter une requête qui retourne une collection d’entités des objets de type qui contiennent une propriété de type complexe.</span><span class="sxs-lookup"><span data-stu-id="45bf3-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
