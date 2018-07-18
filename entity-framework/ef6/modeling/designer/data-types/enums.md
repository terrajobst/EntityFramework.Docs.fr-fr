---
title: Prise en charge enum - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
caps.latest.revision: 3
ms.openlocfilehash: cbf9b01fcbe21274ff3644c6ae6bc8fdfd338e3b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121349"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="2241a-102">Prise en charge de l’enum - Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="2241a-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="2241a-103">**EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="2241a-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="2241a-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="2241a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2241a-105">Cette procédure pas à pas vidéo et pas à pas montre comment utiliser les types enum avec Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="2241a-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="2241a-106">Il montre également comment utiliser les énumérations dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="2241a-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="2241a-107">Cette procédure pas à pas utilise Model First pour créer une nouvelle base de données, mais le Concepteur EF peut également servir avec la [Database First](~/ef6/modeling/designer/workflows/database-first.md) flux de travail pour mapper à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="2241a-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="2241a-108">Prise en charge de l’énumération a été introduite dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="2241a-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="2241a-109">Pour utiliser les nouvelles fonctionnalités telles que les énumérations, les types de données spatiales et les fonctions table, vous devez cibler .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="2241a-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="2241a-110">Visual Studio 2012 cible .NET 4.5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="2241a-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="2241a-111">Dans Entity Framework, une énumération peut avoir des types sous-jacents suivants : **octets**, **Int16**, **Int32**, **Int64** , ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="2241a-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="2241a-112">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="2241a-112">Watch the Video</span></span>
<span data-ttu-id="2241a-113">Cette vidéo montre comment utiliser les types enum avec Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="2241a-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="2241a-114">Il montre également comment utiliser les énumérations dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="2241a-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="2241a-115">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="2241a-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="2241a-116">**Vidéo**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="2241a-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2241a-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="2241a-117">Pre-Requisites</span></span>

<span data-ttu-id="2241a-118">Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="2241a-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2241a-119">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="2241a-119">Set up the Project</span></span>

1.  <span data-ttu-id="2241a-120">Ouvrez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2241a-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="2241a-121">Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**</span><span class="sxs-lookup"><span data-stu-id="2241a-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="2241a-122">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle</span><span class="sxs-lookup"><span data-stu-id="2241a-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="2241a-123">Entrez **EnumEFDesigner** en tant que le nom du projet et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="2241a-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="2241a-124">Créer un nouveau modèle à l’aide du Concepteur EF</span><span class="sxs-lookup"><span data-stu-id="2241a-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="2241a-125">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**</span><span class="sxs-lookup"><span data-stu-id="2241a-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="2241a-126">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles</span><span class="sxs-lookup"><span data-stu-id="2241a-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="2241a-127">Entrez **EnumTestModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="2241a-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="2241a-128">Dans la page de l’Assistant Entity Data Model, sélectionnez **modèle vide** dans la boîte de dialogue Choisir le contenu du modèle</span><span class="sxs-lookup"><span data-stu-id="2241a-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="2241a-129">Cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="2241a-129">Click **Finish**</span></span>

<span data-ttu-id="2241a-130">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2241a-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="2241a-131">L'assistant exécute les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="2241a-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="2241a-132">Génère le fichier EnumTestModel.edmx qui définit le modèle conceptuel, le modèle de stockage et le mappage entre les deux.</span><span class="sxs-lookup"><span data-stu-id="2241a-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="2241a-133">Définit la propriété Metadata Artifact Processing du fichier .edmx à incorporer dans l’Assembly de sortie pour les fichiers de métadonnées générées sont intégrés dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="2241a-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="2241a-134">Ajoute une référence aux assemblys suivants : EntityFramework System.ComponentModel.DataAnnotations et System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="2241a-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="2241a-135">Crée des fichiers EnumTestModel.tt et EnumTestModel.Context.tt et les ajoute dans le fichier .edmx.</span><span class="sxs-lookup"><span data-stu-id="2241a-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="2241a-136">Ces fichiers de modèle T4 génèrent le code qui définit le type DbContext dérivée et les types POCO qui mappent aux entités dans le modèle .edmx.</span><span class="sxs-lookup"><span data-stu-id="2241a-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="2241a-137">Ajouter un nouveau Type d’entité</span><span class="sxs-lookup"><span data-stu-id="2241a-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="2241a-138">Cliquez sur une zone vide de l’aire de conception, sélectionnez **Add -&gt; entité**, s’affiche la boîte de dialogue nouvelle entité</span><span class="sxs-lookup"><span data-stu-id="2241a-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="2241a-139">Spécifiez **département** pour le type de nommer et spécifier **DepartmentID** pour le nom de la propriété de clé, conservez le type **Int32**</span><span class="sxs-lookup"><span data-stu-id="2241a-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="2241a-140">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2241a-140">Click **OK**</span></span>
4.  <span data-ttu-id="2241a-141">Cliquez sur l’entité et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**</span><span class="sxs-lookup"><span data-stu-id="2241a-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="2241a-142">Renommer la nouvelle propriété à **nom**</span><span class="sxs-lookup"><span data-stu-id="2241a-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="2241a-143">Modifier le type de la nouvelle propriété à **Int32** (par défaut, la nouvelle propriété est de type chaîne) pour modifier le type, ouvrez la fenêtre Propriétés et affectez à la propriété de Type **Int32**</span><span class="sxs-lookup"><span data-stu-id="2241a-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="2241a-144">Ajouter une autre propriété scalaire et renommez-le **Budget**, modifiez le type de **décimal**</span><span class="sxs-lookup"><span data-stu-id="2241a-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="2241a-145">Ajouter un Type Enum</span><span class="sxs-lookup"><span data-stu-id="2241a-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="2241a-146">Dans Entity Framework Designer, cliquez sur la propriété Name, sélectionnez **convertir enum**</span><span class="sxs-lookup"><span data-stu-id="2241a-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="2241a-148">Dans le **Enum ajouter** boîte de dialogue Entrez **DepartmentNames** pour le nom de Type Enum, modifiez le Type sous-jacent **Int32**, puis ajoutez les membres suivants pour le type : anglais, Mathématiques et des prix</span><span class="sxs-lookup"><span data-stu-id="2241a-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="2241a-150">Appuyez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="2241a-150">Press **OK**</span></span>
4.  <span data-ttu-id="2241a-151">Enregistrer le modèle et générer le projet</span><span class="sxs-lookup"><span data-stu-id="2241a-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="2241a-152">Lorsque vous générez, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="2241a-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="2241a-153">Vous pouvez ignorer ces avertissements, car une fois que nous avons choisi générer la base de données à partir du modèle, les erreurs disparaîtront.</span><span class="sxs-lookup"><span data-stu-id="2241a-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="2241a-154">Si vous examinez la fenêtre Propriétés, vous pouvez remarquer que le type de la propriété de nom a été changé en **DepartmentNames** et le type d’énumération qui vient d’être ajoutée a été ajouté à la liste des types.</span><span class="sxs-lookup"><span data-stu-id="2241a-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="2241a-155">Si vous basculez vers la fenêtre Explorateur de modèles, vous verrez que le type a été également ajouté au nœud Types Enum.</span><span class="sxs-lookup"><span data-stu-id="2241a-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="2241a-157">Vous pouvez également ajouter des nouveaux types enum à partir de cette fenêtre en cliquant sur le bouton droit de la souris et en sélectionnant **ajouter un Enum Type**.</span><span class="sxs-lookup"><span data-stu-id="2241a-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="2241a-158">Une fois que le type est créé il apparaîtra dans la liste des types et vous pourrez associer à une propriété</span><span class="sxs-lookup"><span data-stu-id="2241a-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="2241a-159">Générer la base de données à partir du modèle</span><span class="sxs-lookup"><span data-stu-id="2241a-159">Generate Database from Model</span></span>

<span data-ttu-id="2241a-160">Maintenant, nous pouvons générer une base de données qui est basé sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="2241a-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="2241a-161">Cliquez sur un espace vide sur l’aire de conception et sélectionnez le Concepteur d’entités **générer la base de données à partir du modèle**</span><span class="sxs-lookup"><span data-stu-id="2241a-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="2241a-162">Choisir votre boîte de dialogue de connexion de données de l’Assistant génération de la base de données est affiché cliquez sur le **nouvelle connexion** bouton spécifier **(localdb)\\mssqllocaldb** pour le nom du serveur et  **EnumTest** pour la base de données et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="2241a-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="2241a-163">Une boîte de dialogue vous demandant si vous souhaitez créer une base de données s’affiche, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2241a-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="2241a-164">Cliquez sur **suivant** et l’Assistant Création de base de données génère le langage de définition de données (DDL) pour créer une base de données le DDL généré est affiché dans le résumé et la Note de boîte de dialogue de paramètres, le DDL ne contenant pas de définition pour un table qui mappe au type énumération</span><span class="sxs-lookup"><span data-stu-id="2241a-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="2241a-165">Cliquez sur **Terminer** le script DDL ne s’exécute pas en cliquant sur Terminer.</span><span class="sxs-lookup"><span data-stu-id="2241a-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="2241a-166">L’Assistant Création de base de données effectue les opérations suivantes : ouvre le **EnumTest.edmx.sql** dans l’éditeur T-SQL génère les sections de mappage du schéma et magasin de l’EDMX fichier ajoute les informations de chaîne de connexion au fichier App.config</span><span class="sxs-lookup"><span data-stu-id="2241a-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="2241a-167">Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL et sélectionnez **Execute** établir une connexion à la boîte de dialogue serveur s’affiche, entrez les informations de connexion de l’étape 2 sur **Connect**</span><span class="sxs-lookup"><span data-stu-id="2241a-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="2241a-168">Pour afficher le schéma généré, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server, puis sélectionnez **actualiser**</span><span class="sxs-lookup"><span data-stu-id="2241a-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="2241a-169">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="2241a-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="2241a-170">Ouvrez le fichier Program.cs dans lequel la méthode Main est définie.</span><span class="sxs-lookup"><span data-stu-id="2241a-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="2241a-171">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="2241a-171">Add the following code into the Main function.</span></span> <span data-ttu-id="2241a-172">Le code ajoute un nouvel objet de service pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="2241a-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="2241a-173">Ensuite, il enregistre les données.</span><span class="sxs-lookup"><span data-stu-id="2241a-173">It then saves the data.</span></span> <span data-ttu-id="2241a-174">Le code exécute également une requête LINQ qui retourne un département où le nom est DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="2241a-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="2241a-175">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="2241a-175">Compile and run the application.</span></span> <span data-ttu-id="2241a-176">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="2241a-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="2241a-177">Pour afficher les données dans la base de données, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server et sélectionnez **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="2241a-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="2241a-178">Ensuite, cliquez sur le bouton droit de la souris sur la table et sélectionnez **afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="2241a-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="2241a-179">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="2241a-179">Summary</span></span>

<span data-ttu-id="2241a-180">Dans cette procédure pas à pas, nous avons vu comment mapper des types d’énumération à l’aide d’Entity Framework Designer et comment utiliser des énumérations dans le code.</span><span class="sxs-lookup"><span data-stu-id="2241a-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
