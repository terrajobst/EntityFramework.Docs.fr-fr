---
title: Liaison de données avec WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384850"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="01ac4-102">Liaison de données avec WinForms</span><span class="sxs-lookup"><span data-stu-id="01ac4-102">Databinding with WinForms</span></span>
<span data-ttu-id="01ac4-103">Cette procédure pas à pas montre comment lier les types POCO aux contrôles Windows Forms (WinForms) dans un formulaire « maître / détail ».</span><span class="sxs-lookup"><span data-stu-id="01ac4-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="01ac4-104">L’application utilise Entity Framework pour remplir des objets avec des données à partir de la base de données, le suivi des modifications et conserver les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="01ac4-105">Le modèle définit deux types impliquées dans une relation un-à-plusieurs : catégorie (principal\\master) et Product (dépendants\\détail).</span><span class="sxs-lookup"><span data-stu-id="01ac4-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="01ac4-106">Ensuite, les outils de Visual Studio sont utilisés pour lier les types définis dans le modèle pour les contrôles WinForms.</span><span class="sxs-lookup"><span data-stu-id="01ac4-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="01ac4-107">L’infrastructure de liaison de données WinForms permet la navigation entre les objets connexes : sélection de lignes dans la vue principale, la vue de détail mettre à jour avec les données enfants correspondantes.</span><span class="sxs-lookup"><span data-stu-id="01ac4-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="01ac4-108">Les captures d’écran et les listings de code dans cette procédure pas à pas sont effectuées à partir de Visual Studio 2013, mais vous pouvez effectuer cette procédure pas à pas avec Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="01ac4-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="01ac4-109">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="01ac4-109">Pre-Requisites</span></span>

<span data-ttu-id="01ac4-110">Vous devez disposer de Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="01ac4-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="01ac4-111">Si vous utilisez Visual Studio 2010, vous devez également installer le package NuGet.</span><span class="sxs-lookup"><span data-stu-id="01ac4-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="01ac4-112">Pour plus d’informations, consultez [installation NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="01ac4-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="01ac4-113">Création de l’application</span><span class="sxs-lookup"><span data-stu-id="01ac4-113">Create the Application</span></span>

-   <span data-ttu-id="01ac4-114">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ac4-114">Open Visual Studio</span></span>
-   <span data-ttu-id="01ac4-115">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="01ac4-116">Sélectionnez **Windows** dans le volet gauche et **Windows FormsApplication** dans le volet droit</span><span class="sxs-lookup"><span data-stu-id="01ac4-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="01ac4-117">Entrez **WinFormswithEFSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="01ac4-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="01ac4-118">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="01ac4-119">Installez le package NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="01ac4-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="01ac4-120">Dans l’Explorateur de solutions, cliquez sur le **WinFormswithEFSample** projet</span><span class="sxs-lookup"><span data-stu-id="01ac4-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="01ac4-121">Sélectionnez **gérer les Packages NuGet...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="01ac4-122">Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package</span><span class="sxs-lookup"><span data-stu-id="01ac4-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="01ac4-123">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="01ac4-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="01ac4-124">En plus de l’assembly EntityFramework une référence à System.ComponentModel.DataAnnotations est également ajoutée.</span><span class="sxs-lookup"><span data-stu-id="01ac4-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="01ac4-125">Si le projet a une référence à System.Data.Entity, puis elle va être supprimée lorsque le package EntityFramework est installé.</span><span class="sxs-lookup"><span data-stu-id="01ac4-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="01ac4-126">L’assembly System.Data.Entity est n’est plus utilisé pour les applications Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="01ac4-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="01ac4-127">Implémentation IListSource pour les Collections</span><span class="sxs-lookup"><span data-stu-id="01ac4-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="01ac4-128">Propriétés de la collection doivent implémenter l’interface IListSource pour permettre la liaison de données bidirectionnelle avec tri lors de l’utilisation de Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="01ac4-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="01ac4-129">Pour ce faire, nous allons étendre ObservableCollection pour ajouter des fonctionnalités IListSource.</span><span class="sxs-lookup"><span data-stu-id="01ac4-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="01ac4-130">Ajouter un **ObservableListSource** classe au projet :</span><span class="sxs-lookup"><span data-stu-id="01ac4-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="01ac4-131">Avec le bouton droit sur le nom du projet</span><span class="sxs-lookup"><span data-stu-id="01ac4-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="01ac4-132">Sélectionnez **Add -&gt; un nouvel élément**</span><span class="sxs-lookup"><span data-stu-id="01ac4-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="01ac4-133">Sélectionnez **classe** et entrez **ObservableListSource** pour le nom de classe</span><span class="sxs-lookup"><span data-stu-id="01ac4-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="01ac4-134">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="01ac4-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="01ac4-135">*Cette classe permet de données bidirectionnelle de liaison ainsi que de tri. La classe dérive de ObservableCollection&lt;T&gt; et ajoute une implémentation explicite de IListSource. La méthode GetList() de IListSource est implémentée pour retourner une implémentation IBindingList qui reste synchronisée avec ObservableCollection. L’implémentation IBindingList générée par ToBindingList prend en charge le tri. La méthode d’extension ToBindingList est définie dans l’assembly EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="01ac4-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

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

## <a name="define-a-model"></a><span data-ttu-id="01ac4-136">Définir un modèle</span><span class="sxs-lookup"><span data-stu-id="01ac4-136">Define a Model</span></span>

<span data-ttu-id="01ac4-137">Dans cette procédure pas à pas, que vous pouvez choisi d’implémenter un modèle à l’aide de Code First ou le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="01ac4-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="01ac4-138">Effectuez l’une des deux sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="01ac4-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="01ac4-139">Option 1 : Définir un modèle à l’aide de Code First</span><span class="sxs-lookup"><span data-stu-id="01ac4-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="01ac4-140">Cette section montre comment créer un modèle et sa base de données associée à l’aide de Code First.</span><span class="sxs-lookup"><span data-stu-id="01ac4-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="01ac4-141">Passez à la section suivante (**Option 2 : définir un modèle à l’aide de la première base de données)** si vous préférez utiliser Database First pour inverser concevoir votre modèle à partir d’une base de données à l’aide du Concepteur EF</span><span class="sxs-lookup"><span data-stu-id="01ac4-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="01ac4-142">Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine).</span><span class="sxs-lookup"><span data-stu-id="01ac4-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="01ac4-143">Ajouter un nouveau **produit** classe au projet</span><span class="sxs-lookup"><span data-stu-id="01ac4-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="01ac4-144">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="01ac4-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="01ac4-145">Ajouter un **catégorie** classe au projet.</span><span class="sxs-lookup"><span data-stu-id="01ac4-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="01ac4-146">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="01ac4-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="01ac4-147">Outre la définition des entités, vous devez définir une classe qui dérive de **DbContext** et expose **DbSet&lt;TEntity&gt;**  propriétés.</span><span class="sxs-lookup"><span data-stu-id="01ac4-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="01ac4-148">Le **DbSet** propriétés permettent le contexte de connaître les types que vous souhaitez inclure dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="01ac4-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="01ac4-149">Le **DbContext** et **DbSet** types sont définis dans l’assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="01ac4-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="01ac4-150">Une instance du type DbContext dérivée gère les objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, modifier le suivi et la persistance des données à la base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="01ac4-151">Ajouter un nouveau **ProductContext** classe au projet.</span><span class="sxs-lookup"><span data-stu-id="01ac4-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="01ac4-152">Remplacez le code généré par défaut par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="01ac4-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="01ac4-153">Compilez le projet.</span><span class="sxs-lookup"><span data-stu-id="01ac4-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="01ac4-154">Option 2 : Définir un modèle à l’aide de la première base de données</span><span class="sxs-lookup"><span data-stu-id="01ac4-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="01ac4-155">Cette section montre comment utiliser la première base de données à l’ingénierie inverse de votre modèle à partir d’une base de données à l’aide du Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="01ac4-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="01ac4-156">Si vous avez terminé la section précédente (**Option 1 : définir un modèle à l’aide de Code First)**, puis ignorez cette section et passer directement à la **le chargement différé** section.</span><span class="sxs-lookup"><span data-stu-id="01ac4-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="01ac4-157">Créer une base de données existante</span><span class="sxs-lookup"><span data-stu-id="01ac4-157">Create an Existing Database</span></span>

<span data-ttu-id="01ac4-158">En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.</span><span class="sxs-lookup"><span data-stu-id="01ac4-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="01ac4-159">Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :</span><span class="sxs-lookup"><span data-stu-id="01ac4-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="01ac4-160">Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="01ac4-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="01ac4-161">Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="01ac4-162">Procédons à générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="01ac4-163">**Vue -&gt; Explorateur de serveurs**</span><span class="sxs-lookup"><span data-stu-id="01ac4-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="01ac4-164">Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="01ac4-165">Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données</span><span class="sxs-lookup"><span data-stu-id="01ac4-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Modifier la source de données](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="01ac4-167">Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé, puis entrez **produits** en tant que le nom de la base de données</span><span class="sxs-lookup"><span data-stu-id="01ac4-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Ajouter la connexion base de données locale](~/ef6/media/addconnectionlocaldb.png)

    ![Ajouter des connexions Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="01ac4-170">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="01ac4-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Créer une base de données](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="01ac4-172">La nouvelle base de données s’affiche maintenant dans l’Explorateur de serveurs, avec le bouton droit dessus et sélectionnez **nouvelle requête**</span><span class="sxs-lookup"><span data-stu-id="01ac4-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="01ac4-173">Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**</span><span class="sxs-lookup"><span data-stu-id="01ac4-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="01ac4-174">Modèle d’ingénierie à rebours</span><span class="sxs-lookup"><span data-stu-id="01ac4-174">Reverse Engineer Model</span></span>

<span data-ttu-id="01ac4-175">Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.</span><span class="sxs-lookup"><span data-stu-id="01ac4-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="01ac4-176">**Projet -&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="01ac4-177">Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="01ac4-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="01ac4-178">Entrez **ProductModel** comme nom et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="01ac4-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="01ac4-179">Cette opération lance le **Assistant Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="01ac4-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="01ac4-180">Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="01ac4-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="01ac4-182">Sélectionnez la connexion à la base de données que vous avez créé dans la première section, entrez **ProductContext** comme nom de la chaîne de connexion et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="01ac4-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Choisir votre connexion](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="01ac4-184">Cliquez sur la case à cocher en regard de « Tables » pour importer toutes les tables, cliquez sur « Terminer »</span><span class="sxs-lookup"><span data-stu-id="01ac4-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Choisir vos objets de](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="01ac4-186">Une fois que le processus d’ingénierie à rebours est terminé le nouveau modèle est ajouté à votre projet et ouvert pour l’afficher dans le Concepteur d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="01ac4-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="01ac4-187">Un fichier App.config a également été ajouté à votre projet avec les détails de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="01ac4-188">Étapes supplémentaires dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="01ac4-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="01ac4-189">Si vous travaillez dans Visual Studio 2010 vous devrez mettre à jour le Concepteur EF pour utiliser la génération de code EF6.</span><span class="sxs-lookup"><span data-stu-id="01ac4-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="01ac4-190">Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="01ac4-191">Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**</span><span class="sxs-lookup"><span data-stu-id="01ac4-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="01ac4-192">Sélectionnez le **EF 6.x générateur DbContext pour C\#,** entrez **ProductsModel** comme nom et cliquez sur Ajouter</span><span class="sxs-lookup"><span data-stu-id="01ac4-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="01ac4-193">Mise à jour de la génération de code pour la liaison de données</span><span class="sxs-lookup"><span data-stu-id="01ac4-193">Updating code generation for data binding</span></span>

<span data-ttu-id="01ac4-194">Entity Framework génère du code à partir de votre modèle à l’aide de modèles T4.</span><span class="sxs-lookup"><span data-stu-id="01ac4-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="01ac4-195">Les modèles fournis avec Visual Studio ou téléchargé à partir de la galerie Visual Studio sont destinés à usage général.</span><span class="sxs-lookup"><span data-stu-id="01ac4-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="01ac4-196">Cela signifie que les entités générées à partir de ces modèles ont ICollection simple&lt;T&gt; propriétés.</span><span class="sxs-lookup"><span data-stu-id="01ac4-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="01ac4-197">Toutefois, lors de la liaison de données, il est souhaitable d’avoir des propriétés de collection qui implémentent IListSource.</span><span class="sxs-lookup"><span data-stu-id="01ac4-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="01ac4-198">C’est pourquoi nous avons créé la classe ObservableListSource ci-dessus et nous allons maintenant modifier les modèles s’utiliser de cette classe.</span><span class="sxs-lookup"><span data-stu-id="01ac4-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="01ac4-199">Ouvrez le **l’Explorateur de solutions** et recherchez **ProductModel.edmx** fichier</span><span class="sxs-lookup"><span data-stu-id="01ac4-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="01ac4-200">Rechercher la **ProductModel.tt** fichier qui doit être imbriqué sous le fichier ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="01ac4-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modèle de produit](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="01ac4-202">Double-cliquez sur le fichier ProductModel.tt pour l’ouvrir dans l’éditeur Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ac4-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="01ac4-203">Rechercher et remplacer les deux occurrences de «**ICollection**« avec »**ObservableListSource**».</span><span class="sxs-lookup"><span data-stu-id="01ac4-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="01ac4-204">Ceux-ci sont situés dans des lignes environ 296 et 484.</span><span class="sxs-lookup"><span data-stu-id="01ac4-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="01ac4-205">Rechercher et remplacer la première occurrence de «**HashSet**« avec »**ObservableListSource**».</span><span class="sxs-lookup"><span data-stu-id="01ac4-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="01ac4-206">Cet événement se trouve environ à la ligne 50.</span><span class="sxs-lookup"><span data-stu-id="01ac4-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="01ac4-207">**Ne le faites pas** remplacer la deuxième occurrence de HashSet figure plus loin dans le code.</span><span class="sxs-lookup"><span data-stu-id="01ac4-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="01ac4-208">Enregistrez le fichier ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="01ac4-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="01ac4-209">Cela doit provoquer le code pour les entités d’être régénérée.</span><span class="sxs-lookup"><span data-stu-id="01ac4-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="01ac4-210">Si le code ne régénère pas automatiquement, puis avec le bouton droit sur ProductModel.tt et choisissez « Exécuter un outil personnalisé ».</span><span class="sxs-lookup"><span data-stu-id="01ac4-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="01ac4-211">Si vous ouvrez le fichier Category.cs (qui est imbriqué sous ProductModel.tt), vous devez voir que la collection de produits a le type **ObservableListSource&lt;produit&gt;**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="01ac4-212">Compilez le projet.</span><span class="sxs-lookup"><span data-stu-id="01ac4-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="01ac4-213">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="01ac4-213">Lazy Loading</span></span>

<span data-ttu-id="01ac4-214">Le **produits** propriété sur le **catégorie** classe et **catégorie** propriété sur le **produit** classe sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="01ac4-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="01ac4-215">Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="01ac4-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="01ac4-216">Entity Framework vous offre une option de chargement des entités connexes à partir de la base de données automatiquement la première fois que vous accédez à la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="01ac4-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="01ac4-217">Avec ce type de chargement (appelé chargement différé), n’oubliez pas que la première fois que vous accédez à chaque propriété de navigation une requête distincte sera exécutée la base de données si le contenu n’est pas déjà dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="01ac4-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="01ac4-218">Lorsque vous utilisez des types d’entités POCO, EF réalise le chargement différé par la création d’instances de types de proxy dérivée pendant l’exécution, puis en remplaçant les propriétés virtuelles dans vos classes pour ajouter le raccordement de chargement.</span><span class="sxs-lookup"><span data-stu-id="01ac4-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="01ac4-219">Pour obtenir le chargement différé d’objets connexes, vous devez déclarer les accesseurs Get de propriété en tant que navigation **public** et **virtuels** (**Overridable** en Visual Basic), et vous classe ne doit pas être **sealed** (**NotOverridable** en Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="01ac4-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="01ac4-220">Lors de la base de données à l’aide des propriétés de navigation premier sont automatiquement effectuées virtuelles pour activer le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="01ac4-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="01ac4-221">Dans la section de Code First que nous avons choisi créer les propriétés de navigation virtuelle pour la même raison</span><span class="sxs-lookup"><span data-stu-id="01ac4-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="01ac4-222">Lier des objets aux contrôles</span><span class="sxs-lookup"><span data-stu-id="01ac4-222">Bind Object to Controls</span></span>

<span data-ttu-id="01ac4-223">Ajoutez les classes qui sont définies dans le modèle en tant que sources de données pour cette application WinForms.</span><span class="sxs-lookup"><span data-stu-id="01ac4-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="01ac4-224">Dans le menu principal, sélectionnez **projet -&gt; ajouter une nouvelle Source de données...**</span><span class="sxs-lookup"><span data-stu-id="01ac4-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="01ac4-225">(dans Visual Studio 2010, vous devez sélectionner **données -&gt; ajouter nouvelle Source de données...** )</span><span class="sxs-lookup"><span data-stu-id="01ac4-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="01ac4-226">Dans la fenêtre Choisir un Type de Source de données, sélectionnez **objet** et cliquez sur **suivant**</span><span class="sxs-lookup"><span data-stu-id="01ac4-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="01ac4-227">Dans le, sélectionnez la boîte de dialogue des objets de données, dérouler les **WinFormswithEFSample** deux fois, puis sélectionnez **catégorie** il est inutile de sélectionner la source de données de produit, car nous allons lui par le biais du produit propriété sur la source de données de catégorie.</span><span class="sxs-lookup"><span data-stu-id="01ac4-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![source de données](~/ef6/media/datasource.png)

-   <span data-ttu-id="01ac4-229">Cliquez sur **terminer.** 
     *Si la fenêtre Sources de données ne s’affichent pas, sélectionnez \*\*\* vue -&gt; autres Windows -&gt; des Sources de données*\*</span><span class="sxs-lookup"><span data-stu-id="01ac4-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="01ac4-230">Appuyez sur l’icône d’épingle, afin de la fenêtre Sources de données ne sont pas automatique masquer.</span><span class="sxs-lookup"><span data-stu-id="01ac4-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="01ac4-231">Vous devrez peut-être appuyer sur le bouton de rafraîchissement si la fenêtre a été déjà visible.</span><span class="sxs-lookup"><span data-stu-id="01ac4-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Source de données 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="01ac4-233">Dans l’Explorateur de solutions, double-cliquez sur le **Form1.cs** fichier à ouvrir le formulaire principal dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="01ac4-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="01ac4-234">Sélectionnez le **catégorie** source de données et faites-la glisser sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="01ac4-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="01ac4-235">Par défaut, un nouveau DataGridView (**categoryDataGridView**) et contrôles de barre d’outils de Navigation sont ajoutées au concepteur.</span><span class="sxs-lookup"><span data-stu-id="01ac4-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="01ac4-236">Ces contrôles sont liés à la BindingSource (**categoryBindingSource**) et le navigateur de liaison (**categoryBindingNavigator**) les composants qui sont également créés.</span><span class="sxs-lookup"><span data-stu-id="01ac4-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="01ac4-237">Modifier les colonnes sur la **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="01ac4-238">Nous souhaitons définir la **CategoryId** colonne en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="01ac4-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="01ac4-239">La valeur de la **CategoryId** propriété est générée par la base de données une fois que nous enregistrons les données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="01ac4-240">Le contrôle DataGridView d’avec le bouton droit et sélectionnez Modifier les colonnes...</span><span class="sxs-lookup"><span data-stu-id="01ac4-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="01ac4-241">Sélectionnez la colonne CategoryId et en lecture seule la valeur True</span><span class="sxs-lookup"><span data-stu-id="01ac4-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="01ac4-242">Appuyez sur OK</span><span class="sxs-lookup"><span data-stu-id="01ac4-242">Press OK</span></span>
-   <span data-ttu-id="01ac4-243">Sélectionnez les produits figurant dans la source de données de catégorie et faites-le glisser sur le formulaire.</span><span class="sxs-lookup"><span data-stu-id="01ac4-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="01ac4-244">Le productDataGridView et productBindingSource sont ajoutés au formulaire.</span><span class="sxs-lookup"><span data-stu-id="01ac4-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="01ac4-245">Modifier les colonnes sur la productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="01ac4-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="01ac4-246">Nous souhaitons masquer les colonnes CategoryId et la catégorie et la valeur ProductId en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="01ac4-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="01ac4-247">La valeur de la propriété ProductId est générée par la base de données une fois que nous enregistrons les données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="01ac4-248">Cliquez sur le contrôle DataGridView et sélectionnez **modifier les colonnes...** .</span><span class="sxs-lookup"><span data-stu-id="01ac4-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="01ac4-249">Sélectionnez le **ProductId** colonne et les jeux **ReadOnly** à **True**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="01ac4-250">Sélectionnez le **CategoryId** colonne, puis appuyez sur la **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="01ac4-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="01ac4-251">Faire de même avec le **catégorie** colonne.</span><span class="sxs-lookup"><span data-stu-id="01ac4-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="01ac4-252">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-252">Press **OK**.</span></span>

    <span data-ttu-id="01ac4-253">Jusqu’ici, nous associées nos contrôles DataGridView aux composants BindingSource dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="01ac4-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="01ac4-254">Dans la section suivante, nous allons ajouter code pour le code-behind pour définir categoryBindingSource.DataSource à la collection d’entités qui sont actuellement suivies par DbContext.</span><span class="sxs-lookup"><span data-stu-id="01ac4-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="01ac4-255">Lorsque nous glisser-déplacer de produits sous la catégorie, le WinForms a pris en charge de configuration de la propriété productsBindingSource.DataSource à la propriété categoryBindingSource et productsBindingSource.DataMember aux produits.</span><span class="sxs-lookup"><span data-stu-id="01ac4-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="01ac4-256">En raison de cette liaison, seuls les produits qui appartiennent à la catégorie actuellement sélectionnée seront affichera dans le productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="01ac4-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="01ac4-257">Activer la **enregistrer** dans la barre d’outils de Navigation en cliquant sur le bouton droit de la souris et en sélectionnant **activé**.</span><span class="sxs-lookup"><span data-stu-id="01ac4-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Concepteur de formulaires 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="01ac4-259">Ajouter le Gestionnaire d’événements pour l’enregistrement bouton en double-cliquant sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="01ac4-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="01ac4-260">Cela ajoute le Gestionnaire d’événements et vous permettent du code-behind pour le formulaire.</span><span class="sxs-lookup"><span data-stu-id="01ac4-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="01ac4-261">Le code pour le **categoryBindingNavigatorSaveItem\_cliquez sur** Gestionnaire d’événements sera ajouté dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="01ac4-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="01ac4-262">Ajoutez le Code qui gère l’Interaction de données</span><span class="sxs-lookup"><span data-stu-id="01ac4-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="01ac4-263">Nous allons maintenant ajouter le code pour utiliser le ProductContext pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="01ac4-264">Mettre à jour le code de la fenêtre du formulaire principal comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="01ac4-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="01ac4-265">Le code déclare une instance d’exécution longue de ProductContext.</span><span class="sxs-lookup"><span data-stu-id="01ac4-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="01ac4-266">L’objet ProductContext est utilisé pour interroger et enregistrer les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="01ac4-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="01ac4-267">La méthode Dispose() sur l’instance ProductContext est ensuite appelée à partir de la méthode OnClosing substituée.</span><span class="sxs-lookup"><span data-stu-id="01ac4-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="01ac4-268">Les commentaires du code fournissent des informations sur ce que fait le code.</span><span class="sxs-lookup"><span data-stu-id="01ac4-268">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="01ac4-269">Tester l’Application de formulaires Windows</span><span class="sxs-lookup"><span data-stu-id="01ac4-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="01ac4-270">Compiler et exécuter l’application et vous pouvez tester la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="01ac4-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Écran 1 avant l’enregistrement](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="01ac4-272">Après avoir enregistré les clés de magasin généré sont affichées sur l’écran.</span><span class="sxs-lookup"><span data-stu-id="01ac4-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Écran 1 après enregistrement](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="01ac4-274">Si vous avez utilisé un Code First, vous verrez également qu’un **WinFormswithEFSample.ProductContext** base de données est créée pour vous.</span><span class="sxs-lookup"><span data-stu-id="01ac4-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Explorateur d’objets Server](~/ef6/media/serverobjexplorer.png)
