---
title: EF6 First - de base de données
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
caps.latest.revision: 3
ms.openlocfilehash: 17bba5fe9883a1bee0f8b9624dfa35da889e6005
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120852"
---
# <a name="database-first"></a><span data-ttu-id="118c9-102">Tout d’abord la base de données</span><span class="sxs-lookup"><span data-stu-id="118c9-102">Database First</span></span>
<span data-ttu-id="118c9-103">Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement première base de données à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="118c9-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="118c9-104">Base de données tout d’abord vous permet rétroconcevoir un modèle à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="118c9-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="118c9-105">Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="118c9-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="118c9-106">Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="118c9-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="118c9-107">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="118c9-107">Watch the video</span></span>
<span data-ttu-id="118c9-108">Cette vidéo fournit une introduction au développement première base de données à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="118c9-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="118c9-109">Base de données tout d’abord vous permet rétroconcevoir un modèle à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="118c9-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="118c9-110">Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="118c9-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="118c9-111">Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="118c9-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="118c9-112">**Présentée par** : [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="118c9-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="118c9-113">**Vidéo**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="118c9-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="118c9-114">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="118c9-114">Pre-Requisites</span></span>

<span data-ttu-id="118c9-115">Vous devez avoir au moins Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="118c9-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="118c9-116">Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.</span><span class="sxs-lookup"><span data-stu-id="118c9-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="118c9-117">1. Créer une base de données existante</span><span class="sxs-lookup"><span data-stu-id="118c9-117">1. Create an Existing Database</span></span>

<span data-ttu-id="118c9-118">En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.</span><span class="sxs-lookup"><span data-stu-id="118c9-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="118c9-119">Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :</span><span class="sxs-lookup"><span data-stu-id="118c9-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="118c9-120">Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="118c9-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="118c9-121">Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="118c9-122">Procédons à générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="118c9-123">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="118c9-123">Open Visual Studio</span></span>
-   <span data-ttu-id="118c9-124">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="118c9-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="118c9-125">Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="118c9-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="118c9-126">Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données</span><span class="sxs-lookup"><span data-stu-id="118c9-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="118c9-128">Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé, puis entrez **DatabaseFirst.Blogging** en tant que le nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="118c9-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="118c9-131">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="118c9-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="118c9-133">La nouvelle base de données s’affiche maintenant dans l’Explorateur de serveurs, avec le bouton droit dessus et sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="118c9-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="118c9-134">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="118c9-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="118c9-135">2. Création de l’application</span><span class="sxs-lookup"><span data-stu-id="118c9-135">2. Create the Application</span></span>

<span data-ttu-id="118c9-136">Pour simplifier les choses, nous allons créer une application de console de base qui utilise la première base de données pour accéder aux données :</span><span class="sxs-lookup"><span data-stu-id="118c9-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="118c9-137">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="118c9-137">Open Visual Studio</span></span>
-   <span data-ttu-id="118c9-138">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="118c9-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="118c9-139">Sélectionnez **Windows** dans le menu de gauche et **Application Console**</span><span class="sxs-lookup"><span data-stu-id="118c9-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="118c9-140">Entrez **DatabaseFirstSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="118c9-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="118c9-141">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="118c9-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="118c9-142">3. Modèle d’ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="118c9-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="118c9-143">Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.</span><span class="sxs-lookup"><span data-stu-id="118c9-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="118c9-144">**Projet -&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="118c9-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="118c9-145">Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="118c9-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="118c9-146">Entrez **BloggingModel** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="118c9-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="118c9-147">Cette opération lance le **Assistant Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="118c9-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="118c9-148">Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="118c9-148">Select **Generate from Database** and click **Next**</span></span>

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="118c9-150">Sélectionnez la connexion à la base de données que vous avez créé dans la première section, entrez **BloggingContext** comme nom de la chaîne de connexion et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="118c9-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="118c9-152">Cliquez sur la case à cocher en regard de « Tables » pour importer toutes les tables, cliquez sur « Terminer »</span><span class="sxs-lookup"><span data-stu-id="118c9-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="118c9-154">Une fois que le processus d’ingénierie à rebours est terminé le nouveau modèle est ajouté à votre projet et ouvert pour l’afficher dans le Concepteur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="118c9-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="118c9-155">Un fichier App.config a également été ajouté à votre projet avec les détails de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="118c9-157">Étapes supplémentaires dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="118c9-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="118c9-158">Si vous travaillez dans Visual Studio 2010, il existe quelques étapes supplémentaires à suivre pour mettre à niveau vers la dernière version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="118c9-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="118c9-159">Il est important de la mise à niveau, car il vous donne accès à une surface d’API améliorée, qui est beaucoup plus facile à utiliser, ainsi que les derniers correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="118c9-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="118c9-160">Tout d’abord, nous devons obtenir la dernière version d’Entity Framework à partir de NuGet.</span><span class="sxs-lookup"><span data-stu-id="118c9-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="118c9-161">**Projet –&gt; gérer les Packages NuGet... ** 
     *Si vous n’avez pas le **gérer les Packages NuGet... ** option, vous devez installer le [version la plus récente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="118c9-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="118c9-162">Sélectionnez le **Online** onglet</span><span class="sxs-lookup"><span data-stu-id="118c9-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="118c9-163">Sélectionnez le **EntityFramework** package</span><span class="sxs-lookup"><span data-stu-id="118c9-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="118c9-164">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="118c9-164">Click **Install**</span></span>

<span data-ttu-id="118c9-165">Ensuite, nous devons échanger notre modèle pour générer du code qui utilise l’API DbContext, qui a été introduit dans les versions ultérieures d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="118c9-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="118c9-166">Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**</span><span class="sxs-lookup"><span data-stu-id="118c9-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="118c9-167">Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**</span><span class="sxs-lookup"><span data-stu-id="118c9-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="118c9-168">Sélectionnez le EF **5.x générateur DbContext pour C\#**, entrez **BloggingModel** comme nom et cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="118c9-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="118c9-170">4. Lire et écrire des données</span><span class="sxs-lookup"><span data-stu-id="118c9-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="118c9-171">Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données.</span><span class="sxs-lookup"><span data-stu-id="118c9-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="118c9-172">Les classes que nous allons utiliser pour accéder aux données sont générés automatiquement pour vous selon le fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="118c9-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="118c9-173">*Cette capture d’écran provient de Visual Studio 2012, si vous utilisez Visual Studio 2010 le BloggingModel.tt et les fichiers BloggingModel.Context.tt seront directement sous votre projet plutôt qu’imbriqué sous le fichier EDMX.*</span><span class="sxs-lookup"><span data-stu-id="118c9-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="118c9-175">Implémentez la méthode Main dans Program.cs, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="118c9-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="118c9-176">Ce code crée une nouvelle instance de notre contexte et il utilise ensuite pour insérer un nouveau Blog.</span><span class="sxs-lookup"><span data-stu-id="118c9-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="118c9-177">Il utilise ensuite une requête LINQ pour récupérer tous les Blogs à partir de la base de données classé par ordre alphabétique par titre.</span><span class="sxs-lookup"><span data-stu-id="118c9-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="118c9-178">Vous pouvez maintenant exécuter l’application et le tester.</span><span class="sxs-lookup"><span data-stu-id="118c9-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="118c9-179">5. Gestion des modifications de base de données</span><span class="sxs-lookup"><span data-stu-id="118c9-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="118c9-180">Il est maintenant temps d’apporter certaines modifications à notre schéma de base de données, lorsque nous apporter ces modifications, que nous devons également mettre à jour de notre modèle pour refléter ces modifications.</span><span class="sxs-lookup"><span data-stu-id="118c9-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="118c9-181">La première étape consiste à apporter des modifications au schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="118c9-182">Nous allons ajouter une table des utilisateurs au schéma.</span><span class="sxs-lookup"><span data-stu-id="118c9-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="118c9-183">Avec le bouton droit sur le **DatabaseFirst.Blogging** dans l’Explorateur de serveurs de base de données et sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="118c9-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="118c9-184">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="118c9-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="118c9-185">Maintenant que le schéma est mis à jour, il est temps pour mettre à jour le modèle avec ces modifications.</span><span class="sxs-lookup"><span data-stu-id="118c9-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="118c9-186">Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez « modèle de mise à jour à partir de la base de données... », cette action lance l’Assistant de mise à jour</span><span class="sxs-lookup"><span data-stu-id="118c9-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="118c9-187">Sous l’onglet Ajouter de la vérification de l’Assistant Mise à jour la zone en regard des Tables, cela indique que nous souhaitons ajouter les nouvelles tables à partir du schéma.</span><span class="sxs-lookup"><span data-stu-id="118c9-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="118c9-188">*L’onglet de l’actualisation affiche les tables existantes dans le modèle qui sera vérifié les modifications pendant la mise à jour. Les onglets de suppression affichent toutes les tables qui ont été supprimés à partir du schéma et seront également supprimées à partir du modèle dans le cadre de la mise à jour. Les informations sur ces deux onglets sont détectées automatiquement et sont fournies à titre d’information uniquement, vous ne pouvez pas modifier les paramètres.*</span><span class="sxs-lookup"><span data-stu-id="118c9-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="118c9-190">Cliquez sur Terminer dans l’Assistant de mise à jour</span><span class="sxs-lookup"><span data-stu-id="118c9-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="118c9-191">Le modèle est désormais mis à jour pour inclure une nouvelle entité d’utilisateur qui est mappé à la table d’utilisateurs que nous avons ajouté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="118c9-193">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="118c9-193">Summary</span></span>

<span data-ttu-id="118c9-194">Dans cette procédure pas à pas, nous avons étudié développement Database First, qui nous a permis de créer un modèle dans le Concepteur EF basé sur une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="118c9-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="118c9-195">Ensuite, nous avons utilisé ce modèle pour lire et écrire des données à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="118c9-196">Enfin, nous avons mis à jour le modèle pour refléter les modifications que nous avons apportées au schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="118c9-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
