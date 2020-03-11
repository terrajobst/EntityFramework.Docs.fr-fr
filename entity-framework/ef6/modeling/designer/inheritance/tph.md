---
title: Concepteur TPH héritage-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418426"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="cd686-102">Héritage TPH du concepteur</span><span class="sxs-lookup"><span data-stu-id="cd686-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="cd686-103">Cette procédure pas à pas montre comment implémenter l’héritage TPH (table par hiérarchie) dans votre modèle conceptuel avec l’Entity Framework Designer (concepteur EF).</span><span class="sxs-lookup"><span data-stu-id="cd686-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="cd686-104">L’héritage TPH utilise une table de base de données pour conserver les données de tous les types d’entité dans une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="cd686-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="cd686-105">Dans cette procédure pas à pas, nous allons mapper la table Person à trois types d’entité : person (type de base), Student (Derived person) et Instructor (dérive de Person).</span><span class="sxs-lookup"><span data-stu-id="cd686-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="cd686-106">Nous allons créer un modèle conceptuel à partir de la base de données (Database First), puis modifier le modèle pour implémenter l’héritage TPH à l’aide du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="cd686-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="cd686-107">Il est possible de mapper à un héritage TPH à l’aide de Model First, mais vous devez écrire votre propre flux de travail de génération de base de données, ce qui est complexe.</span><span class="sxs-lookup"><span data-stu-id="cd686-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="cd686-108">Vous affectez ensuite ce flux de travail à la propriété **de flux de travail de génération de base de données** dans le concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="cd686-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="cd686-109">Une alternative plus simple consiste à utiliser Code First.</span><span class="sxs-lookup"><span data-stu-id="cd686-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="cd686-110">Autres options d’héritage</span><span class="sxs-lookup"><span data-stu-id="cd686-110">Other Inheritance Options</span></span>

<span data-ttu-id="cd686-111">La table par type (TPT) est un autre type d’héritage dans lequel des tables distinctes de la base de données sont mappées à des entités qui participent à l’héritage.</span><span class="sxs-lookup"><span data-stu-id="cd686-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="cd686-112"> Pour plus d’informations sur la façon de mapper l’héritage TPT (table par type) à l’aide du concepteur EF, consultez [EF designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="cd686-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="cd686-113">L’héritage de type table par béton (TPC) et les modèles d’héritage mixte sont pris en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="cd686-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="cd686-114">Si vous souhaitez utiliser l’héritage TPC ou mixte, deux options s’offrent à vous : Utilisez Code First ou modifiez manuellement le fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="cd686-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="cd686-115">Si vous choisissez de travailler avec le fichier EDMX, la fenêtre Détails de mappage est placée en « mode sans échec » et vous ne pouvez pas utiliser le concepteur pour modifier les mappages.</span><span class="sxs-lookup"><span data-stu-id="cd686-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd686-116">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="cd686-116">Prerequisites</span></span>

<span data-ttu-id="cd686-117">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="cd686-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cd686-118">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd686-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="cd686-119">[Exemple de base de données School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="cd686-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cd686-120">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="cd686-120">Set up the Project</span></span>

-   <span data-ttu-id="cd686-121">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="cd686-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="cd686-122">Sélectionnez **fichier-&gt; nouveau-&gt; projet**</span><span class="sxs-lookup"><span data-stu-id="cd686-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="cd686-123">Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **console** .</span><span class="sxs-lookup"><span data-stu-id="cd686-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="cd686-124">Entrez **TPHDBFirstSample** comme nom.</span><span class="sxs-lookup"><span data-stu-id="cd686-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="cd686-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd686-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="cd686-126">Création d'un modèle</span><span class="sxs-lookup"><span data-stu-id="cd686-126">Create a Model</span></span>

-   <span data-ttu-id="cd686-127">Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez **Ajouter-&gt; nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="cd686-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="cd686-128">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.</span><span class="sxs-lookup"><span data-stu-id="cd686-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="cd686-129">Entrez **TPHModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="cd686-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="cd686-130">Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="cd686-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="cd686-131">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="cd686-131">Click **New Connection**.</span></span>
    <span data-ttu-id="cd686-132">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, (base de données locale **)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd686-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="cd686-133">La boîte de dialogue choisir votre connexion de données est mise à jour avec votre paramètre de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd686-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="cd686-134">Dans la boîte de dialogue choisir vos objets de base de données, sous le nœud tables, sélectionnez la table **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd686-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="cd686-135">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="cd686-135">Click **Finish**.</span></span>

<span data-ttu-id="cd686-136">Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché.</span><span class="sxs-lookup"><span data-stu-id="cd686-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="cd686-137">Tous les objets que vous avez sélectionnés dans la boîte de dialogue choisir vos objets de base de données sont ajoutés au modèle.</span><span class="sxs-lookup"><span data-stu-id="cd686-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="cd686-138">C’est ainsi que la table **Person** apparaît dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd686-138">That is how the **Person** table looks in the database.</span></span>

![Table Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="cd686-140">Implémenter l’héritage TPH (table par hiérarchie)</span><span class="sxs-lookup"><span data-stu-id="cd686-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="cd686-141">La table **Person** contient la colonne de **discriminateur** , qui peut avoir l’une des deux valeurs suivantes : « Student » et « Instructor ».</span><span class="sxs-lookup"><span data-stu-id="cd686-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="cd686-142">Selon la valeur, la table **Person** est mappée à l’entité **Student** ou à l’entité **Instructor** .</span><span class="sxs-lookup"><span data-stu-id="cd686-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="cd686-143">La table **Person** possède également deux colonnes : **HireDate** et **EnrollmentDate**, qui doit être **Nullable** , car une personne ne peut pas être un étudiant et un formateur en même temps (au moins dans cette procédure pas à pas).</span><span class="sxs-lookup"><span data-stu-id="cd686-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="cd686-144">Ajouter de nouvelles entités</span><span class="sxs-lookup"><span data-stu-id="cd686-144">Add new Entities</span></span>

-   <span data-ttu-id="cd686-145">Ajoutez une nouvelle entité.</span><span class="sxs-lookup"><span data-stu-id="cd686-145">Add a new entity.</span></span>
    <span data-ttu-id="cd686-146">Pour ce faire, cliquez avec le bouton droit sur un espace vide de l’aire de conception du Entity Framework Designer, puis sélectionnez **Ajouter-&gt;entité**.</span><span class="sxs-lookup"><span data-stu-id="cd686-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="cd686-147">Tapez **Instructor** pour le nom de l' **entité** et sélectionnez **Person** dans la liste déroulante du **type de base**.</span><span class="sxs-lookup"><span data-stu-id="cd686-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="cd686-148">Cliquez sur  **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd686-148">Click **OK**.</span></span>
-   <span data-ttu-id="cd686-149">Ajoutez une autre nouvelle entité.</span><span class="sxs-lookup"><span data-stu-id="cd686-149">Add another new entity.</span></span> <span data-ttu-id="cd686-150">Tapez **student** pour le nom de l' **entité** et sélectionnez **Person** dans la liste déroulante du **type de base**.</span><span class="sxs-lookup"><span data-stu-id="cd686-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="cd686-151">Deux nouveaux types d’entité ont été ajoutés à l’aire de conception.</span><span class="sxs-lookup"><span data-stu-id="cd686-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="cd686-152">Une flèche pointe des nouveaux types d’entités vers la **personne** type d’entité ; Cela indique que **Person** est le type de base pour les nouveaux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="cd686-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="cd686-153">Cliquez avec le bouton droit sur la propriété **hiredate** de l’entité **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd686-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="cd686-154">Sélectionnez **couper** (ou utilisez la touche Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="cd686-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cd686-155">Cliquez avec le bouton droit sur l’entité de l' **instructeur** , puis sélectionnez **coller** (ou utilisez la touche Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="cd686-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="cd686-156">Cliquez avec le bouton droit sur la propriété **hiredate** , puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="cd686-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="cd686-157">Dans la fenêtre  **Propriétés** , affectez la valeur **false**à la propriété **Nullable** .</span><span class="sxs-lookup"><span data-stu-id="cd686-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cd686-158">Cliquez avec le bouton droit sur la propriété **EnrollmentDate** de l’entité **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd686-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="cd686-159">Sélectionnez **couper** (ou utilisez la touche Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="cd686-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cd686-160">Cliquez avec le bouton droit sur l’entité **Student** et sélectionnez **Coller (ou utilisez la touche Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="cd686-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="cd686-161">Sélectionnez la propriété **EnrollmentDate** et affectez la valeur **false**à la propriété **Nullable** .</span><span class="sxs-lookup"><span data-stu-id="cd686-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cd686-162">Sélectionnez la **personne** type d’entité.</span><span class="sxs-lookup"><span data-stu-id="cd686-162">Select the **Person** entity type.</span></span> <span data-ttu-id="cd686-163">Dans la fenêtre **propriétés** , affectez la valeur **true**à sa propriété **abstract** .</span><span class="sxs-lookup"><span data-stu-id="cd686-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="cd686-164">Supprimez la propriété de **discriminateur** de **Person**.</span><span class="sxs-lookup"><span data-stu-id="cd686-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="cd686-165">La raison pour laquelle elle doit être supprimée est expliquée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="cd686-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="cd686-166">Mapper les entités</span><span class="sxs-lookup"><span data-stu-id="cd686-166">Map the entities</span></span>

-   <span data-ttu-id="cd686-167">Cliquez avec le bouton droit sur l' **instructeur** et sélectionnez **mappage de table.**</span><span class="sxs-lookup"><span data-stu-id="cd686-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="cd686-168">L’entité Instructor est sélectionnée dans la fenêtre Détails de mappage.</span><span class="sxs-lookup"><span data-stu-id="cd686-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="cd686-169">Cliquez sur **&lt;ajouter une table ou une vue&gt;**  dans la fenêtre des **Détails de mappage** .</span><span class="sxs-lookup"><span data-stu-id="cd686-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="cd686-170">Le **&lt;ajouter une table ou une vue&gt;** champ devient une liste déroulante de tables ou de vues dans laquelle l’entité sélectionnée peut être mappée.</span><span class="sxs-lookup"><span data-stu-id="cd686-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="cd686-171">Sélectionnez **Person** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="cd686-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="cd686-172">La fenêtre des **Détails de mappage** est mise à jour avec les mappages de colonnes par défaut et une option pour ajouter une condition.</span><span class="sxs-lookup"><span data-stu-id="cd686-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="cd686-173">Cliquez sur **&lt;ajouter une Condition&gt;** .</span><span class="sxs-lookup"><span data-stu-id="cd686-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="cd686-174">Le **&lt;ajouter une Condition&gt;** champ devient une liste déroulante des colonnes pour lesquelles des conditions peuvent être définies.</span><span class="sxs-lookup"><span data-stu-id="cd686-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="cd686-175">Sélectionnez de **discriminateur** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="cd686-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="cd686-176">Dans la colonne **opérateur** de la fenêtre des **Détails de mappage** , sélectionnez = dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="cd686-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="cd686-177">Dans la colonne **valeur/propriété** , tapez **Instructor**.</span><span class="sxs-lookup"><span data-stu-id="cd686-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="cd686-178">Le résultat final doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="cd686-178">The end result should look like this:</span></span>

    ![Détails de mappage](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="cd686-180">Répétez ces étapes pour le type d’entité  **Student** , mais faites en sorte que la condition soit égale à la valeur **Student** .</span><span class="sxs-lookup"><span data-stu-id="cd686-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="cd686-181">*La raison pour laquelle nous voulions supprimer la propriété de **discriminateur** , c’est parce que vous ne pouvez pas mapper une colonne de table plusieurs fois. Cette colonne sera utilisée pour le mappage conditionnel. elle ne peut donc pas être utilisée pour le mappage de propriété. La seule façon de pouvoir les utiliser pour les deux, si une condition utilise **est null** ou **n’est pas null** comparaison.*</span><span class="sxs-lookup"><span data-stu-id="cd686-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="cd686-182">L'héritage TPH (table par hiérarchie) est maintenant implémenté.</span><span class="sxs-lookup"><span data-stu-id="cd686-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="cd686-184">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="cd686-184">Use the Model</span></span>

<span data-ttu-id="cd686-185">Ouvrez le fichier **Program.cs** dans lequel la méthode **main** est définie.</span><span class="sxs-lookup"><span data-stu-id="cd686-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="cd686-186">Collez le code suivant dans la fonction **main** .</span><span class="sxs-lookup"><span data-stu-id="cd686-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="cd686-187">Le code exécute trois requêtes.</span><span class="sxs-lookup"><span data-stu-id="cd686-187">The code executes three queries.</span></span> <span data-ttu-id="cd686-188">La première requête rétablit tous les objets **Person** .</span><span class="sxs-lookup"><span data-stu-id="cd686-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="cd686-189">La deuxième requête utilise la méthode **OfType** pour retourner des objets de **formateur** .</span><span class="sxs-lookup"><span data-stu-id="cd686-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="cd686-190">La troisième requête utilise la méthode **OfType** pour retourner les objets **Student** .</span><span class="sxs-lookup"><span data-stu-id="cd686-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
