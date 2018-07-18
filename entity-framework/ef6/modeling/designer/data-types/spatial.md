---
title: Spatial - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121325"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="e41a7-102">Spatial - Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="e41a7-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="e41a7-103">**EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e41a7-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="e41a7-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="e41a7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e41a7-105">La procédure pas à pas vidéo et pas à pas montre comment mapper des types de données spatiales avec Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="e41a7-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="e41a7-106">Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="e41a7-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="e41a7-107">Cette procédure pas à pas utilise Model First pour créer une nouvelle base de données, mais le Concepteur EF peut également servir avec la [Database First](~/ef6/modeling/designer/workflows/database-first.md) flux de travail pour mapper à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="e41a7-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="e41a7-108">Prise en charge de type spatial a été introduite dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e41a7-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="e41a7-109">Notez que pour utiliser les nouvelles fonctionnalités telles que le type spatial, les enums et les fonctions table, vous devez cibler .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="e41a7-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="e41a7-110">Visual Studio 2012 cible .NET 4.5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="e41a7-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="e41a7-111">Pour utiliser les types de données spatiales, vous devez également utiliser un fournisseur Entity Framework qui prend en charge spatiale.</span><span class="sxs-lookup"><span data-stu-id="e41a7-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="e41a7-112">Consultez [prise en charge de fournisseur pour les types spatiaux](~/ef6/fundamentals/providers/spatial-support.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e41a7-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="e41a7-113">Il existe deux types de données spatiales principale : geography et geometry.</span><span class="sxs-lookup"><span data-stu-id="e41a7-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="e41a7-114">Le type de données geography stocke des données ellipsoïdes (par exemple, GPS de latitude et longitude coordonnées).</span><span class="sxs-lookup"><span data-stu-id="e41a7-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="e41a7-115">Le type de données geometry représente un système de coordonnées euclidien (plat).</span><span class="sxs-lookup"><span data-stu-id="e41a7-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e41a7-116">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="e41a7-116">Watch the video</span></span>
<span data-ttu-id="e41a7-117">Cette vidéo montre comment mapper des types de données spatiales avec Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="e41a7-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="e41a7-118">Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="e41a7-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="e41a7-119">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="e41a7-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="e41a7-120">**Vidéo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="e41a7-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e41a7-121">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="e41a7-121">Pre-Requisites</span></span>

<span data-ttu-id="e41a7-122">Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="e41a7-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e41a7-123">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="e41a7-123">Set up the Project</span></span>

1.  <span data-ttu-id="e41a7-124">Ouvrez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e41a7-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="e41a7-125">Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**</span><span class="sxs-lookup"><span data-stu-id="e41a7-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="e41a7-126">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle</span><span class="sxs-lookup"><span data-stu-id="e41a7-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="e41a7-127">Entrez **SpatialEFDesigner** en tant que le nom du projet et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="e41a7-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="e41a7-128">Créer un nouveau modèle à l’aide du Concepteur EF</span><span class="sxs-lookup"><span data-stu-id="e41a7-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="e41a7-129">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**</span><span class="sxs-lookup"><span data-stu-id="e41a7-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="e41a7-130">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles</span><span class="sxs-lookup"><span data-stu-id="e41a7-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="e41a7-131">Entrez **UniversityModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="e41a7-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="e41a7-132">Dans la page de l’Assistant Entity Data Model, sélectionnez **modèle vide** dans la boîte de dialogue Choisir le contenu du modèle</span><span class="sxs-lookup"><span data-stu-id="e41a7-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="e41a7-133">Cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="e41a7-133">Click **Finish**</span></span>

<span data-ttu-id="e41a7-134">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e41a7-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="e41a7-135">L'assistant exécute les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e41a7-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="e41a7-136">Génère le fichier EnumTestModel.edmx qui définit le modèle conceptuel, le modèle de stockage et le mappage entre les deux.</span><span class="sxs-lookup"><span data-stu-id="e41a7-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="e41a7-137">Définit la propriété Metadata Artifact Processing du fichier .edmx à incorporer dans l’Assembly de sortie pour les fichiers de métadonnées générées sont intégrés dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="e41a7-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="e41a7-138">Ajoute une référence aux assemblys suivants : EntityFramework System.ComponentModel.DataAnnotations et System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="e41a7-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="e41a7-139">Crée des fichiers UniversityModel.tt et UniversityModel.Context.tt et les ajoute dans le fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="e41a7-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="e41a7-140">Ces fichiers de modèle T4 génèrent le code qui définit le type DbContext dérivée et les types POCO qui mappent aux entités dans le modèle .edmx</span><span class="sxs-lookup"><span data-stu-id="e41a7-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="e41a7-141">Ajouter un nouveau Type d’entité</span><span class="sxs-lookup"><span data-stu-id="e41a7-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="e41a7-142">Cliquez sur une zone vide de l’aire de conception, sélectionnez **Add -&gt; entité**, s’affiche la boîte de dialogue nouvelle entité</span><span class="sxs-lookup"><span data-stu-id="e41a7-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="e41a7-143">Spécifiez **University** pour le type de nommer et spécifier **UniversityID** pour le nom de la propriété de clé, conservez le type **Int32**</span><span class="sxs-lookup"><span data-stu-id="e41a7-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="e41a7-144">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e41a7-144">Click **OK**</span></span>
4.  <span data-ttu-id="e41a7-145">Cliquez sur l’entité et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**</span><span class="sxs-lookup"><span data-stu-id="e41a7-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="e41a7-146">Renommer la nouvelle propriété à **nom**</span><span class="sxs-lookup"><span data-stu-id="e41a7-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="e41a7-147">Ajouter une autre propriété scalaire et renommez-le **emplacement** ouvrir la fenêtre Propriétés et modifier le type de la nouvelle propriété à **Geography**</span><span class="sxs-lookup"><span data-stu-id="e41a7-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="e41a7-148">Enregistrer le modèle et générer le projet</span><span class="sxs-lookup"><span data-stu-id="e41a7-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="e41a7-149">Lorsque vous générez, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="e41a7-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="e41a7-150">Vous pouvez ignorer ces avertissements, car une fois que nous avons choisi générer la base de données à partir du modèle, les erreurs disparaîtront.</span><span class="sxs-lookup"><span data-stu-id="e41a7-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="e41a7-151">Générer la base de données à partir du modèle</span><span class="sxs-lookup"><span data-stu-id="e41a7-151">Generate Database from Model</span></span>

<span data-ttu-id="e41a7-152">Maintenant, nous pouvons générer une base de données qui est basé sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="e41a7-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="e41a7-153">Cliquez sur un espace vide sur l’aire de conception et sélectionnez le Concepteur d’entités **générer la base de données à partir du modèle**</span><span class="sxs-lookup"><span data-stu-id="e41a7-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="e41a7-154">Choisir votre boîte de dialogue de connexion de données de l’Assistant génération de la base de données est affiché cliquez sur le **nouvelle connexion** bouton spécifier **(localdb)\\mssqllocaldb** pour le nom du serveur et  **Université** pour la base de données et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="e41a7-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="e41a7-155">Une boîte de dialogue vous demandant si vous souhaitez créer une base de données s’affiche, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e41a7-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="e41a7-156">Cliquez sur **suivant** et l’Assistant Création de base de données génère le langage de définition de données (DDL) pour créer une base de données le DDL généré est affiché dans le résumé et la Note de boîte de dialogue de paramètres, le DDL ne contenant pas de définition pour un table qui mappe au type énumération</span><span class="sxs-lookup"><span data-stu-id="e41a7-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="e41a7-157">Cliquez sur **Terminer** le script DDL ne s’exécute pas en cliquant sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="e41a7-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="e41a7-158">L’Assistant Création de base de données effectue les opérations suivantes : ouvre le **UniversityModel.edmx.sql** dans l’éditeur T-SQL génère les sections de mappage du schéma et magasin de l’EDMX fichier ajoute les informations de chaîne de connexion au fichier App.config</span><span class="sxs-lookup"><span data-stu-id="e41a7-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="e41a7-159">Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL et sélectionnez **Execute** établir une connexion à la boîte de dialogue serveur s’affiche, entrez les informations de connexion de l’étape 2 sur **Connect**</span><span class="sxs-lookup"><span data-stu-id="e41a7-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="e41a7-160">Pour afficher le schéma généré, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server, puis sélectionnez **actualiser**</span><span class="sxs-lookup"><span data-stu-id="e41a7-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="e41a7-161">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="e41a7-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="e41a7-162">Ouvrez le fichier Program.cs dans lequel la méthode Main est définie.</span><span class="sxs-lookup"><span data-stu-id="e41a7-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="e41a7-163">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="e41a7-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="e41a7-164">Le code ajoute deux nouveaux objets de l’université au contexte.</span><span class="sxs-lookup"><span data-stu-id="e41a7-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="e41a7-165">Propriétés spatiales sont initialisées à l’aide de la méthode DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="e41a7-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="e41a7-166">Le point géographique représenté en tant que WellKnownText est transmise à la méthode.</span><span class="sxs-lookup"><span data-stu-id="e41a7-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="e41a7-167">Le code enregistre ensuite les données.</span><span class="sxs-lookup"><span data-stu-id="e41a7-167">The code then saves the data.</span></span> <span data-ttu-id="e41a7-168">Ensuite, la requête LINQ qui retourne un objet de l’université où son emplacement le plus proche de l’emplacement spécifié, construction et l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e41a7-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="e41a7-169">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="e41a7-169">Compile and run the application.</span></span> <span data-ttu-id="e41a7-170">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="e41a7-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="e41a7-171">Pour afficher les données dans la base de données, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server et sélectionnez **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="e41a7-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="e41a7-172">Ensuite, cliquez sur le bouton droit de la souris sur la table et sélectionnez **afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="e41a7-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="e41a7-173">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="e41a7-173">Summary</span></span>

<span data-ttu-id="e41a7-174">Dans cette procédure pas à pas, nous avons vu comment mapper des types de données spatiales à l’aide d’Entity Framework Designer et comment utiliser des types de données spatiales dans le code.</span><span class="sxs-lookup"><span data-stu-id="e41a7-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
