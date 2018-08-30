---
title: Code First pour une base de données - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 50c6a4710bc50879304f64e781a46c4836f86882
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152476"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="bb9dc-102">Code First pour une base de données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-102">Code First to a New Database</span></span>
<span data-ttu-id="bb9dc-103">Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Code First ciblant une base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="bb9dc-104">Ce scénario inclut une base de données qui n’existe pas de ciblage et Code First crée, ou une base de données vide ce Code First l’ajoutera des tables à.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="bb9dc-105">Code tout d’abord vous permet de définir votre modèle à l’aide de C\# ou les classes VB.Net.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="bb9dc-106">Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="bb9dc-107">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="bb9dc-107">Watch the video</span></span>
<span data-ttu-id="bb9dc-108">Cette vidéo fournit une introduction au développement Code First ciblant une base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="bb9dc-109">Ce scénario inclut une base de données qui n’existe pas de ciblage et Code First crée, ou une base de données vide ce Code First l’ajoutera des tables à.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="bb9dc-110">Code tout d’abord vous permet de définir votre modèle à l’aide de classes c# ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="bb9dc-111">Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="bb9dc-112">**Présentée par** : [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="bb9dc-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="bb9dc-113">**Vidéo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="bb9dc-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="bb9dc-114">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="bb9dc-114">Pre-Requisites</span></span>

<span data-ttu-id="bb9dc-115">Vous devez avoir au moins Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="bb9dc-116">Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="bb9dc-117">1. Création de l’application</span><span class="sxs-lookup"><span data-stu-id="bb9dc-117">1. Create the Application</span></span>

<span data-ttu-id="bb9dc-118">Pour simplifier les choses, nous allons créer une application de console de base qui utilise Code First pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="bb9dc-119">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb9dc-119">Open Visual Studio</span></span>
-   <span data-ttu-id="bb9dc-120">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="bb9dc-121">Sélectionnez **Windows** dans le menu de gauche et **Application Console**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="bb9dc-122">Entrez **CodeFirstNewDatabaseSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="bb9dc-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="bb9dc-123">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="bb9dc-124">2. Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="bb9dc-124">2. Create the Model</span></span>

<span data-ttu-id="bb9dc-125">Nous allons définir un modèle très simple à l’aide de classes.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="bb9dc-126">Nous allons simplement les définir dans le fichier Program.cs, mais dans une application réelle que vous serez diviser vos classes out en fichiers distincts et, potentiellement, un projet distinct.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="bb9dc-127">Sous la définition de classe de programme dans le fichier Program.cs, ajoutez les deux classes suivantes.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="bb9dc-128">Vous remarquerez que nous apportons les deux propriétés de navigation (Blog.Posts et Post.Blog) virtuel.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="bb9dc-129">Ainsi, la fonctionnalité de chargement différé d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="bb9dc-130">Le chargement différé signifie que le contenu de ces propriétés est automatiquement chargé à partir de la base de données lorsque vous essayez d’y accéder.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="bb9dc-131">3. Créer un contexte</span><span class="sxs-lookup"><span data-stu-id="bb9dc-131">3. Create a Context</span></span>

<span data-ttu-id="bb9dc-132">Il est maintenant temps de définir un contexte dérivé qui représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="bb9dc-133">Nous définissons un contexte qui dérive de System.Data.Entity.DbContext et expose un DbSet typé&lt;TEntity&gt; pour chaque classe dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="bb9dc-134">Maintenant, nous commençons à utiliser des types à partir d’Entity Framework afin de nous devons ajouter le package EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="bb9dc-135">**Projet –&gt; gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="bb9dc-136">Remarque : Si vous n’avez pas le **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="bb9dc-137">option, vous devez installer le [version la plus récente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="bb9dc-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="bb9dc-138">Sélectionnez le **Online** onglet</span><span class="sxs-lookup"><span data-stu-id="bb9dc-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="bb9dc-139">Sélectionnez le **EntityFramework** package</span><span class="sxs-lookup"><span data-stu-id="bb9dc-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="bb9dc-140">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-140">Click **Install**</span></span>

<span data-ttu-id="bb9dc-141">Ajoutez une instruction pour System.Data.Entity en haut du fichier Program.cs.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="bb9dc-142">Ci-dessous la classe Post dans le fichier Program.cs, ajoutez le contexte dérivé suivant.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="bb9dc-143">Voici une liste complète de ce que Program.cs doit maintenant contenir.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="bb9dc-144">C’est tout le code que nous devons commencer stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="bb9dc-145">Évidemment, il existe un certain passe dans les coulisses et nous étudierons un que dans un moment, mais premier nous allons voir en action.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="bb9dc-146">4. Lire et écrire des données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="bb9dc-147">Implémentez la méthode Main dans Program.cs, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="bb9dc-148">Ce code crée une nouvelle instance de notre contexte et il utilise ensuite pour insérer un nouveau Blog.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="bb9dc-149">Il utilise ensuite une requête LINQ pour récupérer tous les Blogs à partir de la base de données classé par ordre alphabétique par titre.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="bb9dc-150">Vous pouvez maintenant exécuter l’application et le tester.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="bb9dc-151">Où sont mes données ?</span><span class="sxs-lookup"><span data-stu-id="bb9dc-151">Where’s My Data?</span></span>

<span data-ttu-id="bb9dc-152">Par convention DbContext a créé une base de données pour vous.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="bb9dc-153">Si une instance locale de SQL Express est disponible (installé par défaut avec Visual Studio 2010) puis Code First a créé la base de données sur cette instance</span><span class="sxs-lookup"><span data-stu-id="bb9dc-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="bb9dc-154">Si SQL Express n’est pas disponible, Code First sera tester et d’utiliser [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installé par défaut avec Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="bb9dc-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="bb9dc-155">La base de données est nommée après le nom qualifié complet de contexte dérivé, dans notre cas est **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="bb9dc-156">Il s’agit simplement les conventions par défaut et il existe différentes manières de modifier la base de données qui utilise Code First, informations supplémentaires sont disponibles dans le **comment DbContext détecte le modèle et la connexion de base de données** rubrique.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="bb9dc-157">Vous pouvez vous connecter à cette base de données à l’aide de l’Explorateur de serveurs dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb9dc-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="bb9dc-158">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="bb9dc-159">Cliquez avec le bouton droit sur **des connexions de données** et sélectionnez **ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="bb9dc-160">Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="bb9dc-162">Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé</span><span class="sxs-lookup"><span data-stu-id="bb9dc-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="bb9dc-163">Nous pouvons maintenant inspecter le schéma que Code First créé.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-163">We can now inspect the schema that Code First created.</span></span>

![SchemaInitial](~/ef6/media/schemainitial.png)

<span data-ttu-id="bb9dc-165">DbContext élaboré quelles classes à inclure dans le modèle en examinant les propriétés DbSet que nous avons défini.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="bb9dc-166">Il utilise ensuite l’ensemble de conventions Code First par défaut pour déterminer les noms de table et de colonne, de déterminer les types de données, de trouver les clés primaires, etc.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="bb9dc-167">Plus loin dans cette procédure pas à pas, nous allons examiner comment vous pouvez remplacer ces conventions.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="bb9dc-168">5. Gestion des modifications du modèle</span><span class="sxs-lookup"><span data-stu-id="bb9dc-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="bb9dc-169">Il est maintenant temps d’apporter des modifications à notre modèle, lorsque nous apporter ces modifications, que nous devons également mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="bb9dc-170">Pour ce faire, nous allons utiliser une fonctionnalité appelée Migrations Code First, ou des Migrations pour faire plus court.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="bb9dc-171">Migrations nous permet d’avoir un ensemble ordonné d’étapes qui décrivent comment mettre à niveau (et rétrograder) notre schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="bb9dc-172">Chacune de ces étapes, appelés une migration, contient du code qui décrit les modifications à appliquer.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="bb9dc-173">La première étape consiste à activer les Migrations Code First pour notre BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="bb9dc-174">**Outils -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**</span><span class="sxs-lookup"><span data-stu-id="bb9dc-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="bb9dc-175">Exécutez la commande **Enable-Migrations** dans la Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="bb9dc-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="bb9dc-176">Un nouveau dossier Migrations a été ajouté à notre projet qui contient deux éléments :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="bb9dc-177">**Configuration.cs** – ce fichier contient les paramètres de Migrations utilisera pour la migration BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="bb9dc-178">Nous n’avez pas besoin de modifier quoi que ce soit pour cette procédure pas à pas, mais voici où vous pouvez spécifier les données de valeur initiale, inscrire des fournisseurs pour les autres bases de données, modifie l’espace de noms que les migrations sont générées, etc.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="bb9dc-179">**&lt;horodatage&gt;\_InitialCreate.cs** – il s’agit votre première migration, il représente les modifications qui ont déjà été appliquées à la base de données en ne soient pas d’une base de données vide à celle qui inclut les tables de Blogs et des publications .</span><span class="sxs-lookup"><span data-stu-id="bb9dc-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="bb9dc-180">Bien que nous permettent de Code First automatiquement créer ces tables pour nous, maintenant que nous avons choisi de Migrations qu’ils ont été converties en une Migration.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="bb9dc-181">Code tout d’abord a également enregistrée dans notre base de données locale que cette Migration a déjà été appliquée.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="bb9dc-182">L’horodatage sur le nom de fichier est utilisé pour le classement à des fins.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="bb9dc-183">Maintenant nous allons apporter une modification à notre modèle, ajoutez une propriété d’Url à la classe de Blog :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="bb9dc-184">Exécutez le **Add-Migration AddUrl** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="bb9dc-185">La commande Add-Migration vérifie les modifications apportées depuis votre dernière migration et la structure d’une nouvelle migration avec toutes les modifications qui se trouvent.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="bb9dc-186">Nous pouvons nommez migrations ; Dans ce cas, nous appelons la « AddUrl » de migration.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="bb9dc-187">Le code généré automatiquement est à dire que nous devons ajouter une colonne de l’Url, qui peut contenir des données de chaîne, à dbo. Table de blogs.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="bb9dc-188">Si nécessaire, nous pouvons modifier le code généré automatiquement, mais qui n’est pas obligatoire dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="bb9dc-189">Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="bb9dc-190">Cette commande s’applique les migrations en attente à la base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="bb9dc-191">Notre migration InitialCreate a déjà été appliquée afin de migrations seront applique seulement notre nouvelle migration AddUrl.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="bb9dc-192">Conseil : Vous pouvez utiliser la **– Verbose** commutateur lors de l’appel de mise à jour la base de données pour afficher le code SQL en cours d’exécution par rapport à la base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="bb9dc-193">La nouvelle colonne de l’Url est maintenant ajoutée à la table de Blogs dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-193">The new Url column is now added to the Blogs table in the database:</span></span>

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="bb9dc-195">6. Annotations de données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-195">6. Data Annotations</span></span>

<span data-ttu-id="bb9dc-196">Jusqu'à présent, nous avons simplement laisser EF découvrir le modèle à l’aide de ses conventions par défaut, mais il doit y être parfois nos classes ne suivent pas les conventions et nous devons être en mesure d’effectuer une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="bb9dc-197">Il existe deux options pour cela ; Nous allons examiner les Annotations de données dans cette section, puis sur l’API fluent dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="bb9dc-198">Nous allons ajouter une classe d’utilisateur à notre modèle</span><span class="sxs-lookup"><span data-stu-id="bb9dc-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="bb9dc-199">Nous devons également ajouter un jeu à notre contexte dérivé</span><span class="sxs-lookup"><span data-stu-id="bb9dc-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="bb9dc-200">Si nous avons essayé d’ajouter une migration vous obtenez un message d’erreur indiquant «*EntityType « User » ne possède aucune clé définie. Définissez la clé pour cet EntityType. »*</span><span class="sxs-lookup"><span data-stu-id="bb9dc-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="bb9dc-201">Étant donné que EF n’a aucun moyen de savoir que nom d’utilisateur doit être la clé primaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="bb9dc-202">Nous allons utiliser des Annotations de données maintenant nous devons ajouter une à l’aide de l’instruction en haut du fichier Program.cs</span><span class="sxs-lookup"><span data-stu-id="bb9dc-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="bb9dc-203">Annoter désormais la propriété de nom d’utilisateur pour identifier la clé primaire</span><span class="sxs-lookup"><span data-stu-id="bb9dc-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="bb9dc-204">Utilisez le **Add-Migration AddUser** commande pour générer automatiquement une migration pour appliquer ces modifications à la base de données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="bb9dc-205">Exécutez le **Update-Database** commande pour appliquer la nouvelle migration à la base de données</span><span class="sxs-lookup"><span data-stu-id="bb9dc-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="bb9dc-206">La nouvelle table est maintenant ajoutée à la base de données :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-206">The new table is now added to the database:</span></span>

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

<span data-ttu-id="bb9dc-208">La liste complète des annotations prises en charge par Entity Framework est :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="bb9dc-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="bb9dc-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="bb9dc-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="bb9dc-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="bb9dc-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="bb9dc-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="bb9dc-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="bb9dc-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="bb9dc-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="bb9dc-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="bb9dc-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="bb9dc-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="bb9dc-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="bb9dc-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="bb9dc-222">7. API Fluent</span><span class="sxs-lookup"><span data-stu-id="bb9dc-222">7. Fluent API</span></span>

<span data-ttu-id="bb9dc-223">Dans la section précédente, nous avons vu à l’aide des Annotations de données pour compléter ou remplacer ce qui a été détecté par convention.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="bb9dc-224">L’autre méthode permettant de configurer le modèle est via l’API fluent Code First.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="bb9dc-225">La plupart des configuration de modèle peut être effectuée à l’aide des annotations de données simple.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="bb9dc-226">L’API fluent est un moyen plus avancé de spécifier une configuration de modèle qui couvre toutes les annotations de données peuvent faire de plus pour une configuration plus avancée n’est pas possible avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="bb9dc-227">Annotations de données et l’API fluent peuvent être utilisés ensemble.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="bb9dc-228">Pour accéder à l’API fluent vous substituez la méthode OnModelCreating de DbContext.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="bb9dc-229">Supposons que nous voulions renommer la colonne User.DisplayName est stocké dans pour afficher\_nom.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="bb9dc-230">Substituez la méthode OnModelCreating sur BloggingContext avec le code suivant</span><span class="sxs-lookup"><span data-stu-id="bb9dc-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="bb9dc-231">Utilisez le **Add-Migration ChangeDisplayName** commande pour générer automatiquement une migration pour appliquer ces modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="bb9dc-232">Exécutez le **Update-Database** commande pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="bb9dc-233">La colonne DisplayName a été renommée pour afficher\_nom :</span><span class="sxs-lookup"><span data-stu-id="bb9dc-233">The DisplayName column is now renamed to display\_name:</span></span>

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="bb9dc-235">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="bb9dc-235">Summary</span></span>

<span data-ttu-id="bb9dc-236">Dans cette procédure pas à pas, nous avons vu développement Code First à l’aide d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="bb9dc-237">Nous défini un modèle à l’aide de classes, puis ce modèle permet de créer une base de données et de stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="bb9dc-238">Une fois que la base de données a été créé, nous avons utilisé les Migrations Code First pour modifier le schéma en tant que notre modèle a évolué.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="bb9dc-239">Nous avons également vu comment configurer un modèle à l’aide des Annotations de données et l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="bb9dc-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
