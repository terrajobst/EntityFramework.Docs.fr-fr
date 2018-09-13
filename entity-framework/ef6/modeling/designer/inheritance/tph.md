---
title: Héritage TPH concepteur - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490111"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="4076b-102">Héritage TPH Concepteur</span><span class="sxs-lookup"><span data-stu-id="4076b-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="4076b-103">Cette procédure pas à pas montre comment implémenter l’héritage de table par hiérarchie (TPH) dans votre modèle conceptuel avec Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="4076b-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="4076b-104">L’héritage TPH utilise une table de base de données pour gérer les données de tous les types d’entités dans une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="4076b-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="4076b-105">Dans cette procédure pas à pas, nous allons mapper la table Person à trois types d’entités : Person (type de base), Student (dérive de personne) et Instructor (dérive de personne).</span><span class="sxs-lookup"><span data-stu-id="4076b-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="4076b-106">Nous allons créer un modèle conceptuel à partir de la base de données (Database First), puis la modifier le modèle pour implémenter l’héritage TPH à l’aide du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="4076b-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="4076b-107">Il est possible de mapper à un héritage TPH à l’aide de Model First, mais vous seriez obligé d’écrire vos propres flux de travail de génération de base de données qui est complexe.</span><span class="sxs-lookup"><span data-stu-id="4076b-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="4076b-108">Il vous faut ensuite attribuer ce flux de travail pour le **flux de génération de base de données** propriété dans le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="4076b-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="4076b-109">Une alternative plus simple consiste à utiliser Code First.</span><span class="sxs-lookup"><span data-stu-id="4076b-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="4076b-110">Autres Options d’héritage</span><span class="sxs-lookup"><span data-stu-id="4076b-110">Other Inheritance Options</span></span>

<span data-ttu-id="4076b-111">Table par Type (TPT) est un autre type d’héritage dans laquelle des tables distinctes de la base de données sont mappées aux entités qui participent à l’héritage.</span><span class="sxs-lookup"><span data-stu-id="4076b-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="4076b-112">Pour plus d’informations sur le mappage d’héritage Table par Type avec le Concepteur d’Entity Framework, consultez [l’héritage TPT de Concepteur EF](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="4076b-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="4076b-113">L’héritage de Type de table par concret (TPC) et les modèles d’héritage mixte sont prises en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="4076b-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="4076b-114">Si vous souhaitez utiliser TPC ou mixte de l’héritage, vous avez deux options : utiliser Code First, ou modifier manuellement le fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="4076b-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="4076b-115">Si vous choisissez d’utiliser le fichier EDMX, la fenêtre Détails de mappage est placée dans le « mode sans échec » et vous ne serez pas en mesure d’utiliser le concepteur pour modifier les mappages.</span><span class="sxs-lookup"><span data-stu-id="4076b-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4076b-116">Prérequis</span><span class="sxs-lookup"><span data-stu-id="4076b-116">Prerequisites</span></span>

<span data-ttu-id="4076b-117">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4076b-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="4076b-118">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4076b-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="4076b-119">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="4076b-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="4076b-120">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="4076b-120">Set up the Project</span></span>

-   <span data-ttu-id="4076b-121">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="4076b-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="4076b-122">Sélectionnez **fichier -&gt; nouveau -&gt; projet**</span><span class="sxs-lookup"><span data-stu-id="4076b-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="4076b-123">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="4076b-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="4076b-124">Entrez **TPHDBFirstSample** comme nom.</span><span class="sxs-lookup"><span data-stu-id="4076b-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="4076b-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="4076b-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="4076b-126">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="4076b-126">Create a Model</span></span>

-   <span data-ttu-id="4076b-127">Cliquez sur le nom du projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="4076b-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="4076b-128">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="4076b-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="4076b-129">Entrez **TPHModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4076b-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="4076b-130">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="4076b-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="4076b-131">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="4076b-131">Click **New Connection**.</span></span>
    <span data-ttu-id="4076b-132">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4076b-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="4076b-133">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="4076b-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="4076b-134">Dans la boîte de dialogue Choisir vos objets de base de données, sous le nœud Tables, sélectionnez le **personne** table.</span><span class="sxs-lookup"><span data-stu-id="4076b-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="4076b-135">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="4076b-135">Click **Finish**.</span></span>

<span data-ttu-id="4076b-136">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="4076b-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="4076b-137">Tous les objets que vous avez sélectionné dans la boîte de dialogue Choisir vos objets de base de données sont ajoutées au modèle.</span><span class="sxs-lookup"><span data-stu-id="4076b-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="4076b-138">Autrement dit la **personne** table se présente dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4076b-138">That is how the **Person** table looks in the database.</span></span>

![Table Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="4076b-140">Implémenter l’héritage Table par hiérarchie</span><span class="sxs-lookup"><span data-stu-id="4076b-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="4076b-141">Le **personne** table comporte le **discriminateur** colonne, ce qui peut avoir une des deux valeurs : « L’étudiant » et « Instructor ».</span><span class="sxs-lookup"><span data-stu-id="4076b-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="4076b-142">Selon la valeur la **personne** table sera mappée à la **étudiant** entité ou le **Instructor** entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="4076b-143">Le **personne** table possède également deux colonnes, **HireDate** et **EnrollmentDate**, qui doit être **nullable** , car une personne ne peut pas être un stagiaire et formateur en même temps (au moins pas dans cette procédure pas à pas).</span><span class="sxs-lookup"><span data-stu-id="4076b-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="4076b-144">Ajouter de nouvelles entités</span><span class="sxs-lookup"><span data-stu-id="4076b-144">Add new Entities</span></span>

-   <span data-ttu-id="4076b-145">Ajouter une nouvelle entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-145">Add a new entity.</span></span>
    <span data-ttu-id="4076b-146">Pour ce faire, avec le bouton droit sur un espace vide de l’aire de conception d’Entity Framework Designer et sélectionnez **Add -&gt;entité**.</span><span class="sxs-lookup"><span data-stu-id="4076b-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="4076b-147">Type **Instructor** pour le **nom de l’entité** et sélectionnez **personne** dans la liste déroulante pour le **type de Base**.</span><span class="sxs-lookup"><span data-stu-id="4076b-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="4076b-148">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4076b-148">Click **OK**.</span></span>
-   <span data-ttu-id="4076b-149">Ajouter une autre nouvelle entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-149">Add another new entity.</span></span> <span data-ttu-id="4076b-150">Type **étudiant** pour le **nom de l’entité** et sélectionnez **personne** dans la liste déroulante pour le **type de Base**.</span><span class="sxs-lookup"><span data-stu-id="4076b-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="4076b-151">Deux nouveaux types d’entités ont été ajoutés à l’aire de conception.</span><span class="sxs-lookup"><span data-stu-id="4076b-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="4076b-152">Une flèche pointe à partir de nouveaux types d’entités pour le **personne** type d’entité ; cela indique que **personne** est le type de base pour les nouveaux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="4076b-153">Avec le bouton droit le **HireDate** propriété de la **personne** entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="4076b-154">Sélectionnez **couper** (ou utilisez la touche Ctrl + X).</span><span class="sxs-lookup"><span data-stu-id="4076b-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="4076b-155">Avec le bouton droit le **Instructor** entité, puis sélectionnez **coller** (ou utilisez la touche Ctrl + V).</span><span class="sxs-lookup"><span data-stu-id="4076b-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="4076b-156">Cliquez sur le **HireDate** propriété et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="4076b-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="4076b-157">Dans le **propriétés** fenêtre, définissez la **Nullable** propriété **false**.</span><span class="sxs-lookup"><span data-stu-id="4076b-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="4076b-158">Avec le bouton droit le **EnrollmentDate** propriété de la **personne** entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="4076b-159">Sélectionnez **couper** (ou utilisez la touche Ctrl + X).</span><span class="sxs-lookup"><span data-stu-id="4076b-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="4076b-160">Avec le bouton droit le **étudiant** entité, puis sélectionnez **Coller (ou clé d’utilisation le Ctrl + V).**</span><span class="sxs-lookup"><span data-stu-id="4076b-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="4076b-161">Sélectionnez le **EnrollmentDate** propriété et définissez la **Nullable** propriété **false**.</span><span class="sxs-lookup"><span data-stu-id="4076b-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="4076b-162">Sélectionnez le **personne** type d’entité.</span><span class="sxs-lookup"><span data-stu-id="4076b-162">Select the **Person** entity type.</span></span> <span data-ttu-id="4076b-163">Dans le **propriétés** fenêtre, définissez son **abstraite** propriété **true**.</span><span class="sxs-lookup"><span data-stu-id="4076b-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="4076b-164">Supprimer le **discriminateur** propriété à partir de **personne**.</span><span class="sxs-lookup"><span data-stu-id="4076b-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="4076b-165">La raison pour laquelle qu'il doit être supprimé est expliqué dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="4076b-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="4076b-166">Mapper les entités</span><span class="sxs-lookup"><span data-stu-id="4076b-166">Map the entities</span></span>

-   <span data-ttu-id="4076b-167">Avec le bouton droit le **Instructor** et sélectionnez **mappage de Table.**</span><span class="sxs-lookup"><span data-stu-id="4076b-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="4076b-168">L’entité Instructor est sélectionnée dans la fenêtre Détails de Mapping.</span><span class="sxs-lookup"><span data-stu-id="4076b-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="4076b-169">Cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="4076b-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="4076b-170">Le **&lt;ajouter une Table ou vue&gt;** champ devient une liste déroulante de tables ou des vues, auquel l’entité sélectionnée peut être mappée.</span><span class="sxs-lookup"><span data-stu-id="4076b-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="4076b-171">Sélectionnez **personne** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="4076b-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="4076b-172">Le **détails de Mapping** fenêtre est mis à jour avec les mappages de colonnes par défaut et une option permettant d’ajouter une condition.</span><span class="sxs-lookup"><span data-stu-id="4076b-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="4076b-173">Cliquez sur  **&lt;ajouter une Condition&gt;**.</span><span class="sxs-lookup"><span data-stu-id="4076b-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="4076b-174">Le **&lt;ajouter une Condition&gt;** champ devient une liste déroulante des colonnes pour lesquelles des conditions peuvent être définies.</span><span class="sxs-lookup"><span data-stu-id="4076b-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="4076b-175">Sélectionnez **discriminateur** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="4076b-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="4076b-176">Dans le **opérateur** colonne de la **détails de Mapping** fenêtre, sélectionnez = dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="4076b-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="4076b-177">Dans le **valeur/propriété** colonne, tapez **Instructor**.</span><span class="sxs-lookup"><span data-stu-id="4076b-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="4076b-178">Le résultat final doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="4076b-178">The end result should look like this:</span></span>

    ![Détails de mappage](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="4076b-180">Répétez ces étapes pour le **étudiant** type d’entité, mais vérifiez la condition égale à **étudiant** valeur.</span><span class="sxs-lookup"><span data-stu-id="4076b-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="4076b-181">*La raison pour laquelle nous souhaitons supprimer le **discriminateur** propriété, est, car vous ne pouvez pas mapper une colonne de table plusieurs fois. Cette colonne sera utilisée pour le mappage conditionnel, afin qu’il ne peut pas être utilisé pour le mappage de propriété ainsi. La seule façon, il peut être utilisé pour les deux, si une condition utilise une **Is Null** ou **Is Not Null** comparaison.*</span><span class="sxs-lookup"><span data-stu-id="4076b-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="4076b-182">L'héritage TPH (table par hiérarchie) est maintenant implémenté.</span><span class="sxs-lookup"><span data-stu-id="4076b-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH finale](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="4076b-184">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="4076b-184">Use the Model</span></span>

<span data-ttu-id="4076b-185">Ouvrez le **Program.cs** fichier où le **Main** méthode est définie.</span><span class="sxs-lookup"><span data-stu-id="4076b-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="4076b-186">Collez le code suivant dans le **Main** (fonction).</span><span class="sxs-lookup"><span data-stu-id="4076b-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="4076b-187">Le code s’exécute trois requêtes.</span><span class="sxs-lookup"><span data-stu-id="4076b-187">The code executes three queries.</span></span> <span data-ttu-id="4076b-188">La première requête renvoie tous les **personne** objets.</span><span class="sxs-lookup"><span data-stu-id="4076b-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="4076b-189">La deuxième requête utilise le **OfType** méthode pour retourner **Instructor** objets.</span><span class="sxs-lookup"><span data-stu-id="4076b-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="4076b-190">La troisième requête utilise le **OfType** méthode pour retourner **étudiant** objets.</span><span class="sxs-lookup"><span data-stu-id="4076b-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
