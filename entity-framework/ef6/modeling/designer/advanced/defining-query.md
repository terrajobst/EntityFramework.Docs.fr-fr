---
title: Définition de requête - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 8415a265cdbe078422e0467ee97da955a81b873d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250970"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="9a72c-102">Définition de requête - Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="9a72c-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="9a72c-103">Cette procédure pas à pas montre comment ajouter une définition de type de requête et une entité correspondante à un modèle à l’aide du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="9a72c-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="9a72c-104">Une requête de définition est couramment utilisée pour fournir des fonctionnalités semblables à celles fournies par une vue de base de données, mais la vue est définie dans le modèle, pas la base de données.</span><span class="sxs-lookup"><span data-stu-id="9a72c-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="9a72c-105">Une requête de définition vous permet d’exécuter une instruction SQL qui est spécifiée dans le **DefiningQuery** élément d’un fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="9a72c-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="9a72c-106">Pour plus d’informations, consultez **DefiningQuery** dans le [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="9a72c-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="9a72c-107">Lorsque vous utilisez des requêtes de définition, vous devez également définir un type d’entité dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="9a72c-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="9a72c-108">Le type d’entité est utilisé pour exposer les données exposées par la requête de définition.</span><span class="sxs-lookup"><span data-stu-id="9a72c-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="9a72c-109">Notez que les données révélées par ce type d’entité sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="9a72c-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="9a72c-110">Les requêtes paramétrées ne peuvent pas être exécutées en tant que requêtes de définition.</span><span class="sxs-lookup"><span data-stu-id="9a72c-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="9a72c-111">Toutefois, les données peuvent être mises à jour en mappant les fonctions d'insertion, de mise à jour et de suppression du type d'entité qui surface les données aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="9a72c-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="9a72c-112">Pour plus d’informations, consultez [Insert, Update et Delete avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="9a72c-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="9a72c-113">Cette rubrique montre comment effectuer les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="9a72c-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="9a72c-114">Ajouter une requête de définition</span><span class="sxs-lookup"><span data-stu-id="9a72c-114">Add a Defining Query</span></span>
-   <span data-ttu-id="9a72c-115">Ajouter un Type d’entité au modèle</span><span class="sxs-lookup"><span data-stu-id="9a72c-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="9a72c-116">Mapper la requête de définition pour le Type d’entité</span><span class="sxs-lookup"><span data-stu-id="9a72c-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a72c-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9a72c-117">Prerequisites</span></span>

<span data-ttu-id="9a72c-118">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9a72c-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="9a72c-119">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a72c-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="9a72c-120">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9a72c-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9a72c-121">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="9a72c-121">Set up the Project</span></span>

<span data-ttu-id="9a72c-122">Cette procédure pas à pas utilise Visual Studio 2012 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9a72c-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="9a72c-123">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a72c-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="9a72c-124">Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="9a72c-125">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Application Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="9a72c-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="9a72c-126">Entrez **DefiningQuerySample** en tant que le nom du projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="9a72c-127">Créer un modèle basé sur la base de données School</span><span class="sxs-lookup"><span data-stu-id="9a72c-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="9a72c-128">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="9a72c-129">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="9a72c-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="9a72c-130">Entrez **DefiningQueryModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="9a72c-131">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="9a72c-132">Cliquez sur Nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="9a72c-132">Click New Connection.</span></span> <span data-ttu-id="9a72c-133">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="9a72c-134">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="9a72c-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="9a72c-135">Dans la boîte de dialogue Choisir vos objets de base de données, vérifiez le **Tables** nœud.</span><span class="sxs-lookup"><span data-stu-id="9a72c-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="9a72c-136">Cette opération ajoute toutes les tables à la **School** modèle.</span><span class="sxs-lookup"><span data-stu-id="9a72c-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="9a72c-137">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-137">Click **Finish**.</span></span>
-   <span data-ttu-id="9a72c-138">Dans l’Explorateur de solutions, cliquez sur le **DefiningQueryModel.edmx** fichier et sélectionnez **ouvrir avec...** .</span><span class="sxs-lookup"><span data-stu-id="9a72c-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="9a72c-139">Sélectionnez **éditeur XML (texte)**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-139">Select **XML (Text) Editor**.</span></span>

    ![Éditeur XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="9a72c-141">Cliquez sur **Oui** si vous y êtes invité avec le message suivant :</span><span class="sxs-lookup"><span data-stu-id="9a72c-141">Click **Yes** if prompted with the following message:</span></span>

    ![Avertissement 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="9a72c-143">Ajouter une requête de définition</span><span class="sxs-lookup"><span data-stu-id="9a72c-143">Add a Defining Query</span></span>

<span data-ttu-id="9a72c-144">Dans cette étape, nous allons utiliser l’éditeur XML pour ajouter une définition de requête et un type d’entité à la section SSDL du fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="9a72c-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="9a72c-145">Ajouter un **EntitySet** élément à la section SSDL du fichier .edmx (ligne 5 à 13).</span><span class="sxs-lookup"><span data-stu-id="9a72c-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="9a72c-146">Spécifier les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9a72c-146">Specify the following:</span></span>
    -   <span data-ttu-id="9a72c-147">Uniquement les **nom** et **EntityType** les attributs de la **EntitySet** élément sont spécifiées.</span><span class="sxs-lookup"><span data-stu-id="9a72c-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="9a72c-148">Le nom qualifié complet du type d’entité est utilisé dans le **EntityType** attribut.</span><span class="sxs-lookup"><span data-stu-id="9a72c-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="9a72c-149">L’instruction SQL à exécuter est spécifiée dans le **DefiningQuery** élément.</span><span class="sxs-lookup"><span data-stu-id="9a72c-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="9a72c-150">Ajouter le **EntityType** élément à la section SSDL du fichier.</span><span class="sxs-lookup"><span data-stu-id="9a72c-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="9a72c-151">fichier comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="9a72c-151">file as shown below.</span></span> <span data-ttu-id="9a72c-152">Notez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9a72c-152">Note the following:</span></span>
    -   <span data-ttu-id="9a72c-153">La valeur de la **nom** attribut correspond à la valeur de la **EntityType** d’attribut dans le **EntitySet** élément ci-dessus, bien que le nom qualifié complet de la type d’entité est utilisé dans le **EntityType** attribut.</span><span class="sxs-lookup"><span data-stu-id="9a72c-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="9a72c-154">Les noms des propriétés correspondent aux noms de colonnes retournés par l’instruction SQL dans le **DefiningQuery** élément (ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="9a72c-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="9a72c-155">Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.</span><span class="sxs-lookup"><span data-stu-id="9a72c-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="9a72c-156">Si plus tard vous exécutez le **Assistant modèle de mise à jour** boîte de dialogue, toutes les modifications apportées au modèle de stockage, y compris les requêtes de définition, sera remplacé.</span><span class="sxs-lookup"><span data-stu-id="9a72c-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="9a72c-157">Ajouter un Type d’entité au modèle</span><span class="sxs-lookup"><span data-stu-id="9a72c-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="9a72c-158">Dans cette étape, nous allons ajouter le type d’entité au modèle conceptuel à l’aide du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="9a72c-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="9a72c-159">Notez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9a72c-159">Note the following:</span></span>

-   <span data-ttu-id="9a72c-160">Le **nom** de l’entité correspond à la valeur de la **EntityType** d’attribut dans le **EntitySet** élément ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9a72c-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="9a72c-161">Les noms des propriétés correspondent aux noms de colonnes retournés par l’instruction SQL dans le **DefiningQuery** élément ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9a72c-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="9a72c-162">Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.</span><span class="sxs-lookup"><span data-stu-id="9a72c-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="9a72c-163">Ouvrez le modèle dans le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="9a72c-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="9a72c-164">Double-cliquez sur le DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="9a72c-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="9a72c-165">Par exemple **Oui** au message suivant :</span><span class="sxs-lookup"><span data-stu-id="9a72c-165">Say **Yes** to the following message:</span></span>

    ![Avertissement 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="9a72c-167">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="9a72c-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="9a72c-168">Cliquez sur l’aire du concepteur puis sélectionnez **Ajouter nouveau**-&gt;**entité...** .</span><span class="sxs-lookup"><span data-stu-id="9a72c-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="9a72c-169">Spécifiez **GradeReport** pour le nom de l’entité et **CourseID** pour le **propriété Key**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="9a72c-170">Avec le bouton droit le **GradeReport** entité, puis sélectionnez **Ajouter nouveau** - &gt; **propriété scalaire**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="9a72c-171">Modifier le nom par défaut de la propriété à **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="9a72c-172">Ajoutez une autre propriété scalaire et spécifiez **LastName** pour le nom.</span><span class="sxs-lookup"><span data-stu-id="9a72c-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="9a72c-173">Ajoutez une autre propriété scalaire et spécifiez **Grade** pour le nom.</span><span class="sxs-lookup"><span data-stu-id="9a72c-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="9a72c-174">Dans le **propriétés** fenêtre, modifier le **Grade**de **Type** propriété **décimal**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="9a72c-175">Sélectionnez le **FirstName** et **LastName** propriétés.</span><span class="sxs-lookup"><span data-stu-id="9a72c-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="9a72c-176">Dans le **propriétés** fenêtre, de modifier le **EntityKey** valeur de propriété à **True**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="9a72c-177">Par conséquent, les éléments suivants ont été ajoutés à la **CSDL** section du fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="9a72c-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="9a72c-178">Mapper la requête de définition pour le Type d’entité</span><span class="sxs-lookup"><span data-stu-id="9a72c-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="9a72c-179">Dans cette étape, nous allons utiliser la fenêtre Détails de mappage pour mapper les concepts et les types d’entités de stockage.</span><span class="sxs-lookup"><span data-stu-id="9a72c-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="9a72c-180">Cliquez sur le **GradeReport** entité sur l’aire de conception et sélectionnez **mappage de Table**.</span><span class="sxs-lookup"><span data-stu-id="9a72c-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="9a72c-181">Le **détails de Mapping** fenêtre s’affiche.</span><span class="sxs-lookup"><span data-stu-id="9a72c-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="9a72c-182">Sélectionnez **GradeReport** à partir de la **&lt;ajouter une Table ou vue&gt;** liste déroulante (situé sous **Table**s).</span><span class="sxs-lookup"><span data-stu-id="9a72c-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="9a72c-183">Par défaut des mappages entre les concepts et de stockage **GradeReport** type d’entité s’affichent.</span><span class="sxs-lookup"><span data-stu-id="9a72c-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="9a72c-184">![Mappage 3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="9a72c-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="9a72c-185">Par conséquent, le **EntitySetMapping** élément est ajouté à la section mapping du fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="9a72c-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="9a72c-186">Compilez l'application.</span><span class="sxs-lookup"><span data-stu-id="9a72c-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="9a72c-187">Appeler la requête de définition dans votre Code</span><span class="sxs-lookup"><span data-stu-id="9a72c-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="9a72c-188">Vous pouvez maintenant exécuter la requête de définition à l’aide de la **GradeReport** type d’entité.</span><span class="sxs-lookup"><span data-stu-id="9a72c-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
