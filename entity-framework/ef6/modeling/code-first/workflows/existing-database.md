---
title: Code First pour une base de données existante - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: fedfb921919582e2cdb5f3bc497f11889b972ad6
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251074"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="76f82-102">Code First pour une base de données existante</span><span class="sxs-lookup"><span data-stu-id="76f82-102">Code First to an Existing Database</span></span>
<span data-ttu-id="76f82-103">Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Code First ciblant une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="76f82-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="76f82-104">Code tout d’abord vous permet de définir votre modèle à l’aide de C\# ou les classes VB.Net.</span><span class="sxs-lookup"><span data-stu-id="76f82-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="76f82-105">Une configuration supplémentaire si vous le souhaitez peut être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.</span><span class="sxs-lookup"><span data-stu-id="76f82-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="76f82-106">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="76f82-106">Watch the video</span></span>
<span data-ttu-id="76f82-107">Cette vidéo est [désormais disponible sur Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="76f82-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="76f82-108">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="76f82-108">Pre-Requisites</span></span>

<span data-ttu-id="76f82-109">Vous devez avoir **Visual Studio 2012** ou **Visual Studio 2013** pour effectuer cette procédure.</span><span class="sxs-lookup"><span data-stu-id="76f82-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="76f82-110">Vous devez également version **6.1** (ou version ultérieure) de la **Entity Framework Tools pour Visual Studio** installé.</span><span class="sxs-lookup"><span data-stu-id="76f82-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="76f82-111">Consultez [obtenir Entity Framework](~/ef6/fundamentals/install.md) pour plus d’informations sur l’installation de la dernière version des outils Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="76f82-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="76f82-112">1. Créer une base de données existante</span><span class="sxs-lookup"><span data-stu-id="76f82-112">1. Create an Existing Database</span></span>

<span data-ttu-id="76f82-113">En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.</span><span class="sxs-lookup"><span data-stu-id="76f82-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="76f82-114">Procédons à générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="76f82-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="76f82-115">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76f82-115">Open Visual Studio</span></span>
-   <span data-ttu-id="76f82-116">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="76f82-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="76f82-117">Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="76f82-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="76f82-118">Si vous n’avez pas connecté à une base de données à partir de **Explorateur de serveurs** avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données</span><span class="sxs-lookup"><span data-stu-id="76f82-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Sélectionnez la Source de données](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="76f82-120">Connectez-vous à votre instance de base de données locale, puis entrez **blogs** en tant que le nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="76f82-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Connexion de base de données locale](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="76f82-122">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="76f82-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Base de données de boîte de dialogue Créer](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="76f82-124">La nouvelle base de données s’affiche maintenant dans l’Explorateur de serveurs, avec le bouton droit dessus et sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="76f82-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="76f82-125">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="76f82-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="76f82-126">2. Création de l’application</span><span class="sxs-lookup"><span data-stu-id="76f82-126">2. Create the Application</span></span>

<span data-ttu-id="76f82-127">Pour simplifier les choses, nous allons créer une application de console de base qui utilise Code First pour accéder aux données :</span><span class="sxs-lookup"><span data-stu-id="76f82-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="76f82-128">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76f82-128">Open Visual Studio</span></span>
-   <span data-ttu-id="76f82-129">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="76f82-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="76f82-130">Sélectionnez **Windows** dans le menu de gauche et **Application Console**</span><span class="sxs-lookup"><span data-stu-id="76f82-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="76f82-131">Entrez **CodeFirstExistingDatabaseSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="76f82-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="76f82-132">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="76f82-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="76f82-133">3. Modèle d’ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="76f82-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="76f82-134">Nous allons utiliser Entity Framework Tools pour Visual Studio pour nous aider à générer du code initial pour mapper à la base de données.</span><span class="sxs-lookup"><span data-stu-id="76f82-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="76f82-135">Ces outils sont simplement génération du code que vous pouvez également taper manuellement si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="76f82-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="76f82-136">**Projet -&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="76f82-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="76f82-137">Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="76f82-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="76f82-138">Entrez **BloggingContext** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="76f82-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="76f82-139">Cette opération lance le **Assistant Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="76f82-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="76f82-140">Sélectionnez **Code First à partir de la base de données** et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="76f82-140">Select **Code First from Database** and click **Next**</span></span>

    ![Un CFE Assistant](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="76f82-142">Sélectionnez la connexion à la base de données que vous avez créé dans la première section et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="76f82-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![CFE deux Assistant](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="76f82-144">Cliquez sur la case à cocher en regard **Tables** pour importer toutes les tables, cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="76f82-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![CFE trois Assistant](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="76f82-146">Une fois le processus d’ingénierie à rebours terminé un nombre d’éléments seront ajoutés au projet, nous allons examiner ce qui a été ajouté.</span><span class="sxs-lookup"><span data-stu-id="76f82-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="76f82-147">fichier de configuration</span><span class="sxs-lookup"><span data-stu-id="76f82-147">Configuration file</span></span>

<span data-ttu-id="76f82-148">Un fichier App.config a été ajouté au projet, ce fichier contient la chaîne de connexion à la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="76f82-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="76f82-149">*Vous remarquerez que trop d’autres paramètres dans le fichier de configuration, il s’agit des paramètres EF par défaut qui indiquent à Code First où créer des bases de données. Étant donné que nous réalisons l’association à une base de données existante, ces paramètres seront ignorés dans notre application.*</span><span class="sxs-lookup"><span data-stu-id="76f82-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="76f82-150">Contexte dérivé</span><span class="sxs-lookup"><span data-stu-id="76f82-150">Derived Context</span></span>

<span data-ttu-id="76f82-151">Un **BloggingContext** classe a été ajoutée au projet.</span><span class="sxs-lookup"><span data-stu-id="76f82-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="76f82-152">Le contexte représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="76f82-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="76f82-153">Le contexte expose un **DbSet&lt;TEntity&gt;**  pour chaque type dans notre modèle.</span><span class="sxs-lookup"><span data-stu-id="76f82-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="76f82-154">Vous remarquerez également que le constructeur par défaut qui appelle un constructeur de base à l’aide de la **nom =** syntaxe.</span><span class="sxs-lookup"><span data-stu-id="76f82-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="76f82-155">Ce code indique à Code First que la chaîne de connexion à utiliser pour ce contexte doit être chargée à partir du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="76f82-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="76f82-156">*Vous devez toujours utiliser le **nom =** syntaxe lorsque vous utilisez une chaîne de connexion dans le fichier de configuration. Cela garantit que si la chaîne de connexion n’est pas présente puis Entity Framework lève au lieu de créer une nouvelle base de données par convention.*</span><span class="sxs-lookup"><span data-stu-id="76f82-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="76f82-157">Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="76f82-157">Model classes</span></span>

<span data-ttu-id="76f82-158">Enfin, un **Blog** et **Post** classe ont également été ajoutés au projet.</span><span class="sxs-lookup"><span data-stu-id="76f82-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="76f82-159">Ce sont les classes de domaine qui composent notre modèle.</span><span class="sxs-lookup"><span data-stu-id="76f82-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="76f82-160">Vous verrez des Annotations de données appliqués aux classes pour spécifier la configuration où les conventions Code First seraient non s’aligner sur la structure de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="76f82-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="76f82-161">Par exemple, vous verrez la **StringLength** annotation sur **Blog.Name** et **Blog.Url** , car elles ont une longueur maximale de **200** dans le base de données (la valeur par défaut de Code First consiste à utiliser la longueur maximale prise en charge par le fournisseur de base de données - **nvarchar (max)** dans SQL Server).</span><span class="sxs-lookup"><span data-stu-id="76f82-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="76f82-162">4. Lire et écrire des données</span><span class="sxs-lookup"><span data-stu-id="76f82-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="76f82-163">Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données.</span><span class="sxs-lookup"><span data-stu-id="76f82-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="76f82-164">Implémentez le **Main** méthode dans **Program.cs** comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="76f82-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="76f82-165">Ce code crée une nouvelle instance de notre contexte et puis l’utilise pour insérer un nouveau **Blog**.</span><span class="sxs-lookup"><span data-stu-id="76f82-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="76f82-166">Elle utilise ensuite une requête LINQ pour récupérer tous les **Blogs** à partir de la base de données classé par ordre alphabétique par **titre**.</span><span class="sxs-lookup"><span data-stu-id="76f82-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="76f82-167">Vous pouvez maintenant exécuter l’application et le tester.</span><span class="sxs-lookup"><span data-stu-id="76f82-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="76f82-168">Que se passe-t-il si ma base de données change ?</span><span class="sxs-lookup"><span data-stu-id="76f82-168">What if My Database Changes?</span></span>

<span data-ttu-id="76f82-169">Code First à l’Assistant de base de données est conçu pour générer un ensemble de point de départ de classes que vous pouvez ensuite modifier et modifier.</span><span class="sxs-lookup"><span data-stu-id="76f82-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="76f82-170">Si votre schéma de base de données est modifié vous pouvez manuellement modifier les classes ou effectuer une autre ingénierie à rebours pour remplacer les classes.</span><span class="sxs-lookup"><span data-stu-id="76f82-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="76f82-171">À l’aide des Migrations Code First pour une base de données existante</span><span class="sxs-lookup"><span data-stu-id="76f82-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="76f82-172">Si vous souhaitez utiliser les Migrations Code First avec une base de données existante, consultez [Migrations Code First pour une base de données existante](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="76f82-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="76f82-173">Résumé</span><span class="sxs-lookup"><span data-stu-id="76f82-173">Summary</span></span>

<span data-ttu-id="76f82-174">Dans cette procédure pas à pas, nous avons vu développement Code First à l’aide de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="76f82-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="76f82-175">Nous avons utilisé les outils Entity Framework pour Visual Studio pour rétroconcevoir un ensemble de classes qui mappé à la base de données et peut être utilisé pour stocker et récupérer des données.</span><span class="sxs-lookup"><span data-stu-id="76f82-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
