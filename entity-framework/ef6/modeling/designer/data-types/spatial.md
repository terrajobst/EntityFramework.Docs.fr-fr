---
title: Spatial-EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418538"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="226ad-102">Concepteur spatial-EF</span><span class="sxs-lookup"><span data-stu-id="226ad-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="226ad-103">**EF5 uniquement** : les fonctionnalités, les API, etc. présentées dans cette page ont été introduites dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="226ad-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="226ad-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="226ad-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="226ad-105">La vidéo et la procédure pas à pas montrent comment mapper des types spatiaux avec l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="226ad-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="226ad-106">Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="226ad-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="226ad-107">Cette procédure pas à pas utilise Model First pour créer une base de données, mais le concepteur EF peut également être utilisé avec le flux de travail [Database First](~/ef6/modeling/designer/workflows/database-first.md) pour mapper à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="226ad-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="226ad-108">La prise en charge des types spatiaux a été introduite dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="226ad-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="226ad-109">Notez que pour utiliser les nouvelles fonctionnalités telles que le type spatial, les énumérations et les fonctions table, vous devez cibler .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="226ad-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="226ad-110">Visual Studio 2012 cible .NET 4,5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="226ad-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="226ad-111">Pour utiliser des types de données spatiales, vous devez également utiliser un fournisseur de Entity Framework qui a une prise en charge spatiale.</span><span class="sxs-lookup"><span data-stu-id="226ad-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="226ad-112">Pour plus d’informations, consultez [prise en charge des fournisseurs pour les types spatiaux](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="226ad-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="226ad-113">Il existe deux types de données spatiales principales : Geography et Geometry.</span><span class="sxs-lookup"><span data-stu-id="226ad-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="226ad-114">Le type de données geography stocke des données ellipsoïdal (par exemple, des coordonnées de latitude et de longitude GPS).</span><span class="sxs-lookup"><span data-stu-id="226ad-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="226ad-115">Le type de données geometry représente le système de coordonnées euclidienne (Flat).</span><span class="sxs-lookup"><span data-stu-id="226ad-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="226ad-116">Regarder la vidéo</span><span class="sxs-lookup"><span data-stu-id="226ad-116">Watch the video</span></span>
<span data-ttu-id="226ad-117">Cette vidéo montre comment mapper des types spatiaux avec l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="226ad-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="226ad-118">Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="226ad-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="226ad-119">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="226ad-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="226ad-120">**Vidéo**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (zip)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="226ad-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="226ad-121">Prérequis</span><span class="sxs-lookup"><span data-stu-id="226ad-121">Pre-Requisites</span></span>

<span data-ttu-id="226ad-122">Pour effectuer cette procédure pas à pas, vous devez avoir installé Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="226ad-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="226ad-123">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="226ad-123">Set up the Project</span></span>

1.  <span data-ttu-id="226ad-124">Ouvrir Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="226ad-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="226ad-125">Dans le menu **fichier** , pointez sur **nouveau**, puis cliquez sur **projet** .</span><span class="sxs-lookup"><span data-stu-id="226ad-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="226ad-126">Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **console** .</span><span class="sxs-lookup"><span data-stu-id="226ad-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="226ad-127">Entrez **SpatialEFDesigner** comme nom du projet, puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="226ad-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="226ad-128">Créer un nouveau modèle à l’aide du concepteur EF</span><span class="sxs-lookup"><span data-stu-id="226ad-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="226ad-129">Cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions, pointez sur **Ajouter**, puis cliquez sur **nouvel élément** .</span><span class="sxs-lookup"><span data-stu-id="226ad-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="226ad-130">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet modèles.</span><span class="sxs-lookup"><span data-stu-id="226ad-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="226ad-131">Entrez **UniversityModel. edmx** comme nom de fichier, puis cliquez sur **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="226ad-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="226ad-132">Dans la page Entity Data Model de l’Assistant, sélectionnez **modèle vide** dans la boîte de dialogue choisir le contenu du modèle.</span><span class="sxs-lookup"><span data-stu-id="226ad-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="226ad-133">Cliquez sur **Terminer**</span><span class="sxs-lookup"><span data-stu-id="226ad-133">Click **Finish**</span></span>

<span data-ttu-id="226ad-134">Le Entity Designer, qui fournit une aire de conception pour la modification de votre modèle, est affiché.</span><span class="sxs-lookup"><span data-stu-id="226ad-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="226ad-135">L'assistant exécute les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="226ad-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="226ad-136">Génère le fichier EnumTestModel. edmx qui définit le modèle conceptuel, le modèle de stockage et le mappage entre eux.</span><span class="sxs-lookup"><span data-stu-id="226ad-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="226ad-137">Définit la propriété de traitement des artefacts de métadonnées du fichier. edmx à incorporer dans l’assembly de sortie afin que les fichiers de métadonnées générés soient incorporés dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="226ad-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="226ad-138">Ajoute une référence aux assemblys suivants : EntityFramework, System. ComponentModel. DataAnnotations et System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="226ad-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="226ad-139">Crée des fichiers UniversityModel.tt et UniversityModel.Context.tt et les ajoute sous le fichier. edmx.</span><span class="sxs-lookup"><span data-stu-id="226ad-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="226ad-140">Ces fichiers de modèle T4 génèrent le code qui définit le type dérivé DbContext et les types POCO qui mappent aux entités dans le modèle. edmx</span><span class="sxs-lookup"><span data-stu-id="226ad-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="226ad-141">Ajouter un nouveau type d’entité</span><span class="sxs-lookup"><span data-stu-id="226ad-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="226ad-142">Cliquez avec le bouton droit sur une zone vide de l’aire de conception, sélectionnez **Ajouter-&gt; entité**, la boîte de dialogue nouvelle entité s’affiche.</span><span class="sxs-lookup"><span data-stu-id="226ad-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="226ad-143">Spécifiez **University** comme nom de type et spécifiez **UniversityID** pour le nom de la propriété de clé, laissez le type **Int32**</span><span class="sxs-lookup"><span data-stu-id="226ad-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="226ad-144">Cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="226ad-144">Click **OK**</span></span>
4.  <span data-ttu-id="226ad-145">Cliquez avec le bouton droit sur l’entité, puis sélectionnez **Ajouter une nouvelle&gt; propriété scalaire**</span><span class="sxs-lookup"><span data-stu-id="226ad-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="226ad-146">Renommez la nouvelle propriété en **Name**</span><span class="sxs-lookup"><span data-stu-id="226ad-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="226ad-147">Ajoutez une autre propriété scalaire et renommez-la **emplacement** ouvrir le fenêtre Propriétés et remplacez le type de la nouvelle propriété par **Geography**</span><span class="sxs-lookup"><span data-stu-id="226ad-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="226ad-148">Enregistrer le modèle et générer le projet</span><span class="sxs-lookup"><span data-stu-id="226ad-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="226ad-149">Lorsque vous générez, les avertissements relatifs aux entités et associations non mappés peuvent apparaître dans le Liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="226ad-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="226ad-150">Vous pouvez ignorer ces avertissements, car une fois que vous avez choisi de générer la base de données à partir du modèle, les erreurs disparaissent.</span><span class="sxs-lookup"><span data-stu-id="226ad-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="226ad-151">Générer la base de données à partir du modèle</span><span class="sxs-lookup"><span data-stu-id="226ad-151">Generate Database from Model</span></span>

<span data-ttu-id="226ad-152">Nous pouvons maintenant générer une base de données basée sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="226ad-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="226ad-153">Cliquez avec le bouton droit sur un espace vide sur l’aire de Entity Designer et sélectionnez **générer la base de données à partir du modèle** .</span><span class="sxs-lookup"><span data-stu-id="226ad-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="226ad-154">La boîte de dialogue choisir votre connexion de données de l’Assistant Création d’une base de données s’affiche. cliquez sur le bouton **nouvelle connexion** , spécifiez (base de données locale **)\\mssqllocaldb** pour le nom du serveur et l' **Université** pour la base de données, puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="226ad-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="226ad-155">Une boîte de dialogue vous demandant si vous souhaitez créer une nouvelle base de données s’affiche, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="226ad-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="226ad-156">Cliquez sur **suivant** pour que l’Assistant Création d’une base de données génère le langage de définition de données (DDL) pour la création d’une base de données. la DDL générée est affichée dans la boîte de dialogue Résumé et paramètres. Notez que le DDL ne contient pas de définition pour une table qui correspond au type d’énumération.</span><span class="sxs-lookup"><span data-stu-id="226ad-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="226ad-157">Cliquez sur **Terminer** pour ne pas exécuter le script DDL.</span><span class="sxs-lookup"><span data-stu-id="226ad-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="226ad-158">L’Assistant Création d’une base de données effectue les opérations suivantes : ouvre **UniversityModel. edmx. SQL** dans l’éditeur T-SQL génère les sections de schéma de stockage et de mappage du fichier edmx ajoute les informations de chaîne de connexion au fichier app. config.</span><span class="sxs-lookup"><span data-stu-id="226ad-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="226ad-159">Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL et sélectionnez **exécuter** la boîte de dialogue se connecter au serveur, entrez les informations de connexion de l’étape 2, puis cliquez sur **se connecter** .</span><span class="sxs-lookup"><span data-stu-id="226ad-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="226ad-160">Pour afficher le schéma généré, cliquez avec le bouton droit sur le nom de la base de données dans Explorateur d’objets SQL Server puis sélectionnez **Actualiser** .</span><span class="sxs-lookup"><span data-stu-id="226ad-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="226ad-161">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="226ad-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="226ad-162">Ouvrez le fichier Program.cs dans lequel la méthode main est définie.</span><span class="sxs-lookup"><span data-stu-id="226ad-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="226ad-163">Ajoutez le code suivant à la fonction main.</span><span class="sxs-lookup"><span data-stu-id="226ad-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="226ad-164">Le code ajoute deux nouveaux objets University au contexte.</span><span class="sxs-lookup"><span data-stu-id="226ad-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="226ad-165">Les propriétés spatiales sont initialisées à l’aide de la méthode DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="226ad-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="226ad-166">Le point géographique représenté en tant que WellKnownText est passé à la méthode.</span><span class="sxs-lookup"><span data-stu-id="226ad-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="226ad-167">Le code enregistre ensuite les données.</span><span class="sxs-lookup"><span data-stu-id="226ad-167">The code then saves the data.</span></span> <span data-ttu-id="226ad-168">Ensuite, la requête LINQ qui retourne un objet University dans lequel son emplacement est le plus proche de l’emplacement spécifié est construite et exécutée.</span><span class="sxs-lookup"><span data-stu-id="226ad-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="226ad-169">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="226ad-169">Compile and run the application.</span></span> <span data-ttu-id="226ad-170">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="226ad-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="226ad-171">Pour afficher les données de la base de données, cliquez avec le bouton droit sur le nom de la base de données dans Explorateur d’objets SQL Server, puis sélectionnez **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="226ad-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="226ad-172">Ensuite, cliquez sur le bouton droit de la souris sur la table et sélectionnez **afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="226ad-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="226ad-173">Résumé</span><span class="sxs-lookup"><span data-stu-id="226ad-173">Summary</span></span>

<span data-ttu-id="226ad-174">Dans cette procédure pas à pas, nous avons vu comment mapper des types spatiaux à l’aide de la Entity Framework Designer et comment utiliser des types spatiaux dans le code.</span><span class="sxs-lookup"><span data-stu-id="226ad-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
