---
title: Fonctions table (TVF) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121374"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="f5d03-102">Fonctions table (TVF)</span><span class="sxs-lookup"><span data-stu-id="f5d03-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="f5d03-103">**EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="f5d03-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="f5d03-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="f5d03-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="f5d03-105">La procédure pas à pas vidéo et pas à pas montre comment mapper des fonctions table (TVF) à l’aide d’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="f5d03-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="f5d03-106">Il montre également comment appeler une fonction table à partir d’une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="f5d03-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="f5d03-107">Fonctions table est actuellement pris en charge uniquement dans le flux de travail première base de données.</span><span class="sxs-lookup"><span data-stu-id="f5d03-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="f5d03-108">Prise en charge de la fonction table a été introduite dans Entity Framework version 5.</span><span class="sxs-lookup"><span data-stu-id="f5d03-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="f5d03-109">Notez que pour utiliser les nouvelles fonctionnalités telles que les fonctions table, énumérations et types spatiaux, vous devez cibler .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="f5d03-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="f5d03-110">Visual Studio 2012 cible .NET 4.5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="f5d03-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="f5d03-111">Fonctions table est très semblables aux procédures stockées avec une différence importante : le résultat d’une fonction table est composable.</span><span class="sxs-lookup"><span data-stu-id="f5d03-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="f5d03-112">Cela signifie que les résultats à partir d’une fonction table peuvent être utilisés dans une requête LINQ tandis que les résultats d’une procédure stockée ne peut pas.</span><span class="sxs-lookup"><span data-stu-id="f5d03-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="f5d03-113">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="f5d03-113">Watch the video</span></span>

<span data-ttu-id="f5d03-114">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="f5d03-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="f5d03-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="f5d03-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="f5d03-116">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="f5d03-116">Pre-Requisites</span></span>

<span data-ttu-id="f5d03-117">Pour effectuer cette procédure pas à pas, vous devez :</span><span class="sxs-lookup"><span data-stu-id="f5d03-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="f5d03-118">Installer le [base de données School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="f5d03-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="f5d03-119">Une version récente de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5d03-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f5d03-120">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="f5d03-120">Set up the Project</span></span>

1.  <span data-ttu-id="f5d03-121">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5d03-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="f5d03-122">Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**</span><span class="sxs-lookup"><span data-stu-id="f5d03-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="f5d03-123">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle</span><span class="sxs-lookup"><span data-stu-id="f5d03-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="f5d03-124">Entrez **TVF** en tant que le nom du projet et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f5d03-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="f5d03-125">Ajouter une fonction table à la base de données</span><span class="sxs-lookup"><span data-stu-id="f5d03-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="f5d03-126">Sélectionnez **vue -&gt; Explorateur d’objets SQL Server**</span><span class="sxs-lookup"><span data-stu-id="f5d03-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="f5d03-127">Si la base de données locale n’est pas dans la liste des serveurs : avec le bouton droit sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser la valeur par défaut **l’authentification Windows** pour se connecter au serveur de base de données locale</span><span class="sxs-lookup"><span data-stu-id="f5d03-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="f5d03-128">Développez le nœud de base de données locale</span><span class="sxs-lookup"><span data-stu-id="f5d03-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="f5d03-129">Sous le nœud bases de données, cliquez sur le nœud de base de données School et sélectionnez **nouvelle requête...**</span><span class="sxs-lookup"><span data-stu-id="f5d03-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="f5d03-130">Dans l’éditeur T-SQL, collez la définition de fonction table suivante</span><span class="sxs-lookup"><span data-stu-id="f5d03-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="f5d03-131">Cliquez sur le bouton droit de la souris sur l’éditeur T-SQL et sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="f5d03-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="f5d03-132">La fonction GetStudentGradesForCourse est ajoutée à la base de données School</span><span class="sxs-lookup"><span data-stu-id="f5d03-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="f5d03-133">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="f5d03-133">Create a Model</span></span>

1.  <span data-ttu-id="f5d03-134">Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**</span><span class="sxs-lookup"><span data-stu-id="f5d03-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="f5d03-135">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le **modèles** volet</span><span class="sxs-lookup"><span data-stu-id="f5d03-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="f5d03-136">Entrez **TVFModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="f5d03-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="f5d03-137">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="f5d03-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="f5d03-138">Cliquez sur **nouvelle connexion** entrée **(localdb)\\mssqllocaldb** dans le texte de nom de serveur zone entrée **School** pour la base de données nom puis cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="f5d03-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="f5d03-139">Dans la choisir vos objets de base de données boîte de dialogue, sous le **Tables** nœud, sélectionnez le **personne**, **StudentGrade**, et **cours** tables</span><span class="sxs-lookup"><span data-stu-id="f5d03-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="f5d03-140">Sélectionnez le **GetStudentGradesForCourse** fonction situé sous le **de procédures stockées et fonctions** nœud Remarque, à compter de Visual Studio 2012, le Concepteur d’entités vous permet à l’importation de lot Procédures stockées et fonctions</span><span class="sxs-lookup"><span data-stu-id="f5d03-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="f5d03-141">Cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="f5d03-141">Click **Finish**</span></span>
9.  <span data-ttu-id="f5d03-142">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f5d03-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="f5d03-143">Tous les objets que vous avez sélectionné dans le **choisir vos objets de base de données** boîte de dialogue sont ajoutés au modèle.</span><span class="sxs-lookup"><span data-stu-id="f5d03-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="f5d03-144">Par défaut, la forme du résultat de chaque procédure stockée importée ou la fonction deviennent automatiquement un nouveau type complexe dans votre modèle d’entité.</span><span class="sxs-lookup"><span data-stu-id="f5d03-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="f5d03-145">Mais nous voulons mapper les résultats de la fonction GetStudentGradesForCourse à l’entité StudentGrade : cliquez sur l’aire de conception et sélectionnez **Explorateur de modèles** dans Explorateur de modèles, sélectionnez **Function Imports**, puis double-cliquez sur le **GetStudentGradesForCourse** fonction dans le modifier importation de fonction boîte de dialogue, sélectionnez **entités** et choisissez **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="f5d03-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="f5d03-146">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="f5d03-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="f5d03-147">Ouvrez le fichier dans lequel la méthode Main est définie.</span><span class="sxs-lookup"><span data-stu-id="f5d03-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="f5d03-148">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="f5d03-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="f5d03-149">Le code suivant montre comment créer une requête qui utilise une fonction table.</span><span class="sxs-lookup"><span data-stu-id="f5d03-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="f5d03-150">La requête projette les résultats dans un type anonyme qui contient le titre du cours connexe et les étudiants associées avec un niveau supérieur ou égal à 3.5.</span><span class="sxs-lookup"><span data-stu-id="f5d03-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="f5d03-151">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="f5d03-151">Compile and run the application.</span></span> <span data-ttu-id="f5d03-152">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="f5d03-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="f5d03-153">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="f5d03-153">Summary</span></span>

<span data-ttu-id="f5d03-154">Dans cette procédure pas à pas, nous avons vu comment mapper des fonctions Table (TVF) à l’aide d’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="f5d03-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="f5d03-155">Il a également montré comment appeler une fonction table à partir d’une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="f5d03-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
