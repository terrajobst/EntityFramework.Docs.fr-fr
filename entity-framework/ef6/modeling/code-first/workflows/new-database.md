---
title: Code First à une nouvelle base de données-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182570"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="765c0-102">Code First à une nouvelle base de données</span><span class="sxs-lookup"><span data-stu-id="765c0-102">Code First to a New Database</span></span>
<span data-ttu-id="765c0-103">Cette vidéo et la procédure pas à pas suivante fournissent une introduction au développement Code First ciblant une base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="765c0-104">Ce scénario inclut une base de données cible qui n’existe pas et que Code First va créer, ou une base de données vide où Code First ajoutera des tables.</span><span class="sxs-lookup"><span data-stu-id="765c0-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="765c0-105">Code First va tout d’abord vous permettre de définir votre modèle à l’aide de classes C\# ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="765c0-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="765c0-106">Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et propriétés ou à l’aide d’une API Fluent.</span><span class="sxs-lookup"><span data-stu-id="765c0-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="765c0-107">Regarder la vidéo</span><span class="sxs-lookup"><span data-stu-id="765c0-107">Watch the video</span></span>
<span data-ttu-id="765c0-108">Cette vidéo fournit une introduction au développement Code First ciblant une base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="765c0-109">Ce scénario inclut une base de données cible qui n’existe pas et que Code First va créer, ou une base de données vide où Code First ajoutera des tables.</span><span class="sxs-lookup"><span data-stu-id="765c0-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="765c0-110">Code First va tout d’abord vous permettre de définir votre modèle à l’aide de classes C\# ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="765c0-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="765c0-111">Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et propriétés ou à l’aide d’une API Fluent.</span><span class="sxs-lookup"><span data-stu-id="765c0-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="765c0-112">**Présentée par** : [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="765c0-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="765c0-113">**Vidéo**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (zip)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="765c0-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="765c0-114">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="765c0-114">Pre-Requisites</span></span>

<span data-ttu-id="765c0-115">Vous devez avoir au moins Visual Studio 2010 ou Visual Studio 2012 installé pour effectuer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="765c0-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="765c0-116">Si vous utilisez Visual Studio 2010, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) doit également être installé.</span><span class="sxs-lookup"><span data-stu-id="765c0-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="765c0-117">1. créer l’application</span><span class="sxs-lookup"><span data-stu-id="765c0-117">1. Create the Application</span></span>

<span data-ttu-id="765c0-118">Pour simplifier les choses, nous allons créer une application console de base qui utilise Code First pour effectuer l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="765c0-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="765c0-119">Ouvrez Visual Studio</span><span class="sxs-lookup"><span data-stu-id="765c0-119">Open Visual Studio</span></span>
-   <span data-ttu-id="765c0-120">**Fichier&gt; nouveau&gt;...**</span><span class="sxs-lookup"><span data-stu-id="765c0-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="765c0-121">Sélectionnez **Windows** dans le menu de gauche et dans l' **application console** .</span><span class="sxs-lookup"><span data-stu-id="765c0-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="765c0-122">Entrez **CodeFirstNewDatabaseSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="765c0-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="765c0-123">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="765c0-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="765c0-124">2. créer le modèle</span><span class="sxs-lookup"><span data-stu-id="765c0-124">2. Create the Model</span></span>

<span data-ttu-id="765c0-125">Nous allons définir un modèle très simple à l’aide de classes.</span><span class="sxs-lookup"><span data-stu-id="765c0-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="765c0-126">Nous allons simplement les définir dans le fichier Program.cs, mais dans une application réelle, vous fractionnez vos classes en fichiers distincts et éventuellement dans un projet distinct.</span><span class="sxs-lookup"><span data-stu-id="765c0-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="765c0-127">Sous la définition de classe de programme dans Program.cs, ajoutez les deux classes suivantes.</span><span class="sxs-lookup"><span data-stu-id="765c0-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="765c0-128">Vous remarquerez que les deux propriétés de navigation (blog. publications et post. blog) sont virtuelles.</span><span class="sxs-lookup"><span data-stu-id="765c0-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="765c0-129">Cela active la fonctionnalité de chargement différé de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="765c0-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="765c0-130">Le chargement différé signifie que le contenu de ces propriétés est chargé automatiquement à partir de la base de données lorsque vous tentez d’y accéder.</span><span class="sxs-lookup"><span data-stu-id="765c0-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="765c0-131">3. créer un contexte</span><span class="sxs-lookup"><span data-stu-id="765c0-131">3. Create a Context</span></span>

<span data-ttu-id="765c0-132">À présent, il est temps de définir un contexte dérivé, qui représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="765c0-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="765c0-133">Nous définissons un contexte qui dérive de System. Data. Entity. DbContext et expose un DbSet de type&lt;tente de&gt; typé pour chaque classe dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="765c0-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="765c0-134">Nous commençons à utiliser les types de la Entity Framework, donc nous devons ajouter le package NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="765c0-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="765c0-135">**Projet-&gt; gérer les packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="765c0-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="765c0-136">Remarque : Si vous n’avez pas la **gestion des packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="765c0-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="765c0-137">option vous devez installer la [dernière version de NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="765c0-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="765c0-138">Sélectionner l’onglet **en ligne**</span><span class="sxs-lookup"><span data-stu-id="765c0-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="765c0-139">Sélectionner le package **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="765c0-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="765c0-140">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="765c0-140">Click **Install**</span></span>

<span data-ttu-id="765c0-141">Ajoutez une instruction using pour System. Data. Entity en haut de Program.cs.</span><span class="sxs-lookup"><span data-stu-id="765c0-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="765c0-142">Sous la classe de publication dans Program.cs, ajoutez le contexte dérivé suivant.</span><span class="sxs-lookup"><span data-stu-id="765c0-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="765c0-143">Voici une liste complète de ce que Program.cs doit maintenant contenir.</span><span class="sxs-lookup"><span data-stu-id="765c0-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="765c0-144">C’est tout le code dont nous avons besoin pour commencer à stocker et à récupérer les données.</span><span class="sxs-lookup"><span data-stu-id="765c0-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="765c0-145">Évidemment, il y a beaucoup de choses en coulisses et nous examinerons cela dans un moment, mais commençons par le voir en action.</span><span class="sxs-lookup"><span data-stu-id="765c0-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="765c0-146">4. lecture & écriture de données</span><span class="sxs-lookup"><span data-stu-id="765c0-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="765c0-147">Implémentez la méthode main dans Program.cs comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="765c0-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="765c0-148">Ce code crée une nouvelle instance de notre contexte, puis l’utilise pour insérer un nouveau blog.</span><span class="sxs-lookup"><span data-stu-id="765c0-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="765c0-149">Il utilise ensuite une requête LINQ pour récupérer tous les blogs de la base de données classée par ordre alphabétique par titre.</span><span class="sxs-lookup"><span data-stu-id="765c0-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="765c0-150">Vous pouvez maintenant exécuter l’application et la tester.</span><span class="sxs-lookup"><span data-stu-id="765c0-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="765c0-151">Où sont mes données ?</span><span class="sxs-lookup"><span data-stu-id="765c0-151">Where’s My Data?</span></span>

<span data-ttu-id="765c0-152">Par la Convention DbContext a créé une base de données pour vous.</span><span class="sxs-lookup"><span data-stu-id="765c0-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="765c0-153">Si une instance locale de SQL Express est disponible (installée par défaut avec Visual Studio 2010), Code First a créé la base de données sur cette instance.</span><span class="sxs-lookup"><span data-stu-id="765c0-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="765c0-154">Si SQL Express n’est pas disponible, Code First essaiera et utilisera la [base de données locale (installée](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) par défaut avec Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="765c0-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="765c0-155">La base de données est nommée après le nom qualifié complet du contexte dérivé, dans notre cas **CodeFirstNewDatabaseSample. BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="765c0-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="765c0-156">Il s’agit uniquement des conventions par défaut et il existe différentes façons de modifier la base de données que Code First utilise, plus d’informations sont disponibles dans la rubrique **How DbContext Découvre le modèle et la connexion à la base de données** .</span><span class="sxs-lookup"><span data-stu-id="765c0-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="765c0-157">Vous pouvez vous connecter à cette base de données à l’aide de Explorateur de serveurs dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="765c0-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="765c0-158">**Vue-&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="765c0-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="765c0-159">Cliquez avec le bouton droit sur **connexions de données** , puis sélectionnez **Ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="765c0-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="765c0-160">Si vous n’êtes pas connecté à une base de données à partir de Explorateur de serveurs avant de devoir sélectionner Microsoft SQL Server comme source de données</span><span class="sxs-lookup"><span data-stu-id="765c0-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Sélectionner une source de données](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="765c0-162">Connectez-vous à la base de données locale ou SQL Express, en fonction de celle que vous avez installée</span><span class="sxs-lookup"><span data-stu-id="765c0-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="765c0-163">Nous pouvons maintenant inspecter le schéma créé par Code First.</span><span class="sxs-lookup"><span data-stu-id="765c0-163">We can now inspect the schema that Code First created.</span></span>

![Schéma initial](~/ef6/media/schemainitial.png)

<span data-ttu-id="765c0-165">DbContext a utilisé les classes à inclure dans le modèle en examinant les propriétés DbSet que nous avons définies.</span><span class="sxs-lookup"><span data-stu-id="765c0-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="765c0-166">Il utilise ensuite l’ensemble de conventions d’Code First par défaut pour déterminer les noms de tables et de colonnes, déterminer les types de données, rechercher les clés primaires, etc.</span><span class="sxs-lookup"><span data-stu-id="765c0-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="765c0-167">Plus loin dans cette procédure pas à pas, nous allons voir comment vous pouvez remplacer ces conventions.</span><span class="sxs-lookup"><span data-stu-id="765c0-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="765c0-168">5. traitement des modifications de modèle</span><span class="sxs-lookup"><span data-stu-id="765c0-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="765c0-169">À présent, il est temps d’apporter des modifications à notre modèle, lorsque nous effectuons ces modifications, nous devons également mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="765c0-170">Pour ce faire, nous allons utiliser une fonctionnalité appelée Migrations Code First, ou migrations pour Short.</span><span class="sxs-lookup"><span data-stu-id="765c0-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="765c0-171">Les migrations nous permettent d’avoir un ensemble ordonné d’étapes qui décrivent comment mettre à niveau (et rétrograder) notre schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="765c0-172">Chacune de ces étapes, appelée migration, contient du code qui décrit les modifications à appliquer.</span><span class="sxs-lookup"><span data-stu-id="765c0-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="765c0-173">La première étape consiste à activer Migrations Code First pour notre BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="765c0-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="765c0-174">**Outils-gestionnaire de package de la bibliothèque&gt;-&gt; console du gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="765c0-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="765c0-175">Exécutez la commande **Enable-Migrations** dans la Console du Gestionnaire de Package</span><span class="sxs-lookup"><span data-stu-id="765c0-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="765c0-176">Un nouveau dossier migrations a été ajouté à notre projet qui contient deux éléments :</span><span class="sxs-lookup"><span data-stu-id="765c0-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="765c0-177">**Configuration.cs** : ce fichier contient les paramètres que les migrations vont utiliser pour la migration de BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="765c0-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="765c0-178">Nous n’avons pas besoin de modifier quoi que ce soit pour cette procédure pas à pas, mais ici, vous pouvez spécifier des données de départ, inscrire des fournisseurs pour d’autres bases de données, modifier l’espace de noms dans lequel les migrations sont générées, etc.</span><span class="sxs-lookup"><span data-stu-id="765c0-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="765c0-179">**&lt;timestamp&gt;\_InitialCreate.cs** – il s’agit de votre première migration, il représente les modifications qui ont déjà été appliquées à la base de données pour la faire passer d’une base de données vide à une base de données qui comprend les blogs et les tables des publications.</span><span class="sxs-lookup"><span data-stu-id="765c0-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="765c0-180">Bien que nous puissions laisser Code First créer automatiquement ces tables pour nous, maintenant que nous avons choisi des migrations, elles ont été converties en migration.</span><span class="sxs-lookup"><span data-stu-id="765c0-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="765c0-181">Code First a également enregistré dans notre base de données locale que cette migration a déjà été appliquée.</span><span class="sxs-lookup"><span data-stu-id="765c0-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="765c0-182">L’horodateur sur le nom de fichier est utilisé à des fins de classement.</span><span class="sxs-lookup"><span data-stu-id="765c0-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="765c0-183">Nous allons maintenant apporter une modification à notre modèle, ajouter une propriété URL à la classe de blog :</span><span class="sxs-lookup"><span data-stu-id="765c0-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="765c0-184">Exécutez la commande **Add-migration AddUrl** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="765c0-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="765c0-185">La commande Add-migration vérifie les modifications apportées depuis votre dernière migration et génère une nouvelle migration avec les modifications détectées.</span><span class="sxs-lookup"><span data-stu-id="765c0-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="765c0-186">Nous pouvons attribuer un nom à la migration. dans ce cas, nous appelons la migration « AddUrl ».</span><span class="sxs-lookup"><span data-stu-id="765c0-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="765c0-187">Le code de génération de modèles automatique indique que nous devons ajouter une colonne d’URL, qui peut contenir des données de chaîne, au dbo. Table blogs.</span><span class="sxs-lookup"><span data-stu-id="765c0-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="765c0-188">Si nécessaire, nous pourrions modifier le code de génération de modèles automatique, mais cela n’est pas nécessaire dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="765c0-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="765c0-189">Exécutez la commande **Update-Database** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="765c0-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="765c0-190">Cette commande applique toutes les migrations en attente à la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="765c0-191">Notre migration InitialCreate a déjà été appliquée afin que les migrations appliquent simplement notre nouvelle migration AddUrl.</span><span class="sxs-lookup"><span data-stu-id="765c0-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="765c0-192">Conseil : vous pouvez utiliser le commutateur **– Verbose** lors de l’appel de Update-Database pour voir le SQL en cours d’exécution sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="765c0-193">La nouvelle colonne URL est maintenant ajoutée à la table blogs dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="765c0-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schéma avec URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="765c0-195">6. Annotations de données</span><span class="sxs-lookup"><span data-stu-id="765c0-195">6. Data Annotations</span></span>

<span data-ttu-id="765c0-196">Jusqu’à présent, nous permettons à EF de découvrir le modèle à l’aide de ses conventions par défaut, mais il y a des moments où nos classes ne suivent pas les conventions et nous devons être en mesure d’effectuer une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="765c0-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="765c0-197">Il existe deux options pour cela ; Nous allons examiner les annotations de données dans cette section, puis l’API Fluent dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="765c0-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="765c0-198">Nous allons ajouter une classe d’utilisateur à notre modèle</span><span class="sxs-lookup"><span data-stu-id="765c0-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="765c0-199">Nous devons également ajouter un ensemble à notre contexte dérivé</span><span class="sxs-lookup"><span data-stu-id="765c0-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="765c0-200">Si nous avons essayé d’ajouter une migration, nous obtenons une erreur indiquant «*EntityType’User’n’a aucune clé définie. Définissez la clé pour cet EntityType.»*</span><span class="sxs-lookup"><span data-stu-id="765c0-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="765c0-201">comme EF n’a aucun moyen de savoir que le nom d’utilisateur doit être la clé primaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="765c0-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="765c0-202">Nous allons utiliser des annotations de données maintenant afin d’ajouter une instruction using en haut de Program.cs</span><span class="sxs-lookup"><span data-stu-id="765c0-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="765c0-203">Annotez maintenant la propriété UserName pour identifier qu’il s’agit de la clé primaire</span><span class="sxs-lookup"><span data-stu-id="765c0-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="765c0-204">Utilisez la commande **Add-migration adduser** pour générer automatiquement la structure d’une migration et appliquer ces modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="765c0-205">Exécutez la commande **Update-Database** pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="765c0-206">La nouvelle table est maintenant ajoutée à la base de données :</span><span class="sxs-lookup"><span data-stu-id="765c0-206">The new table is now added to the database:</span></span>

![Schéma avec des utilisateurs](~/ef6/media/schemawithusers.png)

<span data-ttu-id="765c0-208">La liste complète des annotations prises en charge par EF est la suivante :</span><span class="sxs-lookup"><span data-stu-id="765c0-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="765c0-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="765c0-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="765c0-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="765c0-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="765c0-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="765c0-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="765c0-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="765c0-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="765c0-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="765c0-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="765c0-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="765c0-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="765c0-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="765c0-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="765c0-222">7. API Fluent</span><span class="sxs-lookup"><span data-stu-id="765c0-222">7. Fluent API</span></span>

<span data-ttu-id="765c0-223">Dans la section précédente, nous avons vu comment utiliser des annotations de données pour compléter ou remplacer ce qui a été détecté par Convention.</span><span class="sxs-lookup"><span data-stu-id="765c0-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="765c0-224">L’autre façon de configurer le modèle consiste à utiliser l’API Fluent Code First.</span><span class="sxs-lookup"><span data-stu-id="765c0-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="765c0-225">La plupart des configurations de modèle peuvent être effectuées à l’aide d’annotations de données simples.</span><span class="sxs-lookup"><span data-stu-id="765c0-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="765c0-226">L’API Fluent est un moyen plus avancé de spécifier une configuration de modèle qui couvre tout ce que les annotations de données peuvent faire en plus d’une configuration plus avancée qui n’est pas possible avec les annotations de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="765c0-227">Les annotations de données et l’API Fluent peuvent être utilisées ensemble.</span><span class="sxs-lookup"><span data-stu-id="765c0-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="765c0-228">Pour accéder à l’API Fluent, vous devez substituer la méthode OnModelCreating dans DbContext.</span><span class="sxs-lookup"><span data-stu-id="765c0-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="765c0-229">Supposons que nous voulions renommer la colonne dans laquelle User. DisplayName est stocké pour afficher\_nom.</span><span class="sxs-lookup"><span data-stu-id="765c0-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="765c0-230">Remplacez la méthode OnModelCreating sur BloggingContext par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="765c0-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="765c0-231">Utilisez la commande **Add-migration ChangeDisplayName** pour générer automatiquement une migration pour appliquer ces modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="765c0-232">Exécutez la commande **Update-Database** pour appliquer la nouvelle migration à la base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="765c0-233">La colonne DisplayName est maintenant renommée pour afficher\_nom :</span><span class="sxs-lookup"><span data-stu-id="765c0-233">The DisplayName column is now renamed to display\_name:</span></span>

![Schéma dont le nom d’affichage a été renommé](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="765c0-235">Résumé</span><span class="sxs-lookup"><span data-stu-id="765c0-235">Summary</span></span>

<span data-ttu-id="765c0-236">Dans cette procédure pas à pas, nous avons examiné Code First développement à l’aide d’une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="765c0-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="765c0-237">Nous avons défini un modèle à l’aide de classes, puis j’ai utilisé ce modèle pour créer une base de données et stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="765c0-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="765c0-238">Une fois la base de données créée, nous avons utilisé Migrations Code First pour modifier le schéma à mesure de l’évolution du modèle.</span><span class="sxs-lookup"><span data-stu-id="765c0-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="765c0-239">Nous avons également vu comment configurer un modèle à l’aide d’annotations de données et de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="765c0-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
