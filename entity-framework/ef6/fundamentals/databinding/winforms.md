---
title: Liaison de liaison avec WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419602"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="5542c-102">Liaison de liaison avec WinForms</span><span class="sxs-lookup"><span data-stu-id="5542c-102">Databinding with WinForms</span></span>
<span data-ttu-id="5542c-103">Cette procédure pas à pas montre comment lier des types POCO à des contrôles Windows Forms (WinForms) dans un formulaire maître/détail.</span><span class="sxs-lookup"><span data-stu-id="5542c-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="5542c-104">L’application utilise Entity Framework pour remplir les objets avec les données de la base de données, effectuer le suivi des modifications et conserver les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="5542c-105">Le modèle définit deux types qui participent à une relation un-à-plusieurs : la catégorie (principal\\maître) et le produit (détails\\dépendants).</span><span class="sxs-lookup"><span data-stu-id="5542c-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="5542c-106">Les outils Visual Studio sont ensuite utilisés pour lier les types définis dans le modèle aux contrôles WinForms.</span><span class="sxs-lookup"><span data-stu-id="5542c-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="5542c-107">L’infrastructure de liaison de données WinForms active la navigation entre les objets connexes : la sélection de lignes dans le mode maître entraîne la mise à jour de la vue détaillée avec les données enfants correspondantes.</span><span class="sxs-lookup"><span data-stu-id="5542c-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="5542c-108">Les captures d’écran et les listes de codes de cette procédure pas à pas sont extraites de Visual Studio 2013 mais vous pouvez effectuer cette procédure pas à pas avec Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5542c-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5542c-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="5542c-109">Pre-Requisites</span></span>

<span data-ttu-id="5542c-110">Pour effectuer cette procédure pas à pas, vous devez installer Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5542c-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="5542c-111">Si vous utilisez Visual Studio 2010, vous devez également installer NuGet.</span><span class="sxs-lookup"><span data-stu-id="5542c-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="5542c-112">Pour plus d’informations, consultez [installation de NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="5542c-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="5542c-113">Création de l’application</span><span class="sxs-lookup"><span data-stu-id="5542c-113">Create the Application</span></span>

-   <span data-ttu-id="5542c-114">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5542c-114">Open Visual Studio</span></span>
-   <span data-ttu-id="5542c-115">**Fichier&gt; nouveau&gt;...**</span><span class="sxs-lookup"><span data-stu-id="5542c-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="5542c-116">Sélectionnez **fenêtres** dans le volet gauche et **Windows FormsApplication** dans le volet droit</span><span class="sxs-lookup"><span data-stu-id="5542c-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="5542c-117">Entrez **WinFormswithEFSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="5542c-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="5542c-118">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="5542c-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="5542c-119">Installer le package NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5542c-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="5542c-120">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="5542c-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="5542c-121">Sélectionnez **gérer les packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="5542c-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="5542c-122">Dans la boîte de dialogue gérer les packages NuGet, sélectionnez l’onglet **en ligne** et choisissez le package **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="5542c-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="5542c-123">Cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="5542c-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="5542c-124">En plus de l’assembly EntityFramework, une référence à System. ComponentModel. DataAnnotations est également ajoutée.</span><span class="sxs-lookup"><span data-stu-id="5542c-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="5542c-125">Si le projet a une référence à System. Data. Entity, il sera supprimé lors de l’installation du package EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="5542c-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="5542c-126">L’assembly System. Data. Entity n’est plus utilisé pour les applications Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5542c-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="5542c-127">Implémentation de IListSource pour les collections</span><span class="sxs-lookup"><span data-stu-id="5542c-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="5542c-128">Les propriétés de collection doivent implémenter l’interface IListSource pour activer la liaison de données bidirectionnelle avec le tri lors de l’utilisation de Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="5542c-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="5542c-129">Pour ce faire, nous allons étendre ObservableCollection pour ajouter la fonctionnalité IListSource.</span><span class="sxs-lookup"><span data-stu-id="5542c-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="5542c-130">Ajoutez une classe **ObservableListSource** au projet :</span><span class="sxs-lookup"><span data-stu-id="5542c-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="5542c-131">Cliquez avec le bouton droit sur le nom du projet</span><span class="sxs-lookup"><span data-stu-id="5542c-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="5542c-132">Sélectionnez **Ajouter-&gt; nouvel élément**</span><span class="sxs-lookup"><span data-stu-id="5542c-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="5542c-133">Sélectionnez **classe** et entrez **ObservableListSource** pour le nom de la classe</span><span class="sxs-lookup"><span data-stu-id="5542c-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="5542c-134">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5542c-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="5542c-135">*Cette classe active la liaison de données bidirectionnelle et le tri. La classe dérive de ObservableCollection&lt;T&gt; et ajoute une implémentation explicite de IListSource. La méthode GetList () de IListSource est implémentée pour retourner une implémentation de IBindingList qui reste synchronisée avec ObservableCollection. L’implémentation IBindingList générée par ToBindingList prend en charge le tri. La méthode d’extension ToBindingList est définie dans l’assembly EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="5542c-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="5542c-136">Définir un modèle</span><span class="sxs-lookup"><span data-stu-id="5542c-136">Define a Model</span></span>

<span data-ttu-id="5542c-137">Dans cette procédure pas à pas, vous pouvez choisir d’implémenter un modèle à l’aide de Code First ou du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="5542c-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="5542c-138">Suivez l’une des deux sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="5542c-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="5542c-139">Option 1 : définir un modèle à l’aide de Code First</span><span class="sxs-lookup"><span data-stu-id="5542c-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="5542c-140">Cette section montre comment créer un modèle et la base de données qui lui est associée à l’aide de Code First.</span><span class="sxs-lookup"><span data-stu-id="5542c-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="5542c-141">Passez à la section suivante (**option 2 : définir un modèle à l’aide de Database First)** si vous préférez utiliser Database First pour rétroconcevoir votre modèle à partir d’une base de données à l’aide du concepteur EF</span><span class="sxs-lookup"><span data-stu-id="5542c-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="5542c-142">Lors de l’utilisation de Code First développement, vous commencez généralement par écrire des classes .NET Framework qui définissent votre modèle conceptuel (domaine).</span><span class="sxs-lookup"><span data-stu-id="5542c-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="5542c-143">Ajouter une nouvelle classe de **produit** au projet</span><span class="sxs-lookup"><span data-stu-id="5542c-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="5542c-144">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5542c-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="5542c-145">Ajoutez une classe **Category** au projet.</span><span class="sxs-lookup"><span data-stu-id="5542c-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="5542c-146">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5542c-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="5542c-147">En plus de définir des entités, vous devez définir une classe qui dérive de **DbContext** et expose les propriétés **DbSet&lt;TEntity&gt;** .</span><span class="sxs-lookup"><span data-stu-id="5542c-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="5542c-148">Les propriétés **DbSet** permettent au contexte de savoir quels types vous souhaitez inclure dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="5542c-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="5542c-149">Les types **DbContext** et **DbSet** sont définis dans l’assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="5542c-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="5542c-150">Une instance du type dérivé DbContext gère les objets d’entité au moment de l’exécution, ce qui comprend le remplissage des objets avec les données d’une base de données, le suivi des modifications et la persistance des données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="5542c-151">Ajoutez une nouvelle classe **ProductContext** au projet.</span><span class="sxs-lookup"><span data-stu-id="5542c-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="5542c-152">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5542c-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="5542c-153">Compilez le projet.</span><span class="sxs-lookup"><span data-stu-id="5542c-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="5542c-154">Option 2 : définir un modèle à l’aide de Database First</span><span class="sxs-lookup"><span data-stu-id="5542c-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="5542c-155">Cette section montre comment utiliser Database First pour rétroconcevoir votre modèle à partir d’une base de données à l’aide du concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="5542c-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="5542c-156">Si vous avez terminé la section précédente (**option 1 : définir un modèle à l’aide de code First)** , ignorez cette section et passez directement à la section **chargement différé** .</span><span class="sxs-lookup"><span data-stu-id="5542c-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="5542c-157">Créer une base de données existante</span><span class="sxs-lookup"><span data-stu-id="5542c-157">Create an Existing Database</span></span>

<span data-ttu-id="5542c-158">En général, lorsque vous ciblez une base de données existante, elle est déjà créée, mais pour cette procédure pas à pas, nous devons créer une base de données à laquelle accéder.</span><span class="sxs-lookup"><span data-stu-id="5542c-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="5542c-159">Le serveur de base de données installé avec Visual Studio diffère selon la version de Visual Studio que vous avez installée :</span><span class="sxs-lookup"><span data-stu-id="5542c-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5542c-160">Si vous utilisez Visual Studio 2010, vous allez créer une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="5542c-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="5542c-161">Si vous utilisez Visual Studio 2012, vous allez créer une base de [données de base](https://msdn.microsoft.com/library/hh510202.aspx) de données locale.</span><span class="sxs-lookup"><span data-stu-id="5542c-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="5542c-162">Commençons par générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="5542c-163">**Vue-&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="5542c-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="5542c-164">Cliquez avec le bouton droit sur **connexions de données-&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="5542c-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="5542c-165">Si vous n’êtes pas connecté à une base de données à partir de Explorateur de serveurs avant de devoir sélectionner Microsoft SQL Server comme source de données</span><span class="sxs-lookup"><span data-stu-id="5542c-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Changer la source de données](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="5542c-167">Connectez-vous à la base de données locale ou SQL Express, en fonction de celle que vous avez installée, puis entrez **Products** comme nom de la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Ajouter une base de données locale de connexion](~/ef6/media/addconnectionlocaldb.png)

    ![Ajouter une connexion Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="5542c-170">Sélectionnez **OK** . vous serez invité à créer une nouvelle base de données, sélectionnez **Oui** .</span><span class="sxs-lookup"><span data-stu-id="5542c-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Créer une base de données](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="5542c-172">La nouvelle base de données s’affiche alors dans Explorateur de serveurs, cliquez dessus avec le bouton droit et sélectionnez **nouvelle requête** .</span><span class="sxs-lookup"><span data-stu-id="5542c-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="5542c-173">Copiez le code SQL suivant dans la nouvelle requête, cliquez avec le bouton droit sur la requête et sélectionnez **exécuter** .</span><span class="sxs-lookup"><span data-stu-id="5542c-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="5542c-174">Modèle d’ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="5542c-174">Reverse Engineer Model</span></span>

<span data-ttu-id="5542c-175">Nous allons utiliser Entity Framework Designer, inclus dans le cadre de Visual Studio, pour créer notre modèle.</span><span class="sxs-lookup"><span data-stu-id="5542c-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="5542c-176">**Projet-&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="5542c-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="5542c-177">Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="5542c-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="5542c-178">Entrez **ProductModel** comme nom et cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="5542c-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="5542c-179">Cela lance l' **assistant Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="5542c-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="5542c-180">Sélectionnez **générer à partir de la base de données** , puis cliquez sur **suivant** .</span><span class="sxs-lookup"><span data-stu-id="5542c-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="5542c-182">Sélectionnez la connexion à la base de données que vous avez créée dans la première section, entrez **ProductContext** comme nom de la chaîne de connexion, puis cliquez sur **suivant** .</span><span class="sxs-lookup"><span data-stu-id="5542c-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Choisir votre connexion](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="5542c-184">Cochez la case en regard de « tables » pour importer toutes les tables, puis cliquez sur « Terminer ».</span><span class="sxs-lookup"><span data-stu-id="5542c-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Choisir vos objets](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="5542c-186">Une fois le processus d’ingénierie à rebours terminé, le nouveau modèle est ajouté à votre projet et vous est ouvert pour que vous l’affichez dans le Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5542c-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="5542c-187">Un fichier app. config a également été ajouté à votre projet avec les détails de connexion de la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="5542c-188">Étapes supplémentaires dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5542c-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="5542c-189">Si vous travaillez dans Visual Studio 2010, vous devrez mettre à jour le concepteur EF pour utiliser la génération de code EF6.</span><span class="sxs-lookup"><span data-stu-id="5542c-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="5542c-190">Cliquez avec le bouton droit sur une zone vide de votre modèle dans le concepteur EF, puis sélectionnez **Ajouter un élément de génération de code...**</span><span class="sxs-lookup"><span data-stu-id="5542c-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="5542c-191">Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**</span><span class="sxs-lookup"><span data-stu-id="5542c-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="5542c-192">Sélectionnez le **Générateur de DbContext EF 6. x pour C\#,** entrez **ProductsModel** comme nom et cliquez sur Ajouter.</span><span class="sxs-lookup"><span data-stu-id="5542c-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="5542c-193">Mise à jour de la génération de code pour la liaison de données</span><span class="sxs-lookup"><span data-stu-id="5542c-193">Updating code generation for data binding</span></span>

<span data-ttu-id="5542c-194">EF génère du code à partir de votre modèle à l’aide de modèles T4.</span><span class="sxs-lookup"><span data-stu-id="5542c-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="5542c-195">Les modèles fournis avec Visual Studio ou téléchargés à partir de la Galerie Visual Studio sont destinés à un usage général.</span><span class="sxs-lookup"><span data-stu-id="5542c-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="5542c-196">Cela signifie que les entités générées à partir de ces modèles ont des propriétés ICollection&lt;T&gt; simples.</span><span class="sxs-lookup"><span data-stu-id="5542c-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="5542c-197">Toutefois, lors de la liaison de données, il est souhaitable d’avoir des propriétés de collection qui implémentent IListSource.</span><span class="sxs-lookup"><span data-stu-id="5542c-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="5542c-198">C’est la raison pour laquelle nous avons créé la classe ObservableListSource ci-dessus et nous allons maintenant modifier les modèles pour utiliser cette classe.</span><span class="sxs-lookup"><span data-stu-id="5542c-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="5542c-199">Ouvrir le **Explorateur de solutions** et rechercher le fichier **ProductModel. edmx**</span><span class="sxs-lookup"><span data-stu-id="5542c-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="5542c-200">Rechercher le fichier **ProductModel.TT** qui sera imbriqué dans le fichier ProductModel. edmx</span><span class="sxs-lookup"><span data-stu-id="5542c-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modèle de modèle de produit](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="5542c-202">Double-cliquez sur le fichier ProductModel.tt pour l’ouvrir dans l’éditeur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5542c-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="5542c-203">Recherchez et remplacez les deux occurrences de «**ICollection**» par «**ObservableListSource**».</span><span class="sxs-lookup"><span data-stu-id="5542c-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="5542c-204">Celles-ci se trouvent sur des lignes d’environ 296 et 484.</span><span class="sxs-lookup"><span data-stu-id="5542c-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="5542c-205">Recherchez et remplacez la première occurrence de «**HashSet**» par «**ObservableListSource**».</span><span class="sxs-lookup"><span data-stu-id="5542c-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="5542c-206">Cette occurrence se trouve à environ la ligne 50.</span><span class="sxs-lookup"><span data-stu-id="5542c-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="5542c-207">**Ne remplacez pas** la deuxième occurrence de HashSet trouvée plus loin dans le code.</span><span class="sxs-lookup"><span data-stu-id="5542c-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="5542c-208">Enregistrez le fichier ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="5542c-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="5542c-209">Cela devrait entraîner la régénération du code pour les entités.</span><span class="sxs-lookup"><span data-stu-id="5542c-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="5542c-210">Si le code ne se régénère pas automatiquement, cliquez avec le bouton droit sur ProductModel.tt et choisissez « exécuter un outil personnalisé ».</span><span class="sxs-lookup"><span data-stu-id="5542c-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="5542c-211">Si vous ouvrez maintenant le fichier Category.cs (qui est imbriqué sous ProductModel.tt), vous devez voir que la collection Products a le type **ObservableListSource&lt;Product&gt;** .</span><span class="sxs-lookup"><span data-stu-id="5542c-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="5542c-212">Compilez le projet.</span><span class="sxs-lookup"><span data-stu-id="5542c-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="5542c-213">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="5542c-213">Lazy Loading</span></span>

<span data-ttu-id="5542c-214">La propriété **Products** de la classe **Category** et la propriété **Category** de la classe **Product** sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="5542c-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="5542c-215">Dans Entity Framework, les propriétés de navigation offrent un moyen de naviguer dans une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="5542c-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="5542c-216">EF vous donne la possibilité de charger automatiquement les entités associées à partir de la base de données la première fois que vous accédez à la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="5542c-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="5542c-217">Avec ce type de chargement (appelé chargement différé), sachez que la première fois que vous accédez à chaque propriété de navigation, une requête distincte est exécutée sur la base de données si le contenu n’est pas déjà dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="5542c-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="5542c-218">Lorsque vous utilisez des types d’entités POCO, EF réalise un chargement différé en créant des instances de types de proxy dérivés pendant l’exécution, puis en substituant les propriétés virtuelles dans vos classes pour ajouter le raccordement de chargement.</span><span class="sxs-lookup"><span data-stu-id="5542c-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="5542c-219">Pour bénéficier du chargement différé d’objets connexes, vous devez déclarer les accesseurs get de propriété de navigation comme **public** et **virtuel** (**substituable** dans Visual Basic) et vous ne devez pas être **sealed** (**NotOverridable** dans Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="5542c-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="5542c-220">Lors de l’utilisation de Database First propriétés de navigation sont automatiquement configurées pour permettre le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="5542c-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="5542c-221">Dans la section Code First, nous avons choisi de rendre les propriétés de navigation virtuelles pour la même raison</span><span class="sxs-lookup"><span data-stu-id="5542c-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="5542c-222">Lier un objet à des contrôles</span><span class="sxs-lookup"><span data-stu-id="5542c-222">Bind Object to Controls</span></span>

<span data-ttu-id="5542c-223">Ajoutez les classes définies dans le modèle en tant que sources de données pour cette application WinForms.</span><span class="sxs-lookup"><span data-stu-id="5542c-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="5542c-224">Dans le menu principal, sélectionnez **projet-&gt; ajouter une nouvelle source de données...**</span><span class="sxs-lookup"><span data-stu-id="5542c-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="5542c-225">(dans Visual Studio 2010, vous devez sélectionner **données-&gt; ajouter une nouvelle source de données...** )</span><span class="sxs-lookup"><span data-stu-id="5542c-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="5542c-226">Dans la fenêtre choisir un type de source de données, sélectionnez **objet** , puis cliquez sur **suivant** .</span><span class="sxs-lookup"><span data-stu-id="5542c-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="5542c-227">Dans la boîte de dialogue Sélectionner les objets de données, dérouler les **WinFormswithEFSample** deux fois et sélectionner une **catégorie** il n’est pas nécessaire de sélectionner la source de données du produit, car nous y accéderons via la propriété du produit sur la source de données de catégorie.</span><span class="sxs-lookup"><span data-stu-id="5542c-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![source de données](~/ef6/media/datasource.png)

-   <span data-ttu-id="5542c-229">Cliquez sur **Terminer.**</span><span class="sxs-lookup"><span data-stu-id="5542c-229">Click **Finish.**</span></span>
    <span data-ttu-id="5542c-230">Si la fenêtre sources de données ne s’affiche pas, sélectionnez **Afficher-&gt; autres sources de données Windows-&gt;**</span><span class="sxs-lookup"><span data-stu-id="5542c-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="5542c-231">Appuyez sur l’icône d’épingle pour que la fenêtre sources de données ne soit pas masquée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="5542c-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="5542c-232">Vous devrez peut-être cliquer sur le bouton Actualiser si la fenêtre était déjà visible.</span><span class="sxs-lookup"><span data-stu-id="5542c-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Source de données 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="5542c-234">Dans Explorateur de solutions, double-cliquez sur le fichier **Form1.cs** pour ouvrir le formulaire principal dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="5542c-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="5542c-235">Sélectionnez la source de données de **catégorie** et faites-la glisser sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="5542c-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="5542c-236">Par défaut, un nouveau DataGridView (**categoryDataGridView**) et des contrôles de barre d’outils de navigation sont ajoutés au concepteur.</span><span class="sxs-lookup"><span data-stu-id="5542c-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="5542c-237">Ces contrôles sont liés aux composants BindingSource (**categoryBindingSource**) et du navigateur de liaison (**categoryBindingNavigator**) qui sont également créés.</span><span class="sxs-lookup"><span data-stu-id="5542c-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="5542c-238">Modifiez les colonnes sur le **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="5542c-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="5542c-239">Nous souhaitons définir la colonne **CategoryID** en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="5542c-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="5542c-240">La valeur de la propriété **CategoryID** est générée par la base de données après l’enregistrement des données.</span><span class="sxs-lookup"><span data-stu-id="5542c-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="5542c-241">Cliquez avec le bouton droit sur le contrôle DataGridView et sélectionnez Modifier les colonnes...</span><span class="sxs-lookup"><span data-stu-id="5542c-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="5542c-242">Sélectionnez la colonne CategoryId et affectez la valeur true à ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="5542c-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="5542c-243">Appuyez sur OK</span><span class="sxs-lookup"><span data-stu-id="5542c-243">Press OK</span></span>
-   <span data-ttu-id="5542c-244">Sélectionnez produits dans la source de données catégorie et faites-le glisser sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="5542c-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="5542c-245">Les productDataGridView et productBindingSource sont ajoutés au formulaire.</span><span class="sxs-lookup"><span data-stu-id="5542c-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="5542c-246">Modifiez les colonnes sur le productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="5542c-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="5542c-247">Nous souhaitons masquer les colonnes CategoryId et Category et définir ProductId en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="5542c-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="5542c-248">La valeur de la propriété ProductId est générée par la base de données après l’enregistrement des données.</span><span class="sxs-lookup"><span data-stu-id="5542c-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="5542c-249">Cliquez avec le bouton droit sur le contrôle DataGridView et sélectionnez **modifier les colonnes...** .</span><span class="sxs-lookup"><span data-stu-id="5542c-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="5542c-250">Sélectionnez la colonne **ProductID** et affectez la valeur **true**à **ReadOnly** .</span><span class="sxs-lookup"><span data-stu-id="5542c-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="5542c-251">Sélectionnez la colonne **CategoryID** et appuyez sur le bouton **supprimer** .</span><span class="sxs-lookup"><span data-stu-id="5542c-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="5542c-252">Faites de même avec la colonne **Category** .</span><span class="sxs-lookup"><span data-stu-id="5542c-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="5542c-253">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="5542c-253">Press **OK**.</span></span>

    <span data-ttu-id="5542c-254">Jusqu’à présent, nous avons associé nos contrôles DataGridView à des composants BindingSource dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="5542c-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="5542c-255">Dans la section suivante, nous allons ajouter du code au code-behind pour définir categoryBindingSource. DataSource sur la collection d’entités actuellement suivies par DbContext.</span><span class="sxs-lookup"><span data-stu-id="5542c-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="5542c-256">Lorsque nous avons fait glisser les produits de la catégorie sous la catégorie, le WinForms s’est engagé à configurer la propriété productsBindingSource. DataSource sur categoryBindingSource et la propriété productsBindingSource. DataMember sur Products.</span><span class="sxs-lookup"><span data-stu-id="5542c-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="5542c-257">En raison de cette liaison, seuls les produits appartenant à la catégorie actuellement sélectionnée seront affichés dans le productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="5542c-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="5542c-258">Activez le bouton **Enregistrer** dans la barre d’outils de navigation en cliquant sur le bouton droit de la souris et en sélectionnant **activé**.</span><span class="sxs-lookup"><span data-stu-id="5542c-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Concepteur de formulaire 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="5542c-260">Ajoutez le gestionnaire d’événements pour le bouton enregistrer en double-cliquant sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="5542c-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="5542c-261">Cette opération ajoute le gestionnaire d’événements et vous permet d’afficher le code-behind du formulaire.</span><span class="sxs-lookup"><span data-stu-id="5542c-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="5542c-262">Le code du gestionnaire d’événements **categoryBindingNavigatorSaveItem\_Click** est ajouté dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="5542c-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="5542c-263">Ajouter le code qui gère l’interaction des données</span><span class="sxs-lookup"><span data-stu-id="5542c-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="5542c-264">Nous allons maintenant ajouter le code permettant d’utiliser le ProductContext pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="5542c-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="5542c-265">Mettez à jour le code de la fenêtre principale du formulaire comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5542c-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="5542c-266">Le code déclare une instance de longue durée de ProductContext.</span><span class="sxs-lookup"><span data-stu-id="5542c-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="5542c-267">L’objet ProductContext est utilisé pour interroger et enregistrer des données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5542c-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="5542c-268">La méthode Dispose () sur l’instance ProductContext est ensuite appelée à partir de la méthode OnClosing substituée.</span><span class="sxs-lookup"><span data-stu-id="5542c-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="5542c-269">Les commentaires de code fournissent des détails sur ce que fait le code.</span><span class="sxs-lookup"><span data-stu-id="5542c-269">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="5542c-270">Tester l’application Windows Forms</span><span class="sxs-lookup"><span data-stu-id="5542c-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="5542c-271">Compilez et exécutez l’application, et vous pouvez tester la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="5542c-271">Compile and run the application and you can test out the functionality.</span></span>

    ![Formulaire 1 avant l’enregistrement](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="5542c-273">Une fois les clés générées par le magasin enregistrées, elles s’affichent à l’écran.</span><span class="sxs-lookup"><span data-stu-id="5542c-273">After saving the store generated keys are shown on the screen.</span></span>

    ![Formulaire 1 après enregistrement](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="5542c-275">Si vous avez utilisé Code First, vous verrez également qu’une base de données **WinFormswithEFSample. ProductContext** est créée pour vous.</span><span class="sxs-lookup"><span data-stu-id="5542c-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Explorateur d’objets serveur](~/ef6/media/serverobjexplorer.png)
