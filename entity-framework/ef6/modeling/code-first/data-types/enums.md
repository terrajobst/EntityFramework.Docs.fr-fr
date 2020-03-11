---
title: Prise en charge d’enum-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419107"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="86dd4-102">Prise en charge d’enum-Code First</span><span class="sxs-lookup"><span data-stu-id="86dd4-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="86dd4-103">**EF5 uniquement** : les fonctionnalités, les API, etc. présentées dans cette page ont été introduites dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="86dd4-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="86dd4-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="86dd4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="86dd4-105">Cette vidéo et la procédure pas à pas expliquent comment utiliser les types ENUM avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="86dd4-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="86dd4-106">Il montre également comment utiliser des enums dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="86dd4-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="86dd4-107">Cette procédure pas à pas utilise Code First pour créer une base de données, mais vous pouvez également utiliser des [Code First pour mapper à une base de données existante](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="86dd4-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="86dd4-108">La prise en charge des énumérations a été introduite dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="86dd4-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="86dd4-109">Pour utiliser les nouvelles fonctionnalités telles que les énumérations, les types de données spatiales et les fonctions table, vous devez cibler .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="86dd4-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="86dd4-110">Visual Studio 2012 cible .NET 4,5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="86dd4-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="86dd4-111">Dans Entity Framework, une énumération peut avoir les types sous-jacents suivants : **Byte**, **Int16**, **Int32**, **Int64** ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="86dd4-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="86dd4-112">Regarder la vidéo</span><span class="sxs-lookup"><span data-stu-id="86dd4-112">Watch the video</span></span>
<span data-ttu-id="86dd4-113">Cette vidéo montre comment utiliser les types ENUM avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="86dd4-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="86dd4-114">Il montre également comment utiliser des enums dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="86dd4-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="86dd4-115">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="86dd4-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="86dd4-116">**Vidéo**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="86dd4-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="86dd4-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="86dd4-117">Pre-Requisites</span></span>

<span data-ttu-id="86dd4-118">Pour effectuer cette procédure pas à pas, vous devez avoir installé Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="86dd4-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="86dd4-119">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="86dd4-119">Set up the Project</span></span>

1.  <span data-ttu-id="86dd4-120">Ouvrir Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="86dd4-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="86dd4-121">Dans le menu **fichier** , pointez sur **nouveau**, puis cliquez sur **projet** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="86dd4-122">Dans le volet gauche, cliquez sur **Visual C\#** , puis sélectionnez le modèle **console** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="86dd4-123">Entrez **EnumCodeFirst** comme nom du projet, puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="86dd4-124">Définir un nouveau modèle à l’aide de Code First</span><span class="sxs-lookup"><span data-stu-id="86dd4-124">Define a New Model using Code First</span></span>

<span data-ttu-id="86dd4-125">Lors de l’utilisation de Code First développement, vous commencez généralement par écrire des classes .NET Framework qui définissent votre modèle conceptuel (domaine).</span><span class="sxs-lookup"><span data-stu-id="86dd4-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="86dd4-126">Le code ci-dessous définit la classe Department.</span><span class="sxs-lookup"><span data-stu-id="86dd4-126">The code below defines the Department class.</span></span>

<span data-ttu-id="86dd4-127">Le code définit également l’énumération DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="86dd4-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="86dd4-128">Par défaut, l’énumération est de type **int** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="86dd4-129">La propriété Name de la classe Department est du type DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="86dd4-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="86dd4-130">Ouvrez le fichier Program.cs et collez les définitions de classe suivantes.</span><span class="sxs-lookup"><span data-stu-id="86dd4-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="86dd4-131">Définir le type dérivé DbContext</span><span class="sxs-lookup"><span data-stu-id="86dd4-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="86dd4-132">En plus de définir des entités, vous devez définir une classe qui dérive de DbContext et expose les propriétés DbSet&lt;TEntity&gt;.</span><span class="sxs-lookup"><span data-stu-id="86dd4-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="86dd4-133">Les propriétés DbSet&lt;TEntity&gt; permettent au contexte de savoir quels types vous souhaitez inclure dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="86dd4-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="86dd4-134">Une instance du type dérivé DbContext gère les objets d’entité au moment de l’exécution, ce qui comprend le remplissage des objets avec les données d’une base de données, le suivi des modifications et la persistance des données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="86dd4-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="86dd4-135">Les types DbContext et DbSet sont définis dans l’assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="86dd4-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="86dd4-136">Nous allons ajouter une référence à cette DLL à l’aide du package NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="86dd4-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="86dd4-137">Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet.</span><span class="sxs-lookup"><span data-stu-id="86dd4-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="86dd4-138">Sélectionnez **gérer les packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="86dd4-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="86dd4-139">Dans la boîte de dialogue gérer les packages NuGet, sélectionnez l’onglet **en ligne** et choisissez le package **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="86dd4-140">Cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="86dd4-140">Click **Install**</span></span>

<span data-ttu-id="86dd4-141">Notez qu’en plus de l’assembly EntityFramework, les références aux assemblys System. ComponentModel. DataAnnotations et System. Data. Entity sont également ajoutées.</span><span class="sxs-lookup"><span data-stu-id="86dd4-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="86dd4-142">En haut du fichier Program.cs, ajoutez l’instruction using suivante :</span><span class="sxs-lookup"><span data-stu-id="86dd4-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="86dd4-143">Dans Program.cs, ajoutez la définition de contexte.</span><span class="sxs-lookup"><span data-stu-id="86dd4-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="86dd4-144">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="86dd4-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="86dd4-145">Ouvrez le fichier Program.cs dans lequel la méthode main est définie.</span><span class="sxs-lookup"><span data-stu-id="86dd4-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="86dd4-146">Ajoutez le code suivant à la fonction main.</span><span class="sxs-lookup"><span data-stu-id="86dd4-146">Add the following code into the Main function.</span></span> <span data-ttu-id="86dd4-147">Le code ajoute un nouvel objet Department au contexte.</span><span class="sxs-lookup"><span data-stu-id="86dd4-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="86dd4-148">Il enregistre ensuite les données.</span><span class="sxs-lookup"><span data-stu-id="86dd4-148">It then saves the data.</span></span> <span data-ttu-id="86dd4-149">Le code exécute également une requête LINQ qui retourne un service dont le nom est DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="86dd4-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="86dd4-150">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="86dd4-150">Compile and run the application.</span></span> <span data-ttu-id="86dd4-151">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="86dd4-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="86dd4-152">Afficher la base de données générée</span><span class="sxs-lookup"><span data-stu-id="86dd4-152">View the Generated Database</span></span>

<span data-ttu-id="86dd4-153">Lorsque vous exécutez l’application la première fois, la Entity Framework crée une base de données pour vous.</span><span class="sxs-lookup"><span data-stu-id="86dd4-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="86dd4-154">Étant donné que Visual Studio 2012 est installé, la base de données sera créée sur l’instance de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="86dd4-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="86dd4-155">Par défaut, le Entity Framework nomme la base de données après le nom qualifié complet du contexte dérivé (pour cet exemple, il s’agit de **EnumCodeFirst. EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="86dd4-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="86dd4-156">La prochaine fois que la base de données existante sera utilisée.</span><span class="sxs-lookup"><span data-stu-id="86dd4-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="86dd4-157">Notez que si vous apportez des modifications à votre modèle après la création de la base de données, vous devez utiliser Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="86dd4-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="86dd4-158">Pour obtenir un exemple d’utilisation de migrations, consultez [Code First à une nouvelle base de données](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="86dd4-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="86dd4-159">Pour afficher la base de données et les données, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="86dd4-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="86dd4-160">Dans le menu principal de Visual Studio 2012, sélectionnez **afficher** -&gt; **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="86dd4-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="86dd4-161">Si la base de données locale ne figure pas dans la liste des serveurs, cliquez sur le bouton droit de la souris sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser l' **authentification Windows** par défaut pour vous connecter à l’instance de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="86dd4-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="86dd4-162">Développez le nœud de base de données locale</span><span class="sxs-lookup"><span data-stu-id="86dd4-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="86dd4-163">Dérouler le dossier **bases de données** pour afficher la nouvelle base de données et accéder à la table **Department** , qui code First ne crée pas de table mappée au type d’énumération</span><span class="sxs-lookup"><span data-stu-id="86dd4-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="86dd4-164">Pour afficher les données, cliquez avec le bouton droit sur la table et sélectionnez **afficher les données** .</span><span class="sxs-lookup"><span data-stu-id="86dd4-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="86dd4-165">Résumé</span><span class="sxs-lookup"><span data-stu-id="86dd4-165">Summary</span></span>

<span data-ttu-id="86dd4-166">Dans cette procédure pas à pas, nous avons vu comment utiliser les types ENUM avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="86dd4-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
