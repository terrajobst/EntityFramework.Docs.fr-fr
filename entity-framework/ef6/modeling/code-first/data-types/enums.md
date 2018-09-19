---
title: Prise en charge de l’enum - Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283718"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="17ac7-102">Prise en charge de l’enum - Code First</span><span class="sxs-lookup"><span data-stu-id="17ac7-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="17ac7-103">**EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="17ac7-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="17ac7-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="17ac7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="17ac7-105">Cette procédure pas à pas vidéo et pas à pas montre comment utiliser les types enum avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="17ac7-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="17ac7-106">Il montre également comment utiliser les énumérations dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="17ac7-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="17ac7-107">Cette procédure pas à pas utilise Code First pour créer une nouvelle base de données, mais vous pouvez également utiliser [Code First pour mapper à une base de données existante](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="17ac7-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="17ac7-108">Prise en charge de l’énumération a été introduite dans Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="17ac7-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="17ac7-109">Pour utiliser les nouvelles fonctionnalités telles que les énumérations, les types de données spatiales et les fonctions table, vous devez cibler .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="17ac7-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="17ac7-110">Visual Studio 2012 cible .NET 4.5 par défaut.</span><span class="sxs-lookup"><span data-stu-id="17ac7-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="17ac7-111">Dans Entity Framework, une énumération peut avoir des types sous-jacents suivants : **octets**, **Int16**, **Int32**, **Int64** , ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="17ac7-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="17ac7-112">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="17ac7-112">Watch the video</span></span>
<span data-ttu-id="17ac7-113">Cette vidéo montre comment utiliser les types enum avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="17ac7-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="17ac7-114">Il montre également comment utiliser les énumérations dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="17ac7-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="17ac7-115">**Présenté par**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="17ac7-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="17ac7-116">**Vidéo**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="17ac7-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="17ac7-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="17ac7-117">Pre-Requisites</span></span>

<span data-ttu-id="17ac7-118">Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="17ac7-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="17ac7-119">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="17ac7-119">Set up the Project</span></span>

1.  <span data-ttu-id="17ac7-120">Ouvrez Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="17ac7-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="17ac7-121">Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**</span><span class="sxs-lookup"><span data-stu-id="17ac7-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="17ac7-122">Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle</span><span class="sxs-lookup"><span data-stu-id="17ac7-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="17ac7-123">Entrez **EnumCodeFirst** en tant que le nom du projet et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="17ac7-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="17ac7-124">Définir un nouveau modèle à l’aide de Code First</span><span class="sxs-lookup"><span data-stu-id="17ac7-124">Define a New Model using Code First</span></span>

<span data-ttu-id="17ac7-125">Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine).</span><span class="sxs-lookup"><span data-stu-id="17ac7-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="17ac7-126">Le code suivant définit la classe de service.</span><span class="sxs-lookup"><span data-stu-id="17ac7-126">The code below defines the Department class.</span></span>

<span data-ttu-id="17ac7-127">Le code définit également l’énumération DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="17ac7-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="17ac7-128">Par défaut, l’énumération est de **int** type.</span><span class="sxs-lookup"><span data-stu-id="17ac7-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="17ac7-129">La propriété nom de la classe de service est du type DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="17ac7-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="17ac7-130">Ouvrez le fichier Program.cs et collez les définitions de classe suivantes.</span><span class="sxs-lookup"><span data-stu-id="17ac7-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="17ac7-131">Définir le DbContext de Type dérivé</span><span class="sxs-lookup"><span data-stu-id="17ac7-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="17ac7-132">Outre la définition des entités, vous devez définir une classe qui dérive de DbContext et expose DbSet&lt;TEntity&gt; propriétés.</span><span class="sxs-lookup"><span data-stu-id="17ac7-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="17ac7-133">Le DbSet&lt;TEntity&gt; propriétés permettent le contexte de connaître les types que vous souhaitez inclure dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="17ac7-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="17ac7-134">Une instance du type DbContext dérivée gère les objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, modifier le suivi et la persistance des données à la base de données.</span><span class="sxs-lookup"><span data-stu-id="17ac7-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="17ac7-135">Les types DbContext et DbSet sont définis dans l’assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="17ac7-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="17ac7-136">Nous allons ajouter une référence à cette DLL à l’aide du package EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="17ac7-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="17ac7-137">Dans l’Explorateur de solutions, cliquez sur le nom du projet.</span><span class="sxs-lookup"><span data-stu-id="17ac7-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="17ac7-138">Sélectionnez **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="17ac7-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="17ac7-139">Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package.</span><span class="sxs-lookup"><span data-stu-id="17ac7-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="17ac7-140">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="17ac7-140">Click **Install**</span></span>

<span data-ttu-id="17ac7-141">Notez que l’assembly EntityFramework, en plus des références aux assemblys System.ComponentModel.DataAnnotations et System.Data.Entity sont également ajoutés.</span><span class="sxs-lookup"><span data-stu-id="17ac7-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="17ac7-142">En haut du fichier Program.cs, ajoutez le code suivant à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="17ac7-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="17ac7-143">Dans Program.cs, ajoutez la définition du contexte.</span><span class="sxs-lookup"><span data-stu-id="17ac7-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="17ac7-144">Conserver et récupérer des données</span><span class="sxs-lookup"><span data-stu-id="17ac7-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="17ac7-145">Ouvrez le fichier Program.cs dans lequel la méthode Main est définie.</span><span class="sxs-lookup"><span data-stu-id="17ac7-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="17ac7-146">Ajoutez le code suivant dans la fonction Main.</span><span class="sxs-lookup"><span data-stu-id="17ac7-146">Add the following code into the Main function.</span></span> <span data-ttu-id="17ac7-147">Le code ajoute un nouvel objet de service pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="17ac7-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="17ac7-148">Ensuite, il enregistre les données.</span><span class="sxs-lookup"><span data-stu-id="17ac7-148">It then saves the data.</span></span> <span data-ttu-id="17ac7-149">Le code exécute également une requête LINQ qui retourne un département où le nom est DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="17ac7-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="17ac7-150">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="17ac7-150">Compile and run the application.</span></span> <span data-ttu-id="17ac7-151">Le programme génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="17ac7-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="17ac7-152">Afficher la base de données généré</span><span class="sxs-lookup"><span data-stu-id="17ac7-152">View the Generated Database</span></span>

<span data-ttu-id="17ac7-153">Lorsque vous exécutez l’application la première fois, Entity Framework crée une base de données pour vous.</span><span class="sxs-lookup"><span data-stu-id="17ac7-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="17ac7-154">Étant donné que nous Visual Studio 2012 est installé, la base de données est créé sur l’instance de base de données locale.</span><span class="sxs-lookup"><span data-stu-id="17ac7-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="17ac7-155">Par défaut, Entity Framework nomme la base de données après le nom qualifié complet de contexte dérivé (pour cet exemple est **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="17ac7-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="17ac7-156">Les fois suivantes, que la base de données existante sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="17ac7-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="17ac7-157">Notez que si vous apportez des modifications à votre modèle une fois que la base de données a été créé, vous devez utiliser des Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="17ac7-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="17ac7-158">Consultez [Code First pour une base de données](~/ef6/modeling/code-first/workflows/new-database.md) pour obtenir un exemple d’utilisation de Migrations.</span><span class="sxs-lookup"><span data-stu-id="17ac7-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="17ac7-159">Pour afficher la base de données et les données, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="17ac7-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="17ac7-160">Dans le menu principal de Visual Studio 2012, sélectionnez **vue**  - &gt; **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="17ac7-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="17ac7-161">Si la base de données locale n’est pas dans la liste des serveurs, cliquez sur le bouton droit de la souris sur **SQL Server** et sélectionnez **ajouter SQL Server** utiliser la valeur par défaut **l’authentification Windows** pour se connecter à la Instance de base de données locale</span><span class="sxs-lookup"><span data-stu-id="17ac7-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="17ac7-162">Développez le nœud de base de données locale</span><span class="sxs-lookup"><span data-stu-id="17ac7-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="17ac7-163">Dérouler la **bases de données** dossier pour voir la nouvelle base de données et accédez à la **département** Note de Code First ne crée pas une table qui mappe au type énumération de table</span><span class="sxs-lookup"><span data-stu-id="17ac7-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="17ac7-164">Pour afficher les données, avec le bouton droit sur la table et sélectionnez **afficher les données**</span><span class="sxs-lookup"><span data-stu-id="17ac7-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="17ac7-165">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="17ac7-165">Summary</span></span>

<span data-ttu-id="17ac7-166">Dans cette procédure pas à pas, nous avons vu comment utiliser les types enum avec Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="17ac7-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
