---
title: Relations - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994646"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="120c3-102">Relations - Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="120c3-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="120c3-103">Cette page fournit des informations sur la définition des relations dans votre modèle à l’aide du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="120c3-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="120c3-104">Pour obtenir des informations générales sur les relations dans Entity Framework et comment accéder à et manipuler des données à l’aide de relations, consultez [relations & Propriétés de Navigation](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="120c3-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="120c3-105">Les associations définissent les relations entre les types d’entité dans un modèle.</span><span class="sxs-lookup"><span data-stu-id="120c3-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="120c3-106">Cette rubrique montre comment mapper des associations avec Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="120c3-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="120c3-107">L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="120c3-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="120c3-109">Lorsque vous générez le modèle conceptuel, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="120c3-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="120c3-110">Vous pouvez ignorer ces avertissements, car une fois que vous choisissez de générer la base de données à partir du modèle, les erreurs disparaîtront.</span><span class="sxs-lookup"><span data-stu-id="120c3-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="120c3-111">Vue d’ensemble d’associations</span><span class="sxs-lookup"><span data-stu-id="120c3-111">Associations Overview</span></span>

<span data-ttu-id="120c3-112">Lorsque vous concevez votre modèle à l’aide de l’Entity Framework Designer, un fichier .edmx représente votre modèle.</span><span class="sxs-lookup"><span data-stu-id="120c3-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="120c3-113">Dans le fichier .edmx, un **Association** élément définit une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="120c3-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="120c3-114">Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ».</span><span class="sxs-lookup"><span data-stu-id="120c3-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="120c3-115">La multiplicité d’une terminaison d’association peut avoir une valeur d’un (1), zéro ou un (0.. 1) ou plusieurs (\*).</span><span class="sxs-lookup"><span data-stu-id="120c3-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="120c3-116">Ces informations sont spécifiées dans les deux enfants **fin** éléments.</span><span class="sxs-lookup"><span data-stu-id="120c3-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="120c3-117">Au moment de l’exécution, instances de type d’entité à une extrémité d’une association sont accessible via les propriétés de navigation ou de clés étrangères (si vous choisissez d’exposer les clés étrangères dans vos entités).</span><span class="sxs-lookup"><span data-stu-id="120c3-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="120c3-118">Avec des clés étrangères exposées, la relation entre les entités est gérée avec un **ReferentialConstraint** élément (un élément enfant de le **Association** élément).</span><span class="sxs-lookup"><span data-stu-id="120c3-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="120c3-119">Il est recommandé de toujours exposer les clés étrangères pour les relations dans vos entités.</span><span class="sxs-lookup"><span data-stu-id="120c3-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="120c3-120">Dans plusieurs-à-plusieurs (\*:\*) vous ne pouvez pas ajouter de clés étrangères aux entités.</span><span class="sxs-lookup"><span data-stu-id="120c3-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="120c3-121">Dans un \*:\* relation, les informations d’association est gérée avec un objet indépendant.</span><span class="sxs-lookup"><span data-stu-id="120c3-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="120c3-122">Pour plus d’informations sur les éléments du langage CSDL (**ReferentialConstraint**, **Association**, etc.) consultez le [spécification CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="120c3-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="120c3-123">Créer et supprimer des Associations</span><span class="sxs-lookup"><span data-stu-id="120c3-123">Create and Delete Associations</span></span>

<span data-ttu-id="120c3-124">Création d’une association avec les mises à jour du Concepteur EF le contenu du modèle du fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="120c3-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="120c3-125">Après avoir créé une association, vous devez créer les mappages pour l’association (décrits plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="120c3-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="120c3-126">Cette section suppose que vous avez déjà ajouté les entités que vous souhaitez créer une association entre à votre modèle.</span><span class="sxs-lookup"><span data-stu-id="120c3-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="120c3-127">Pour créer une association</span><span class="sxs-lookup"><span data-stu-id="120c3-127">To create an association</span></span>

1.  <span data-ttu-id="120c3-128">Cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis sélectionnez **Association...** .</span><span class="sxs-lookup"><span data-stu-id="120c3-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="120c3-129">Définissez les paramètres pour l’association dans le **ajouter une Association** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="120c3-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="120c3-131">Vous pouvez choisir de ne pas ajouter les propriétés de navigation ou les propriétés de clé étrangère aux entités situées aux terminaisons de l’association en désactivant le ** propriété de Navigation ** et ** ajouter des propriétés de clé étrangères pour la &lt;nom de type d’entité&gt; entité ** cases à cocher.</span><span class="sxs-lookup"><span data-stu-id="120c3-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="120c3-132">Si vous ajoutez une seule propriété de navigation, l'association n'est parcourable que dans une seule direction.</span><span class="sxs-lookup"><span data-stu-id="120c3-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="120c3-133">Si vous n'ajoutez pas de propriétés de navigation, vous devez ajouter des propriétés de clé étrangère pour accéder aux entités au niveau des terminaisons de l'association.</span><span class="sxs-lookup"><span data-stu-id="120c3-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="120c3-134">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="120c3-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="120c3-135">Pour supprimer une association</span><span class="sxs-lookup"><span data-stu-id="120c3-135">To delete an association</span></span>

<span data-ttu-id="120c3-136">Pour supprimer une association, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="120c3-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="120c3-137">Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez l’association de **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="120c3-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="120c3-138">OR :</span><span class="sxs-lookup"><span data-stu-id="120c3-138">OR -</span></span>

-   <span data-ttu-id="120c3-139">Sélectionnez une ou plusieurs associations et appuyez sur la touche SUPPR.</span><span class="sxs-lookup"><span data-stu-id="120c3-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="120c3-140">Inclure les propriétés de clé étrangère dans vos entités (contraintes référentielles)</span><span class="sxs-lookup"><span data-stu-id="120c3-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="120c3-141">Il est recommandé de toujours exposer les clés étrangères pour les relations dans vos entités.</span><span class="sxs-lookup"><span data-stu-id="120c3-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="120c3-142">Entity Framework utilise une contrainte référentielle pour identifier qu’une propriété agit comme la clé étrangère d’une relation.</span><span class="sxs-lookup"><span data-stu-id="120c3-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="120c3-143">Si vous avez coché la ***ajouter des propriétés de clé étrangères pour la &lt;nom de type d’entité&gt; entité*** case à cocher lorsque vous créez une relation, cette contrainte référentielle a été ajoutée pour vous.</span><span class="sxs-lookup"><span data-stu-id="120c3-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="120c3-144">Lorsque vous utilisez le Concepteur EF pour ajouter ou modifier une contrainte référentielle, le Concepteur EF ajoute ou modifie un **ReferentialConstraint** élément dans le contenu CSDL du fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="120c3-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="120c3-145">Double-cliquez sur l'association à modifier.</span><span class="sxs-lookup"><span data-stu-id="120c3-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="120c3-146">Le **contrainte référentielle** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="120c3-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="120c3-147">À partir de la **Principal** liste déroulante, sélectionnez l’entité principale dans la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="120c3-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="120c3-148">Propriétés de clé de l’entité sont ajoutées à la **clé du principal du** liste dans la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="120c3-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="120c3-149">À partir de la **dépendants** liste déroulante, sélectionnez l’entité dépendante dans la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="120c3-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="120c3-150">Pour chaque clé principale qui a une clé dépendante, sélectionnez une clé dépendante correspondante dans les listes déroulantes dans le **clé dépendante** colonne.</span><span class="sxs-lookup"><span data-stu-id="120c3-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="120c3-152">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="120c3-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="120c3-153">créer et modifier des mappages d'association</span><span class="sxs-lookup"><span data-stu-id="120c3-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="120c3-154">Vous pouvez spécifier comment une association est mappé à la base de données dans le **détails de Mapping** fenêtre du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="120c3-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="120c3-155">Vous ne pouvez mapper que des détails pour les associations qui n’ont pas une contrainte référentielle spécifiée.</span><span class="sxs-lookup"><span data-stu-id="120c3-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="120c3-156">Si une contrainte référentielle n’est spécifiée une propriété de clé étrangère est incluse dans l’entité, puis vous pouvez utiliser les détails de mappage pour l’entité au contrôle de la clé étrangère de la colonne qui mappe à.</span><span class="sxs-lookup"><span data-stu-id="120c3-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="120c3-157">Créer un mappage d’association</span><span class="sxs-lookup"><span data-stu-id="120c3-157">Create an association mapping</span></span>

-   <span data-ttu-id="120c3-158">Cliquez sur une association dans l’aire de conception et sélectionnez **mappage de Table**.</span><span class="sxs-lookup"><span data-stu-id="120c3-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="120c3-159">Cette opération affiche le mappage d’association dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="120c3-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="120c3-160">Cliquez sur **ajouter une Table ou vue**.</span><span class="sxs-lookup"><span data-stu-id="120c3-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="120c3-161">Une liste déroulante s'affiche. Elle contient toutes les tables du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="120c3-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="120c3-162">Sélectionnez la table à laquelle l'association doit être mappée.</span><span class="sxs-lookup"><span data-stu-id="120c3-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="120c3-163">Le **détails de Mapping** fenêtre affiche les deux terminaisons de l’association et les propriétés de clé pour le type d’entité à chaque **fin**.</span><span class="sxs-lookup"><span data-stu-id="120c3-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="120c3-164">Pour chaque propriété de clé, cliquez sur le **colonne** champ, puis sélectionnez la colonne à laquelle la propriété doit être mappée.</span><span class="sxs-lookup"><span data-stu-id="120c3-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="120c3-166">Modifier un mappage d’association</span><span class="sxs-lookup"><span data-stu-id="120c3-166">Edit an association mapping</span></span>

-   <span data-ttu-id="120c3-167">Cliquez sur une association dans l’aire de conception et sélectionnez **mappage de Table**.</span><span class="sxs-lookup"><span data-stu-id="120c3-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="120c3-168">Cette opération affiche le mappage d’association dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="120c3-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="120c3-169">Cliquez sur **est mappé à &lt;nom de la Table&gt;**.</span><span class="sxs-lookup"><span data-stu-id="120c3-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="120c3-170">Une liste déroulante s'affiche. Elle contient toutes les tables du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="120c3-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="120c3-171">Sélectionnez la table à laquelle l'association doit être mappée.</span><span class="sxs-lookup"><span data-stu-id="120c3-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="120c3-172">Le **détails de Mapping** fenêtre affiche les deux terminaisons de l’association et les propriétés de clé pour le type d’entité à chaque extrémité.</span><span class="sxs-lookup"><span data-stu-id="120c3-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="120c3-173">Pour chaque propriété de clé, cliquez sur le **colonne** champ, puis sélectionnez la colonne à laquelle la propriété doit être mappée.</span><span class="sxs-lookup"><span data-stu-id="120c3-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="120c3-174">Modifier et supprimer les propriétés de Navigation</span><span class="sxs-lookup"><span data-stu-id="120c3-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="120c3-175">Propriétés de navigation sont des propriétés de raccourci utilisées pour rechercher les entités situées aux terminaisons d’une association dans un modèle.</span><span class="sxs-lookup"><span data-stu-id="120c3-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="120c3-176">Il est possible de créer des propriétés de navigation lorsque vous créez une association entre deux types d'entité.</span><span class="sxs-lookup"><span data-stu-id="120c3-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="120c3-177">Pour modifier les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="120c3-177">To edit navigation properties</span></span>

-   <span data-ttu-id="120c3-178">Sélectionnez une propriété de navigation sur l’aire du Concepteur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="120c3-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="120c3-179">Informations sur la propriété de navigation s’affichent dans Visual Studio **propriétés** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="120c3-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="120c3-180">Modifier les paramètres de propriété dans le **propriétés** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="120c3-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="120c3-181">Supprimer les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="120c3-181">To delete navigation properties</span></span>

-   <span data-ttu-id="120c3-182">Si les clés étrangères ne sont pas exposées sur les types d'entité dans le modèle conceptuel, le fait de supprimer une propriété de navigation peut rendre l'association correspondante parcourable dans une seule direction ou pas parcourable du tout.</span><span class="sxs-lookup"><span data-stu-id="120c3-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="120c3-183">Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une propriété de navigation **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="120c3-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
