---
title: Fractionnement de table du concepteur-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418167"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="292c6-102">Fractionnement des tables du concepteur</span><span class="sxs-lookup"><span data-stu-id="292c6-102">Designer Table Splitting</span></span>
<span data-ttu-id="292c6-103">Cette procédure pas à pas montre comment mapper plusieurs types d’entité à une seule table en modifiant un modèle avec le Entity Framework Designer (concepteur EF).</span><span class="sxs-lookup"><span data-stu-id="292c6-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="292c6-104">L’une des raisons pour lesquelles vous pouvez souhaiter utiliser le fractionnement de table est de retarder le chargement de certaines propriétés lors de l’utilisation du chargement différé pour charger vos objets.</span><span class="sxs-lookup"><span data-stu-id="292c6-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="292c6-105"> Vous pouvez séparer les propriétés qui peuvent contenir de très grandes quantités de données dans une entité distincte et les charger uniquement lorsque cela est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="292c6-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="292c6-106">L’illustration suivante montre les fenêtres principales qui sont utilisées lors de l’utilisation du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="292c6-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="292c6-108">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="292c6-108">Prerequisites</span></span>

<span data-ttu-id="292c6-109">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="292c6-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="292c6-110">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="292c6-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="292c6-111">[Exemple de base de données School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="292c6-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="292c6-112">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="292c6-112">Set up the Project</span></span>

<span data-ttu-id="292c6-113">Cette procédure pas à pas utilise Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="292c6-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="292c6-114">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="292c6-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="292c6-115">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="292c6-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="292c6-116">Dans le volet gauche, cliquez sur Visual C\#, puis sélectionnez le modèle application console.</span><span class="sxs-lookup"><span data-stu-id="292c6-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="292c6-117">Entrez **TableSplittingSample** comme nom du projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="292c6-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="292c6-118">Créer un modèle basé sur la base de données School</span><span class="sxs-lookup"><span data-stu-id="292c6-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="292c6-119">Cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="292c6-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="292c6-120">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.</span><span class="sxs-lookup"><span data-stu-id="292c6-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="292c6-121">Entrez **TableSplittingModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="292c6-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="292c6-122">Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant.**</span><span class="sxs-lookup"><span data-stu-id="292c6-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="292c6-123">Cliquez sur nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="292c6-123">Click New Connection.</span></span> <span data-ttu-id="292c6-124">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, (base de données locale **)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="292c6-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="292c6-125">La boîte de dialogue choisir votre connexion de données est mise à jour avec votre paramètre de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="292c6-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="292c6-126">Dans la boîte de dialogue choisir vos objets de base de données, dérouler les **Tables** nœud et vérifier la table **Person** .</span><span class="sxs-lookup"><span data-stu-id="292c6-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="292c6-127">Cette opération ajoute la table spécifiée au modèle **School** .</span><span class="sxs-lookup"><span data-stu-id="292c6-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="292c6-128">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="292c6-128">Click **Finish**.</span></span>

<span data-ttu-id="292c6-129">Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché.</span><span class="sxs-lookup"><span data-stu-id="292c6-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="292c6-130">Tous les objets que vous avez sélectionnés dans la boîte **de dialogue choisir vos objets de base de données** sont ajoutés au modèle.</span><span class="sxs-lookup"><span data-stu-id="292c6-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="292c6-131">Mapper deux entités à une table unique</span><span class="sxs-lookup"><span data-stu-id="292c6-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="292c6-132">Dans cette section, vous allez fractionner l’entité **Person** en deux entités, puis les mapper à une table unique.</span><span class="sxs-lookup"><span data-stu-id="292c6-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="292c6-133">L’entité **Person** ne contient pas de propriétés pouvant contenir une grande quantité de données ; elle est utilisée à titre d’exemple.</span><span class="sxs-lookup"><span data-stu-id="292c6-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="292c6-134">Cliquez avec le bouton droit sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **entité**.</span><span class="sxs-lookup"><span data-stu-id="292c6-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="292c6-135">La boîte **de dialogue nouvelle** d’entité s’affiche.</span><span class="sxs-lookup"><span data-stu-id="292c6-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="292c6-136">Tapez **HireInfo** pour le nom de l' **entité** et **PersonID** pour le nom de la **propriété de clé** .</span><span class="sxs-lookup"><span data-stu-id="292c6-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="292c6-137">Cliquez sur  **OK**.</span><span class="sxs-lookup"><span data-stu-id="292c6-137">Click **OK**.</span></span>
-   <span data-ttu-id="292c6-138">Un nouveau type d'entité est créé et affiché sur l'aire de conception.</span><span class="sxs-lookup"><span data-stu-id="292c6-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="292c6-139">Sélectionnez la propriété **hiredate** de la **personne** type d’entité, puis appuyez sur **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="292c6-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="292c6-140">Sélectionnez l’entité **HireInfo** et appuyez sur les touches **Ctrl + V** .</span><span class="sxs-lookup"><span data-stu-id="292c6-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="292c6-141">Créer une association entre **Person** et **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="292c6-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="292c6-142">Pour ce faire, cliquez avec le bouton droit sur une zone vide de l’aire de conception, pointez sur **Ajouter nouveau**, puis cliquez sur **Association**.</span><span class="sxs-lookup"><span data-stu-id="292c6-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="292c6-143">La boîte de dialogue Ajouter un d' **Association** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="292c6-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="292c6-144">Le nom **PersonHireInfo** est donné par défaut.</span><span class="sxs-lookup"><span data-stu-id="292c6-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="292c6-145">Spécifiez la multiplicité **1 (un)** aux deux extrémités de la relation.</span><span class="sxs-lookup"><span data-stu-id="292c6-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="292c6-146">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="292c6-146">Press **OK**.</span></span>

<span data-ttu-id="292c6-147">L’étape suivante nécessite la fenêtre **Détails de mappage** .</span><span class="sxs-lookup"><span data-stu-id="292c6-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="292c6-148">Si vous ne voyez pas cette fenêtre, cliquez avec le bouton droit sur l’aire de conception, puis sélectionnez **Détails de mapping**.</span><span class="sxs-lookup"><span data-stu-id="292c6-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="292c6-149">Sélectionnez le type d’entité **HireInfo** , puis cliquez sur **&lt;ajouter une table ou une vue&gt;**  dans la fenêtre des **Détails de mappage** .</span><span class="sxs-lookup"><span data-stu-id="292c6-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="292c6-150">Sélectionnez **Person** dans la liste déroulante **&lt;ajouter une table ou une vue&gt;**  champ.</span><span class="sxs-lookup"><span data-stu-id="292c6-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="292c6-151">La liste contient les tables ou les vues auxquelles l’entité sélectionnée peut être mappée.</span><span class="sxs-lookup"><span data-stu-id="292c6-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="292c6-152">Les propriétés appropriées doivent être mappées par défaut.</span><span class="sxs-lookup"><span data-stu-id="292c6-152">The appropriate properties should be mapped by default.</span></span>

    ![Mappage](~/ef6/media/mapping.png)

-   <span data-ttu-id="292c6-154">Sélectionnez l’Association **PersonHireInfo** sur l’aire de conception.</span><span class="sxs-lookup"><span data-stu-id="292c6-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="292c6-155">Cliquez avec le bouton droit sur l’Association sur l’aire de conception, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="292c6-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="292c6-156">Dans la fenêtre **Propriétés** , sélectionnez la propriété **contraintes référentielles** , puis cliquez sur le bouton de sélection.</span><span class="sxs-lookup"><span data-stu-id="292c6-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="292c6-157">Sélectionnez **Person** dans la liste déroulante **principal** .</span><span class="sxs-lookup"><span data-stu-id="292c6-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="292c6-158">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="292c6-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="292c6-159">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="292c6-159">Use the Model</span></span>

-   <span data-ttu-id="292c6-160">Collez le code suivant dans la méthode main.</span><span class="sxs-lookup"><span data-stu-id="292c6-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="292c6-161">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="292c6-161">Compile and run the application.</span></span>

<span data-ttu-id="292c6-162">Les instructions T-SQL suivantes ont été exécutées sur la base de données **School** suite à l’exécution de cette application.</span><span class="sxs-lookup"><span data-stu-id="292c6-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="292c6-163">L' **instruction INSERT** suivante a été exécutée à la suite de l’exécution du contexte. SaveChanges () et combine les données des entités **Person** et **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="292c6-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insérer](~/ef6/media/insert.png)

-   <span data-ttu-id="292c6-165">La commande **Select** suivante a été exécutée à la suite de l’exécution du contexte. People. FirstOrDefault () et sélectionne uniquement les colonnes mappées à **Person**</span><span class="sxs-lookup"><span data-stu-id="292c6-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Sélectionner 1](~/ef6/media/select1.png)

-   <span data-ttu-id="292c6-167">La commande **Select** suivante a été exécutée suite à l’accès à la propriété de navigation existingPerson. Instructor et sélectionne uniquement les colonnes mappées à **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="292c6-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Sélectionner 2](~/ef6/media/select2.png)
