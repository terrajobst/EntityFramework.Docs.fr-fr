---
title: Définition de Query-EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418797"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="ec32e-102">Définition de la requête-concepteur EF</span><span class="sxs-lookup"><span data-stu-id="ec32e-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="ec32e-103">Cette procédure pas à pas montre comment ajouter une requête de définition et un type d’entité correspondant à un modèle à l’aide du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="ec32e-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="ec32e-104">Une requête de définition est couramment utilisée pour fournir des fonctionnalités semblables à celles fournies par une vue de base de données, mais la vue est définie dans le modèle, et non dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="ec32e-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="ec32e-105">Une requête de définition vous permet d’exécuter une instruction SQL qui est spécifiée dans l’élément **DefiningQuery** d’un fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="ec32e-106">Pour plus d’informations, consultez **DefiningQuery** dans la [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="ec32e-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="ec32e-107">Lorsque vous utilisez la définition de requêtes, vous devez également définir un type d’entité dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="ec32e-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="ec32e-108">Le type d’entité est utilisé pour exposer les données exposées par la requête de définition.</span><span class="sxs-lookup"><span data-stu-id="ec32e-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="ec32e-109">Notez que les données exposées par le biais de ce type d’entité sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="ec32e-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="ec32e-110">Les requêtes paramétrées ne peuvent pas être exécutées en tant que requêtes de définition.</span><span class="sxs-lookup"><span data-stu-id="ec32e-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="ec32e-111">Toutefois, les données peuvent être mises à jour en mappant les fonctions d'insertion, de mise à jour et de suppression du type d'entité qui surface les données aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="ec32e-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="ec32e-112">Pour plus d’informations, consultez [Insert, Update et Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="ec32e-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="ec32e-113">Cette rubrique montre comment effectuer les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="ec32e-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="ec32e-114">Ajouter une requête de définition</span><span class="sxs-lookup"><span data-stu-id="ec32e-114">Add a Defining Query</span></span>
-   <span data-ttu-id="ec32e-115">Ajouter un type d’entité au modèle</span><span class="sxs-lookup"><span data-stu-id="ec32e-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="ec32e-116">Mapper la requête de définition au type d’entité</span><span class="sxs-lookup"><span data-stu-id="ec32e-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec32e-117">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="ec32e-117">Prerequisites</span></span>

<span data-ttu-id="ec32e-118">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ec32e-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ec32e-119">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec32e-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ec32e-120">[Exemple de base de données School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ec32e-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ec32e-121">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="ec32e-121">Set up the Project</span></span>

<span data-ttu-id="ec32e-122">Cette procédure pas à pas utilise Visual Studio 2012 ou une version plus récente.</span><span class="sxs-lookup"><span data-stu-id="ec32e-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="ec32e-123">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec32e-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="ec32e-124">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="ec32e-125">Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **application console** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="ec32e-126">Entrez **DefiningQuerySample** comme nom du projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="ec32e-127">Créer un modèle basé sur la base de données School</span><span class="sxs-lookup"><span data-stu-id="ec32e-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="ec32e-128">Cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="ec32e-129">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.</span><span class="sxs-lookup"><span data-stu-id="ec32e-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ec32e-130">Entrez **DefiningQueryModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ec32e-131">Dans la boîte de dialogue choisir le contenu du Model, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ec32e-132">Cliquez sur nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="ec32e-132">Click New Connection.</span></span> <span data-ttu-id="ec32e-133">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, (base de données locale **)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ec32e-134">La boîte de dialogue choisir votre connexion de données est mise à jour avec votre paramètre de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="ec32e-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ec32e-135">Dans la boîte de dialogue choisir vos objets de base de données, vérifiez le nœud **Tables** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="ec32e-136">Cette opération ajoute toutes les tables au modèle **School** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="ec32e-137">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-137">Click **Finish**.</span></span>
-   <span data-ttu-id="ec32e-138">Dans Explorateur de solutions, cliquez avec le bouton droit sur le fichier **DefiningQueryModel. edmx** , puis sélectionnez **Ouvrir avec...** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="ec32e-139">Sélectionnez **éditeur XML (texte)** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-139">Select **XML (Text) Editor**.</span></span>

    ![Éditeur XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="ec32e-141">Cliquez sur **Oui** si le message suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="ec32e-141">Click **Yes** if prompted with the following message:</span></span>

    ![AVERTISSEMENT 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="ec32e-143">Ajouter une requête de définition</span><span class="sxs-lookup"><span data-stu-id="ec32e-143">Add a Defining Query</span></span>

<span data-ttu-id="ec32e-144">Dans cette étape, nous allons utiliser l’éditeur XML pour ajouter une requête de définition et un type d’entité à la section SSDL du fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="ec32e-145">Ajoutez un élément **EntitySet** à la section SSDL du fichier. edmx (ligne 5 à 13).</span><span class="sxs-lookup"><span data-stu-id="ec32e-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="ec32e-146">Spécifiez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ec32e-146">Specify the following:</span></span>
    -   <span data-ttu-id="ec32e-147">Seuls les attributs **Name** et **EntityType** de l’élément **EntitySet** sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ec32e-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="ec32e-148">Le nom qualifié complet du type d’entité est utilisé dans l’attribut **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="ec32e-149">L’instruction SQL à exécuter est spécifiée dans l’élément **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="ec32e-150">Ajoutez l’élément **EntityType** à la section SSDL du fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="ec32e-151">fichier comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ec32e-151">file as shown below.</span></span> <span data-ttu-id="ec32e-152">Notez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="ec32e-152">Note the following:</span></span>
    -   <span data-ttu-id="ec32e-153">La valeur de l’attribut **Name** correspond à la valeur de l’attribut **EntityType** dans l’élément **EntitySet** ci-dessus, même si le nom complet du type d’entité est utilisé dans l’attribut **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="ec32e-154">Les noms de propriété correspondent aux noms de colonnes retournés par l’instruction SQL dans l’élément **DefiningQuery** (ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="ec32e-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="ec32e-155">Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.</span><span class="sxs-lookup"><span data-stu-id="ec32e-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="ec32e-156">Si vous exécutez ultérieurement la boîte de dialogue de l' **Assistant Mise à jour du modèle** , toutes les modifications apportées au modèle de stockage, y compris la définition des requêtes, seront remplacées.</span><span class="sxs-lookup"><span data-stu-id="ec32e-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="ec32e-157">Ajouter un type d’entité au modèle</span><span class="sxs-lookup"><span data-stu-id="ec32e-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="ec32e-158">Dans cette étape, nous allons ajouter le type d’entité au modèle conceptuel à l’aide du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="ec32e-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="ec32e-159"> Notez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="ec32e-159"> Note the following:</span></span>

-   <span data-ttu-id="ec32e-160">Le **nom** de l’entité correspond à la valeur de l’attribut **EntityType** dans l’élément **EntitySet** ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ec32e-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="ec32e-161">Les noms de propriété correspondent aux noms de colonnes retournés par l’instruction SQL dans l’élément **DefiningQuery** ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ec32e-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="ec32e-162">Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.</span><span class="sxs-lookup"><span data-stu-id="ec32e-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="ec32e-163">Ouvrez le modèle dans le concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="ec32e-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="ec32e-164">Double-cliquez sur DefiningQueryModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="ec32e-165">Disons **Oui** au message suivant :</span><span class="sxs-lookup"><span data-stu-id="ec32e-165">Say **Yes** to the following message:</span></span>

    ![AVERTISSEMENT 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="ec32e-167">Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché.</span><span class="sxs-lookup"><span data-stu-id="ec32e-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="ec32e-168">Cliquez avec le bouton droit sur l’aire du concepteur, puis sélectionnez **Ajouter une nouvelle**-&gt;**entité...** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="ec32e-169">Spécifiez **GradeReport** pour le nom d’entité et le **CourseID** pour la **propriété de clé**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="ec32e-170">Cliquez avec le bouton droit sur l’entité **GradeReport** et sélectionnez **ajouter une nouvelle**-&gt; **propriété scalaire**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="ec32e-171">Remplacez le nom par défaut de la propriété par **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="ec32e-172">Ajoutez une autre propriété scalaire et spécifiez **LastName** comme nom.</span><span class="sxs-lookup"><span data-stu-id="ec32e-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="ec32e-173">Ajoutez une autre propriété scalaire et spécifiez le nom de la **classe** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="ec32e-174">Dans la fenêtre **Propriétés** , affectez à la propriété **type** de la **classe**la valeur **décimale**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="ec32e-175">Sélectionnez les propriétés **FirstName** et **LastName** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="ec32e-176">Dans la fenêtre **Propriétés** , remplacez la valeur de la propriété **EntityKey** par **true**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="ec32e-177">Par conséquent, les éléments suivants ont été ajoutés à la section **CSDL** du fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="ec32e-178">Mapper la requête de définition au type d’entité</span><span class="sxs-lookup"><span data-stu-id="ec32e-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="ec32e-179">Dans cette étape, nous allons utiliser la fenêtre Détails de mappage pour mapper les types d’entité conceptuels et de stockage.</span><span class="sxs-lookup"><span data-stu-id="ec32e-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="ec32e-180">Cliquez avec le bouton droit sur l’entité **GradeReport** sur l’aire de conception, puis sélectionnez **mappage de table**.</span><span class="sxs-lookup"><span data-stu-id="ec32e-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="ec32e-181">La fenêtre **Détails de mappage** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ec32e-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="ec32e-182">Sélectionnez **GradeReport** dans la liste déroulante **&lt;ajouter une table ou une vue&gt;** (située sous la **table**s).</span><span class="sxs-lookup"><span data-stu-id="ec32e-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="ec32e-183">Les mappages par défaut entre le type d’entité conceptuel et stockage **GradeReport** s’affichent.</span><span class="sxs-lookup"><span data-stu-id="ec32e-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="ec32e-184">![](~/ef6/media/mappingdetails.png) Details3 de mappage</span><span class="sxs-lookup"><span data-stu-id="ec32e-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="ec32e-185">Par conséquent, l’élément **EntitySetMapping** est ajouté à la section Mapping du fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="ec32e-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="ec32e-186">Compilez l’application.</span><span class="sxs-lookup"><span data-stu-id="ec32e-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="ec32e-187">Appeler la requête de définition dans votre code</span><span class="sxs-lookup"><span data-stu-id="ec32e-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="ec32e-188">Vous pouvez maintenant exécuter la requête de définition à l’aide du type d’entité **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="ec32e-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
