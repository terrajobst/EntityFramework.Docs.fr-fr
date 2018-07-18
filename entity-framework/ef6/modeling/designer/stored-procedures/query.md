---
title: Requête concepteur procédures stockées - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
caps.latest.revision: 3
ms.openlocfilehash: a08c1afc02266b35372a49fca1e829963e4785b2
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121140"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="66189-102">Requêtes du Générateur de procédures stockées</span><span class="sxs-lookup"><span data-stu-id="66189-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="66189-103">Cette procédure pas à pas montrent comment utiliser Entity Framework Designer (Concepteur d’EF) pour importer des procédures stockées dans un modèle, puis appelez les procédures stockées importés pour récupérer les résultats.</span><span class="sxs-lookup"><span data-stu-id="66189-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="66189-104">Notez que Code First ne prend pas en charge le mappage de procédures stockées ou fonctions.</span><span class="sxs-lookup"><span data-stu-id="66189-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="66189-105">Toutefois, vous pouvez appeler des procédures stockées ou fonctions à l’aide de la méthode System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="66189-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="66189-106">Exemple :</span><span class="sxs-lookup"><span data-stu-id="66189-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="66189-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="66189-107">Prerequisites</span></span>

<span data-ttu-id="66189-108">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="66189-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="66189-109">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66189-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="66189-110">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="66189-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="66189-111">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="66189-111">Set up the Project</span></span>

-   <span data-ttu-id="66189-112">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="66189-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="66189-113">Sélectionnez **fichier -&gt; nouveau -&gt; projet**</span><span class="sxs-lookup"><span data-stu-id="66189-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="66189-114">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="66189-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="66189-115">Entrez **EFwithSProcsSample** comme nom.</span><span class="sxs-lookup"><span data-stu-id="66189-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="66189-116">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="66189-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="66189-117">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="66189-117">Create a Model</span></span>

-   <span data-ttu-id="66189-118">Cliquez sur le projet dans l’Explorateur de solutions et sélectionnez **Add -&gt; un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="66189-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="66189-119">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="66189-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="66189-120">Entrez **EFwithSProcsModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="66189-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="66189-121">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="66189-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="66189-122">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="66189-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="66189-123">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="66189-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="66189-124">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="66189-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="66189-125">Dans la boîte de dialogue Choisir vos objets de base de données, vérifiez le **Tables** case à cocher pour sélectionner toutes les tables.</span><span class="sxs-lookup"><span data-stu-id="66189-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="66189-126">En outre, sélectionnez les procédures stockées suivantes sous la **de procédures stockées et fonctions** nœud : **GetStudentGrades** et **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="66189-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Import](~/ef6/media/import.jpg)

    <span data-ttu-id="66189-128">*À partir de Visual Studio 2012, le Concepteur EF prend en charge l’importation en bloc des procédures stockées. Le **importation sélectionné des procédures stockées et fonctions dans theentity modèle** est activée par défaut.*</span><span class="sxs-lookup"><span data-stu-id="66189-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="66189-129">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="66189-129">Click **Finish**.</span></span>

<span data-ttu-id="66189-130">Par défaut, la forme du résultat de chaque procédure stockée importée ou d’une fonction qui retourne plusieurs colonnes deviennent automatiquement un nouveau type complexe.</span><span class="sxs-lookup"><span data-stu-id="66189-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="66189-131">Dans cet exemple, nous voulons mapper les résultats de la **GetStudentGrades** fonctionner à la **StudentGrade** entité et les résultats de la **GetDepartmentName** à **aucun** (**aucun** est la valeur par défaut).</span><span class="sxs-lookup"><span data-stu-id="66189-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="66189-132">Pour une importation de fonction retourner un type d’entité, les colonnes retournées par la procédure stockée correspondante doivent correspondre exactement les propriétés scalaires du type d’entité retourné.</span><span class="sxs-lookup"><span data-stu-id="66189-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="66189-133">Une importation de fonction peut également retourner des collections de types simples, des types complexes ou aucune valeur.</span><span class="sxs-lookup"><span data-stu-id="66189-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="66189-134">Cliquez sur l’aire de conception et sélectionnez **Explorateur de modèles**.</span><span class="sxs-lookup"><span data-stu-id="66189-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="66189-135">Dans **Explorateur de modèles**, sélectionnez **Function Imports**, puis double-cliquez sur le **GetStudentGrades** (fonction).</span><span class="sxs-lookup"><span data-stu-id="66189-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="66189-136">Dans la boîte de dialogue Modifier une importation de fonction, sélectionnez **entités** et choisissez **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="66189-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="66189-137">*Le **importation de fonction est composable** case à cocher en haut de la **Function Imports** boîte de dialogue vous permet de mapper aux fonctions composables. Si vous ne cochez pas cette case, seules les fonctions composables (fonctions table) seront affiche dans le **procédure stockée / nom de la fonction** liste déroulante. Si vous ne cochez pas cette case, seules les fonctions non composables seront affichera dans la liste.*</span><span class="sxs-lookup"><span data-stu-id="66189-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="66189-138">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="66189-138">Use the Model</span></span>

<span data-ttu-id="66189-139">Ouvrez le **Program.cs** fichier où le **Main** méthode est définie.</span><span class="sxs-lookup"><span data-stu-id="66189-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="66189-140">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="66189-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="66189-141">Le code appelle deux procédures stockées : **GetStudentGrades** (retourne **StudentGrades** spécifié *StudentId*) et **GetDepartmentName** (retourne le nom du service dans le paramètre de sortie).</span><span class="sxs-lookup"><span data-stu-id="66189-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="66189-142">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="66189-142">Compile and run the application.</span></span> <span data-ttu-id="66189-143">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="66189-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="66189-144">Paramètres de sortie</span><span class="sxs-lookup"><span data-stu-id="66189-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="66189-145">Si les paramètres de sortie sont utilisés, leurs valeurs ne sera pas disponibles jusqu'à ce que les résultats ont été lues entièrement.</span><span class="sxs-lookup"><span data-stu-id="66189-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="66189-146">Il s’agit en raison du comportement sous-jacent de DbDataReader, consultez [extraction de données à l’aide d’un DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="66189-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
