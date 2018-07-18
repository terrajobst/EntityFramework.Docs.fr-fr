---
title: Fractionnement de la Table concepteur - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120822"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="78e7b-102">Table du Concepteur de fractionnement</span><span class="sxs-lookup"><span data-stu-id="78e7b-102">Designer Table Splitting</span></span>
<span data-ttu-id="78e7b-103">Cette procédure pas à pas montre comment mapper plusieurs types d’entités à une table unique en modifiant un modèle avec Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="78e7b-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="78e7b-104">L’une des raisons que vous souhaiterez utiliser le fractionnement de table sont ce qui peut retarder le chargement de certaines propriétés lorsque vous utilisez le chargement pour charger vos objets différé.</span><span class="sxs-lookup"><span data-stu-id="78e7b-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="78e7b-105">Vous pouvez séparer les propriétés qui peuvent contenir de très grande quantité de données dans une entité distincte et chargez uniquement si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="78e7b-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="78e7b-106">L’illustration suivante montre les principales fenêtres qui sont utilisées lorsque vous travaillez avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="78e7b-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="78e7b-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="78e7b-108">Prerequisites</span></span>

<span data-ttu-id="78e7b-109">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="78e7b-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="78e7b-110">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78e7b-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="78e7b-111">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="78e7b-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="78e7b-112">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="78e7b-112">Set up the Project</span></span>

<span data-ttu-id="78e7b-113">Cette procédure pas à pas utilise Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="78e7b-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="78e7b-114">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="78e7b-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="78e7b-115">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="78e7b-116">Dans le volet gauche, cliquez sur Visual C\#, puis sélectionnez le modèle Application Console.</span><span class="sxs-lookup"><span data-stu-id="78e7b-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="78e7b-117">Entrez **TableSplittingSample** en tant que le nom du projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="78e7b-118">Créer un modèle basé sur la base de données School</span><span class="sxs-lookup"><span data-stu-id="78e7b-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="78e7b-119">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="78e7b-120">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="78e7b-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="78e7b-121">Entrez **TableSplittingModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="78e7b-122">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**</span><span class="sxs-lookup"><span data-stu-id="78e7b-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="78e7b-123">Cliquez sur Nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="78e7b-123">Click New Connection.</span></span> <span data-ttu-id="78e7b-124">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="78e7b-125">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="78e7b-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="78e7b-126">Dans la boîte de dialogue Choisir vos objets de base de données, dérouler les **Tables** nœud et vérifiez la **personne** table.</span><span class="sxs-lookup"><span data-stu-id="78e7b-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="78e7b-127">Cette opération ajoute la table spécifiée à la **School** modèle.</span><span class="sxs-lookup"><span data-stu-id="78e7b-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="78e7b-128">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-128">Click **Finish**.</span></span>

<span data-ttu-id="78e7b-129">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="78e7b-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="78e7b-130">Tous les objets que vous avez sélectionné dans le **choisir vos objets de base de données** boîte de dialogue sont ajoutés au modèle.</span><span class="sxs-lookup"><span data-stu-id="78e7b-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="78e7b-131">Mapper des deux entités à une seule Table</span><span class="sxs-lookup"><span data-stu-id="78e7b-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="78e7b-132">Dans cette section, vous allez fractionner la **personne** entité en deux entités puis des mapper à une seule table.</span><span class="sxs-lookup"><span data-stu-id="78e7b-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="78e7b-133">Le **personne** entité ne contient pas toutes les propriétés qui peuvent contenir des données volumineuses ; il est simplement utilisé comme un exemple.</span><span class="sxs-lookup"><span data-stu-id="78e7b-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="78e7b-134">Cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **entité**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="78e7b-135">Le **nouvelle entité** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="78e7b-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="78e7b-136">Type **HireInfo** pour le **nom de l’entité** et **PersonID** pour le **propriété Key** nom.</span><span class="sxs-lookup"><span data-stu-id="78e7b-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="78e7b-137">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-137">Click **OK**.</span></span>
-   <span data-ttu-id="78e7b-138">Un nouveau type d'entité est créé et affiché sur l'aire de conception.</span><span class="sxs-lookup"><span data-stu-id="78e7b-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="78e7b-139">Sélectionnez le **HireDate** propriété de la **personne** type d’entité, puis appuyez sur **Ctrl + X** clés.</span><span class="sxs-lookup"><span data-stu-id="78e7b-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="78e7b-140">Sélectionnez le **HireInfo** entité, puis appuyez sur **Ctrl + V** clés.</span><span class="sxs-lookup"><span data-stu-id="78e7b-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="78e7b-141">Créer une association entre **personne** et **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="78e7b-142">Pour ce faire, cliquez sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **Association**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="78e7b-143">Le **ajouter une Association** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="78e7b-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="78e7b-144">Le **PersonHireInfo** nom est attribué par défaut.</span><span class="sxs-lookup"><span data-stu-id="78e7b-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="78e7b-145">Spécifier la multiplicité **1(One)** aux deux extrémités de la relation.</span><span class="sxs-lookup"><span data-stu-id="78e7b-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="78e7b-146">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-146">Press **OK**.</span></span>

<span data-ttu-id="78e7b-147">L’étape suivante requiert la **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="78e7b-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="78e7b-148">Si vous ne voyez pas cette fenêtre, cliquez sur l’aire de conception et sélectionnez **détails de Mapping**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="78e7b-149">Sélectionnez le **HireInfo** type d’entité et cliquez sur **&lt;ajouter une Table ou vue&gt;** dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="78e7b-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="78e7b-150">Sélectionnez **personne** à partir de la **&lt;ajouter une Table ou vue&gt;** liste de champs de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="78e7b-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="78e7b-151">La liste contient des tables ou des vues, auquel l’entité sélectionnée peut être mappée.</span><span class="sxs-lookup"><span data-stu-id="78e7b-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="78e7b-152">Les propriétés appropriées doivent être mappées par défaut.</span><span class="sxs-lookup"><span data-stu-id="78e7b-152">The appropriate properties should be mapped by default.</span></span>

    ![Mappage](~/ef6/media/mapping.png)

-   <span data-ttu-id="78e7b-154">Sélectionnez le **PersonHireInfo** association sur l’aire de conception.</span><span class="sxs-lookup"><span data-stu-id="78e7b-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="78e7b-155">Avec le bouton droit sur l’aire de conception et sélectionnez l’association de **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="78e7b-156">Dans le **propriétés** fenêtre, sélectionnez le **contraintes référentielles** propriété et cliquez sur le bouton de sélection.</span><span class="sxs-lookup"><span data-stu-id="78e7b-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="78e7b-157">Sélectionnez **personne** à partir de la **Principal** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="78e7b-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="78e7b-158">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e7b-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="78e7b-159">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="78e7b-159">Use the Model</span></span>

-   <span data-ttu-id="78e7b-160">Collez le code suivant dans la méthode Main.</span><span class="sxs-lookup"><span data-stu-id="78e7b-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="78e7b-161">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="78e7b-161">Compile and run the application.</span></span>

<span data-ttu-id="78e7b-162">Les instructions T-SQL suivantes ont été exécutées sur le **School** base de données suite à l’exécution de cette application.</span><span class="sxs-lookup"><span data-stu-id="78e7b-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="78e7b-163">Ce qui suit **insérer** a été exécutée suite à l’exécution de contexte. SaveChanges() et combine les données à partir de la **personne** et **HireInfo** entités</span><span class="sxs-lookup"><span data-stu-id="78e7b-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="78e7b-165">Ce qui suit **sélectionnez** a été exécutée suite à l’exécution de contexte. People.FirstOrDefault() et sélectionne uniquement les colonnes mappées aux **personne**</span><span class="sxs-lookup"><span data-stu-id="78e7b-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="78e7b-167">Ce qui suit **sélectionnez** a été exécutée à la suite de l’accès à la existingPerson.Instructor de propriété de navigation et sélectionne uniquement les colonnes mappées aux **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="78e7b-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
