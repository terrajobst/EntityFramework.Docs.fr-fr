---
title: Concepteur CUD procédures stockées - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489555"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="277b0-102">CUD Concepteur de procédures stockées</span><span class="sxs-lookup"><span data-stu-id="277b0-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="277b0-103">Cette procédure pas à pas montrent comment mapper la créer\\insérer, mettre à jour et supprimer des opérations (CUD) d’un type d’entité aux procédures stockées à l’aide d’Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="277b0-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="277b0-104">Par défaut, Entity Framework génère automatiquement les instructions SQL pour les opérations CUD, mais vous pouvez également mapper des procédures stockées à ces opérations.</span><span class="sxs-lookup"><span data-stu-id="277b0-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="277b0-105">Notez que Code First ne prend pas en charge le mappage de procédures stockées ou fonctions.</span><span class="sxs-lookup"><span data-stu-id="277b0-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="277b0-106">Toutefois, vous pouvez appeler des procédures stockées ou fonctions à l’aide de la méthode System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="277b0-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="277b0-107">Exemple :</span><span class="sxs-lookup"><span data-stu-id="277b0-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="277b0-108">Considérations lors du mappage d’opérations CUD à des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="277b0-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="277b0-109">Lorsque vous mappez les opérations CUD à des procédures stockées, les considérations suivantes s’appliquent :</span><span class="sxs-lookup"><span data-stu-id="277b0-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="277b0-110">Si vous mappez une des opérations CUD à une procédure stockée, mapper l’ensemble d'entre eux.</span><span class="sxs-lookup"><span data-stu-id="277b0-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="277b0-111">Si vous ne mappez pas les trois, les opérations non mappées échouera si exécutée et qu’un **UpdateException** sera levée.</span><span class="sxs-lookup"><span data-stu-id="277b0-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="277b0-112">Vous devez mapper chaque paramètre de la procédure stockée aux propriétés de l’entité.</span><span class="sxs-lookup"><span data-stu-id="277b0-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="277b0-113">Si le serveur génère la valeur de clé primaire pour la ligne insérée, vous devez mapper cette valeur à la propriété de clé de l’entité.</span><span class="sxs-lookup"><span data-stu-id="277b0-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="277b0-114">Dans l’exemple qui suit, la **InsertPerson** procédure stockée retourne la clé primaire nouvellement créée en tant que partie du jeu de résultats de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="277b0-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="277b0-115">La clé primaire est mappée à la clé d’entité (**PersonID**) à l’aide de la **&lt;ajouter un Result Binding&gt;** fonctionnalité du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="277b0-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="277b0-116">Les appels de procédure stockée sont mappés 1:1 avec les entités dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="277b0-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="277b0-117">Par exemple, si vous implémentez une hiérarchie d’héritage dans votre modèle conceptuel et le mappage puis le CUD procédures stockées pour le **Parent** (base) et le **enfant** des entités (dérivées), l’enregistrement de la **Enfant** modifications appelle uniquement la **enfant**de procédures stockées, elle ne déclenche pas le **Parent**des appels de procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="277b0-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="277b0-118">Prérequis</span><span class="sxs-lookup"><span data-stu-id="277b0-118">Prerequisites</span></span>

<span data-ttu-id="277b0-119">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="277b0-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="277b0-120">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="277b0-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="277b0-121">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="277b0-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="277b0-122">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="277b0-122">Set up the Project</span></span>

-   <span data-ttu-id="277b0-123">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="277b0-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="277b0-124">Sélectionnez **fichier -&gt; nouveau -&gt; projet**</span><span class="sxs-lookup"><span data-stu-id="277b0-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="277b0-125">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="277b0-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="277b0-126">Entrez **CUDSProcsSample** comme nom.</span><span class="sxs-lookup"><span data-stu-id="277b0-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="277b0-127">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="277b0-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="277b0-128">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="277b0-128">Create a Model</span></span>

-   <span data-ttu-id="277b0-129">Cliquez sur le nom du projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="277b0-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="277b0-130">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="277b0-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="277b0-131">Entrez **CUDSProcs.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="277b0-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="277b0-132">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="277b0-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="277b0-133">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="277b0-133">Click **New Connection**.</span></span> <span data-ttu-id="277b0-134">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="277b0-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="277b0-135">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="277b0-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="277b0-136">Dans la choisir vos objets de base de données boîte de dialogue, sous le **Tables** nœud, sélectionnez le **personne** table.</span><span class="sxs-lookup"><span data-stu-id="277b0-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="277b0-137">En outre, sélectionnez les procédures stockées suivantes sous la **de procédures stockées et fonctions** nœud : **DeletePerson**, **InsertPerson**, et **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="277b0-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="277b0-138">À partir de Visual Studio 2012, le Concepteur EF prend en charge l’importation en bloc des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="277b0-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="277b0-139">Le **importation sélectionné des procédures stockées et fonctions dans le modèle d’entité** est activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="277b0-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="277b0-140">Étant donné que dans cet exemple nous avons des procédures stockées qui insérant, mettre à jour et supprimer des types d’entité, nous ne souhaitez pas les importer et sera décochez cette case à cocher.</span><span class="sxs-lookup"><span data-stu-id="277b0-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![Importer S Procs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="277b0-142">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="277b0-142">Click **Finish**.</span></span>
    <span data-ttu-id="277b0-143">Le Concepteur EF, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="277b0-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="277b0-144">Mapper l’entité Person à des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="277b0-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="277b0-145">Cliquez sur le **personne** type d’entité, puis sélectionnez **mappage de procédure stockée**.</span><span class="sxs-lookup"><span data-stu-id="277b0-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="277b0-146">Les mappages de la procédure stockée s’affichent dans le **détails de Mapping** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="277b0-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="277b0-147">Cliquez sur  **&lt;Sélectionnez Insérer une fonction&gt;**.</span><span class="sxs-lookup"><span data-stu-id="277b0-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="277b0-148">Le champ devient une liste déroulante des procédures stockées dans le modèle de stockage qui peut être mappé aux types d'entité dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="277b0-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="277b0-149">Sélectionnez **InsertPerson** dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="277b0-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="277b0-150">Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent.</span><span class="sxs-lookup"><span data-stu-id="277b0-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="277b0-151">Notez que les flèches indiquent le sens du mappage : les valeurs des propriétés sont fournies aux paramètres des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="277b0-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="277b0-152">Cliquez sur  **&lt;ajouter un Result Binding&gt;**.</span><span class="sxs-lookup"><span data-stu-id="277b0-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="277b0-153">Type **NewPersonID**, le nom du paramètre retourné par la **InsertPerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="277b0-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="277b0-154">Veillez ne pas à taper les espaces de début ou de fin.</span><span class="sxs-lookup"><span data-stu-id="277b0-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="277b0-155">Appuyez sur **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="277b0-155">Press **Enter**.</span></span>
-   <span data-ttu-id="277b0-156">Par défaut, **NewPersonID** est mappé à la clé d’entité **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="277b0-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="277b0-157">Notez qu'une flèche indique le sens du mappage : la valeur de la colonne de résultats est fournie à la propriété.</span><span class="sxs-lookup"><span data-stu-id="277b0-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Détails de mappage](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="277b0-159">Cliquez sur **&lt;sélectionner un Update Function&gt;** et sélectionnez **UpdatePerson** dans la liste déroulante résultante.</span><span class="sxs-lookup"><span data-stu-id="277b0-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="277b0-160">Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent.</span><span class="sxs-lookup"><span data-stu-id="277b0-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="277b0-161">Cliquez sur **&lt;sélectionner un Delete Function&gt;** et sélectionnez **DeletePerson** dans la liste déroulante résultante.</span><span class="sxs-lookup"><span data-stu-id="277b0-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="277b0-162">Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent.</span><span class="sxs-lookup"><span data-stu-id="277b0-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="277b0-163">L’instruction insert, update et delete d’opérations de le **personne** type d’entité sont maintenant mappées à des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="277b0-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="277b0-164">Si vous souhaitez activer l’accès concurrentiel lors de la mise à jour ou suppression d’une entité avec des procédures stockées, utilisez une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="277b0-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="277b0-165">Utilisez un **sortie** paramètre pour retourner le nombre d’affecté des lignes à partir de la procédure stockée et la vérification de la **paramètre des lignes affectées** case à cocher en regard du nom de paramètre.</span><span class="sxs-lookup"><span data-stu-id="277b0-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="277b0-166">Si la valeur retournée est zéro lorsque l’opération est appelée, un [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) sera levée.</span><span class="sxs-lookup"><span data-stu-id="277b0-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="277b0-167">Vérifier le **utiliser Original Value** case à cocher en regard d’une propriété que vous souhaitez utiliser pour le contrôle d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="277b0-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="277b0-168">Lorsqu’une mise à jour est tentée, la valeur de la propriété qui a été initialement lue à partir de la base de données sera utilisée lors de l’écriture des données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="277b0-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="277b0-169">Si la valeur ne correspond pas à la valeur dans la base de données, un **OptimisticConcurrencyException** sera levée.</span><span class="sxs-lookup"><span data-stu-id="277b0-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="277b0-170">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="277b0-170">Use the Model</span></span>

<span data-ttu-id="277b0-171">Ouvrez le **Program.cs** fichier où le **Main** méthode est définie.</span><span class="sxs-lookup"><span data-stu-id="277b0-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="277b0-172">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="277b0-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="277b0-173">Le code crée un nouveau **personne** objet, puis met à jour l’objet et enfin supprime l’objet.</span><span class="sxs-lookup"><span data-stu-id="277b0-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="277b0-174">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="277b0-174">Compile and run the application.</span></span> <span data-ttu-id="277b0-175">Le programme génère la sortie suivante \*</span><span class="sxs-lookup"><span data-stu-id="277b0-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="277b0-176">PersonID étant généré automatiquement par le serveur, vous verrez très probablement un autre nombre \*</span><span class="sxs-lookup"><span data-stu-id="277b0-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="277b0-177">Si vous travaillez avec la version finale de Visual Studio, vous pouvez utiliser Intellitrace avec le débogueur pour voir les instructions SQL qui sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="277b0-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
