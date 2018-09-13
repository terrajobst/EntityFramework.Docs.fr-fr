---
title: Héritage TPT concepteur - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489451"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="b4ae2-102">Héritage TPT Concepteur</span><span class="sxs-lookup"><span data-stu-id="b4ae2-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="b4ae2-103">Cette procédure pas à pas montre comment implémenter l’héritage de table par type (TPT) dans votre modèle à l’aide d’Entity Framework Designer (Concepteur d’EF).</span><span class="sxs-lookup"><span data-stu-id="b4ae2-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="b4ae2-104">L'héritage TPT (table par type) utilise une table distincte de la base de données pour maintenir des données des propriétés non héritées et des propriétés de clé pour chaque type de la hiérarchie d'héritage.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="b4ae2-105">Dans cette procédure pas à pas, nous allons mapper le **cours** (type de base), **OnlineCourse** (dérive de cours), et **OnsiteCourse** (dérive **cours**) entités à des tables portant le même nom.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="b4ae2-106">Nous allons créer un modèle à partir de la base de données, puis la modifier le modèle pour implémenter l’héritage TPT.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="b4ae2-107">Vous pouvez également commencer par le premier modèle, puis générez la base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="b4ae2-108">Le Concepteur EF utilise la stratégie de TPT par défaut et par conséquent, tout l’héritage dans le modèle sera mappée à des tables distinctes.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="b4ae2-109">Autres Options d’héritage</span><span class="sxs-lookup"><span data-stu-id="b4ae2-109">Other Inheritance Options</span></span>

<span data-ttu-id="b4ae2-110">Table par hiérarchie (TPH) est un autre type d’héritage dans la base d’une données table est utilisée pour gérer les données de tous les types d’entités dans une hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="b4ae2-111">Pour plus d’informations sur le mappage d’héritage Table par hiérarchie avec le Concepteur d’entités, consultez [l’héritage TPH de Concepteur EF](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="b4ae2-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="b4ae2-112">Notez que, la Table par concret Type héritage (TPC) et l’héritage mixte modèles sont pris en charge par le runtime Entity Framework, mais ne sont pas pris en charge par le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="b4ae2-113">Si vous souhaitez utiliser TPC ou mixte de l’héritage, vous avez deux options : utiliser Code First, ou modifier manuellement le fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="b4ae2-114">Si vous choisissez d’utiliser le fichier EDMX, la fenêtre Détails de mappage est placée dans le « mode sans échec » et vous ne serez pas en mesure d’utiliser le concepteur pour modifier les mappages.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4ae2-115">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b4ae2-115">Prerequisites</span></span>

<span data-ttu-id="b4ae2-116">Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b4ae2-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="b4ae2-117">Une version récente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="b4ae2-118">Le [base de données School exemple](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="b4ae2-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b4ae2-119">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="b4ae2-119">Set up the Project</span></span>

-   <span data-ttu-id="b4ae2-120">Ouvrez Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="b4ae2-121">Sélectionnez **fichier -&gt; nouveau -&gt; projet**</span><span class="sxs-lookup"><span data-stu-id="b4ae2-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="b4ae2-122">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="b4ae2-123">Entrez **TPTDBFirstSample** comme nom.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="b4ae2-124">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="b4ae2-125">Créer un modèle</span><span class="sxs-lookup"><span data-stu-id="b4ae2-125">Create a Model</span></span>

-   <span data-ttu-id="b4ae2-126">Cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="b4ae2-127">Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="b4ae2-128">Entrez **TPTModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="b4ae2-129">Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez \*\* Générer à partir de base de données \*\*, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-129">In the Choose Model Contents dialog box, select\*\* Generate from database\*\*, and then click **Next**.</span></span>
-   <span data-ttu-id="b4ae2-130">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-130">Click **New Connection**.</span></span>
    <span data-ttu-id="b4ae2-131">Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="b4ae2-132">La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="b4ae2-133">Dans la boîte de dialogue Choisir vos objets de base de données, sous le nœud Tables, sélectionnez le **département**, **cours, OnlineCourse et OnsiteCourse** tables.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="b4ae2-134">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-134">Click **Finish**.</span></span>

<span data-ttu-id="b4ae2-135">Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="b4ae2-136">Tous les objets que vous avez sélectionné dans la boîte de dialogue Choisir vos objets de base de données sont ajoutées au modèle.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="b4ae2-137">Implémenter l’héritage Table par Type</span><span class="sxs-lookup"><span data-stu-id="b4ae2-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="b4ae2-138">Sur l’aire de conception, cliquez sur le **OnlineCourse** type d’entité, puis sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="b4ae2-139">Dans le **propriétés** fenêtre, définissez la propriété de Type de Base **cours**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="b4ae2-140">Cliquez sur le **OnsiteCourse** type d’entité, puis sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="b4ae2-141">Dans le **propriétés** fenêtre, définissez la propriété de Type de Base **cours**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="b4ae2-142">Avec le bouton droit de l’association (la ligne) entre le **OnlineCourse** et **cours** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="b4ae2-143">Sélectionnez **supprimer du modèle**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="b4ae2-144">Avec le bouton droit de l’association entre le **OnsiteCourse** et **cours** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="b4ae2-145">Sélectionnez **supprimer du modèle**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="b4ae2-146">Nous va maintenant supprimer le **CourseID** propriété à partir de **OnlineCourse** et **OnsiteCourse** étant donné que ces classes héritent **CourseID** à partir de le **cours** type de base.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="b4ae2-147">Avec le bouton droit le **CourseID** propriété de la **OnlineCourse** type d’entité, puis **supprimer du modèle**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="b4ae2-148">Avec le bouton droit le **CourseID** propriété de la **OnsiteCourse** type d’entité, puis **supprimer du modèle**</span><span class="sxs-lookup"><span data-stu-id="b4ae2-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="b4ae2-149">L'héritage TPT (table par type) est maintenant implémenté.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="b4ae2-151">Utiliser le modèle</span><span class="sxs-lookup"><span data-stu-id="b4ae2-151">Use the Model</span></span>

<span data-ttu-id="b4ae2-152">Ouvrez le **Program.cs** fichier où le **Main** méthode est définie.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="b4ae2-153">Collez le code suivant dans le **Main** (fonction).</span><span class="sxs-lookup"><span data-stu-id="b4ae2-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="b4ae2-154">Le code s’exécute trois requêtes.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-154">The code executes three queries.</span></span> <span data-ttu-id="b4ae2-155">La première requête renvoie tous les **cours** associées au département spécifié.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="b4ae2-156">La deuxième requête utilise le **OfType** méthode pour retourner **OnlineCourses** associées au département spécifié.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="b4ae2-157">Retourne la troisième requête **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="b4ae2-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
