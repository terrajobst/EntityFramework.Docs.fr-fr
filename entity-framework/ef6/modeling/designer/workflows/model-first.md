---
title: Model First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 3dd0eba29619f09995d7009dd29462c14bde98c4
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251139"
---
# <a name="model-first"></a><span data-ttu-id="24826-102">Tout d’abord de modèle</span><span class="sxs-lookup"><span data-stu-id="24826-102">Model First</span></span>
<span data-ttu-id="24826-103">Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Model First à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="24826-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="24826-104">Modèle tout d’abord vous permet de créer un nouveau modèle à l’aide d’Entity Framework Designer, puis générez un schéma de base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="24826-105">Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="24826-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="24826-106">Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="24826-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="24826-107">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="24826-107">Watch the video</span></span>
<span data-ttu-id="24826-108">Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Model First à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="24826-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="24826-109">Modèle tout d’abord vous permet de créer un nouveau modèle à l’aide d’Entity Framework Designer, puis générez un schéma de base de données à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="24826-110">Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="24826-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="24826-111">Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="24826-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="24826-112">**Présentée par** : [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="24826-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="24826-113">**Vidéo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="24826-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="24826-114">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="24826-114">Pre-Requisites</span></span>

<span data-ttu-id="24826-115">Vous devez disposer de Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="24826-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="24826-116">Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.</span><span class="sxs-lookup"><span data-stu-id="24826-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="24826-117">1. Création de l’application</span><span class="sxs-lookup"><span data-stu-id="24826-117">1. Create the Application</span></span>

<span data-ttu-id="24826-118">Pour simplifier les choses, nous allons créer une application de console de base qui utilise le premier modèle pour accéder aux données :</span><span class="sxs-lookup"><span data-stu-id="24826-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="24826-119">Ouvrir Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24826-119">Open Visual Studio</span></span>
-   <span data-ttu-id="24826-120">**Fichier -&gt; nouveau -&gt; projet...**</span><span class="sxs-lookup"><span data-stu-id="24826-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="24826-121">Sélectionnez **Windows** dans le menu de gauche et **Application Console**</span><span class="sxs-lookup"><span data-stu-id="24826-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="24826-122">Entrez **ModelFirstSample** comme nom</span><span class="sxs-lookup"><span data-stu-id="24826-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="24826-123">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="24826-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="24826-124">2. Créer le modèle</span><span class="sxs-lookup"><span data-stu-id="24826-124">2. Create Model</span></span>

<span data-ttu-id="24826-125">Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="24826-126">**Projet -&gt; ajouter un nouvel élément...**</span><span class="sxs-lookup"><span data-stu-id="24826-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="24826-127">Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="24826-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="24826-128">Entrez **BloggingModel** comme nom et cliquez sur **OK**, cette action lance l’Assistant Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="24826-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="24826-129">Sélectionnez **modèle vide** et cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="24826-129">Select **Empty Model** and click **Finish**</span></span>

    ![Créer le modèle vide](~/ef6/media/createemptymodel.png)

<span data-ttu-id="24826-131">Entity Framework Designer s’ouvre avec un modèle vide.</span><span class="sxs-lookup"><span data-stu-id="24826-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="24826-132">Maintenant, nous pouvons commencer l’ajout d’entités, les propriétés et les associations au modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="24826-133">Avec le bouton droit sur l’aire de conception et sélectionnez **propriétés**</span><span class="sxs-lookup"><span data-stu-id="24826-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="24826-134">Dans la modification de la fenêtre Propriétés la **Nom_conteneur_entités** à **BloggingContext**
    *il s’agit du nom de contexte dérivé qui sera généré pour vous, le contexte représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données*</span><span class="sxs-lookup"><span data-stu-id="24826-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="24826-135">Avec le bouton droit sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; entité...**</span><span class="sxs-lookup"><span data-stu-id="24826-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="24826-136">Entrez **Blog** en tant que le nom de l’entité et **BlogId** comme le nom de la clé et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="24826-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Ajouter une entité de Blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="24826-138">Avec le bouton droit sur la nouvelle entité sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**, entrez **nom** comme nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="24826-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="24826-139">Répétez ce processus pour ajouter un **Url** propriété.</span><span class="sxs-lookup"><span data-stu-id="24826-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="24826-140">Avec le bouton droit sur le **Url** propriété sur l’aire de conception et sélectionnez **propriétés**, lors de la modification de la fenêtre Propriétés la **Nullable** à **True** 
     *Cela nous permet d’enregistrer un Blog à la base de données sans lui assigner une Url*</span><span class="sxs-lookup"><span data-stu-id="24826-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="24826-141">En utilisant les techniques que vous venez d’apprendre, d’ajouter un **Post** entité avec une **IDMessage** propriété de clé</span><span class="sxs-lookup"><span data-stu-id="24826-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="24826-142">Ajouter **titre** et **contenu** propriétés scalaires à le **Post** entité</span><span class="sxs-lookup"><span data-stu-id="24826-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="24826-143">Maintenant que nous avons deux entités, il est temps d’ajouter une association (ou la relation) entre eux.</span><span class="sxs-lookup"><span data-stu-id="24826-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="24826-144">Avec le bouton droit sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; Association...**</span><span class="sxs-lookup"><span data-stu-id="24826-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="24826-145">Qu’une extrémité de la relation pointent vers **Blog** avec une multiplicité de **un** et le point de terminaison à **Post** avec une multiplicité de **nombreux** 
     *Cela signifie qu’un Blog a de nombreuses publications et une publication appartient à un Blog*</span><span class="sxs-lookup"><span data-stu-id="24826-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="24826-146">Vérifiez le **ajouter des propriétés de clé étrangère à l’entité 'Post'** case est activée et cliquez sur **OK**</span><span class="sxs-lookup"><span data-stu-id="24826-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Ajouter une Association MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="24826-148">Nous disposons désormais d’un modèle simple que nous pouvons générer une base de données et l’utiliser pour lire et écrire des données.</span><span class="sxs-lookup"><span data-stu-id="24826-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modèle Initial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="24826-150">Étapes supplémentaires dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="24826-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="24826-151">Si vous travaillez dans Visual Studio 2010, il existe quelques étapes supplémentaires à suivre pour mettre à niveau vers la dernière version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="24826-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="24826-152">Il est important de la mise à niveau, car il vous donne accès à une surface d’API améliorée, qui est beaucoup plus facile à utiliser, ainsi que les derniers correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="24826-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="24826-153">Tout d’abord, nous devons obtenir la dernière version d’Entity Framework à partir de NuGet.</span><span class="sxs-lookup"><span data-stu-id="24826-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="24826-154">\*\*Projet –&gt; gérer les Packages NuGet... \*\* 
     \*Si vous n’avez pas le \**gérer les Packages NuGet... \*\* option, vous devez installer le [version la plus récente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="24826-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="24826-155">Sélectionnez le **Online** onglet</span><span class="sxs-lookup"><span data-stu-id="24826-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="24826-156">Sélectionnez le **EntityFramework** package</span><span class="sxs-lookup"><span data-stu-id="24826-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="24826-157">Cliquez sur **installer**</span><span class="sxs-lookup"><span data-stu-id="24826-157">Click **Install**</span></span>

<span data-ttu-id="24826-158">Ensuite, nous devons échanger notre modèle pour générer du code qui utilise l’API DbContext, qui a été introduit dans les versions ultérieures d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="24826-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="24826-159">Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**</span><span class="sxs-lookup"><span data-stu-id="24826-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="24826-160">Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**</span><span class="sxs-lookup"><span data-stu-id="24826-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="24826-161">Sélectionnez le EF **5.x générateur DbContext pour C\#**, entrez **BloggingModel** comme nom et cliquez sur **ajouter**</span><span class="sxs-lookup"><span data-stu-id="24826-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modèle de DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="24826-163">3. Génération de la base de données</span><span class="sxs-lookup"><span data-stu-id="24826-163">3. Generating the Database</span></span>

<span data-ttu-id="24826-164">Compte tenu de notre modèle, Entity Framework peut calculer un schéma de base de données qui nous permettra de stocker et récupérer des données à l’aide du modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="24826-165">Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :</span><span class="sxs-lookup"><span data-stu-id="24826-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="24826-166">Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.</span><span class="sxs-lookup"><span data-stu-id="24826-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="24826-167">Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de données.</span><span class="sxs-lookup"><span data-stu-id="24826-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="24826-168">Procédons à générer la base de données.</span><span class="sxs-lookup"><span data-stu-id="24826-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="24826-169">Avec le bouton droit sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle...**</span><span class="sxs-lookup"><span data-stu-id="24826-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="24826-170">Cliquez sur **nouvelle connexion...**</span><span class="sxs-lookup"><span data-stu-id="24826-170">Click **New Connection…**</span></span> <span data-ttu-id="24826-171">et spécifiez la base de données locale ou de SQL Express, selon la version de Visual Studio que vous utilisez, entrez **ModelFirst.Blogging** en tant que le nom de la base de données.</span><span class="sxs-lookup"><span data-stu-id="24826-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Connexion de base de données locale MF](~/ef6/media/localdbconnectionmf.png)

    ![Connexion SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="24826-174">Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**</span><span class="sxs-lookup"><span data-stu-id="24826-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="24826-175">Sélectionnez **suivant** et Entity Framework Designer calculera un script pour créer le schéma de base de données</span><span class="sxs-lookup"><span data-stu-id="24826-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="24826-176">Une fois le script s’affiche, cliquez sur **Terminer** et le script sera ajouté à votre projet et ouvert</span><span class="sxs-lookup"><span data-stu-id="24826-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="24826-177">Avec le bouton droit sur le script et sélectionnez **Execute**, vous devrez spécifier la base de données pour se connecter à, spécifiez LocalDB ou Express de SQL Server, selon la version de Visual Studio que vous utilisez</span><span class="sxs-lookup"><span data-stu-id="24826-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="24826-178">4. Lire et écrire des données</span><span class="sxs-lookup"><span data-stu-id="24826-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="24826-179">Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données.</span><span class="sxs-lookup"><span data-stu-id="24826-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="24826-180">Les classes que nous allons utiliser pour accéder aux données sont générés automatiquement pour vous selon le fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="24826-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="24826-181">*Cette capture d’écran provient de Visual Studio 2012, si vous utilisez Visual Studio 2010 le BloggingModel.tt et les fichiers BloggingModel.Context.tt seront directement sous votre projet plutôt qu’imbriqué sous le fichier EDMX.*</span><span class="sxs-lookup"><span data-stu-id="24826-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classes générées](~/ef6/media/generatedclasses.png)

<span data-ttu-id="24826-183">Implémentez la méthode Main dans Program.cs, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24826-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="24826-184">Ce code crée une nouvelle instance de notre contexte et il utilise ensuite pour insérer un nouveau Blog.</span><span class="sxs-lookup"><span data-stu-id="24826-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="24826-185">Il utilise ensuite une requête LINQ pour récupérer tous les Blogs à partir de la base de données classé par ordre alphabétique par titre.</span><span class="sxs-lookup"><span data-stu-id="24826-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="24826-186">Vous pouvez maintenant exécuter l’application et le tester.</span><span class="sxs-lookup"><span data-stu-id="24826-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="24826-187">5. Gestion des modifications du modèle</span><span class="sxs-lookup"><span data-stu-id="24826-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="24826-188">Il est maintenant temps d’apporter des modifications à notre modèle, lorsque nous apporter ces modifications, que nous devons également mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="24826-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="24826-189">Nous allons commencer en ajoutant une nouvelle entité d’utilisateur à notre modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="24826-190">Ajouter un nouveau **utilisateur** nom de l’entité avec **nom d’utilisateur** comme nom de clé et **chaîne** comme type de propriété pour la clé</span><span class="sxs-lookup"><span data-stu-id="24826-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Ajouter une entité d’utilisateur](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="24826-192">Avec le bouton droit sur le **nom d’utilisateur** propriété sur l’aire de conception et sélectionnez **propriétés**, changement de fenêtre dans les propriétés le **MaxLength** à \*\*50 \*\* 
     *Cela limite les données qui peuvent être stockées dans le nom d’utilisateur à 50 caractères*</span><span class="sxs-lookup"><span data-stu-id="24826-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="24826-193">Ajouter un **DisplayName** propriété scalaire à la **utilisateur** entité</span><span class="sxs-lookup"><span data-stu-id="24826-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="24826-194">Nous disposons désormais d’un modèle mis à jour et nous sommes prêts à mettre à jour de la base de données pour prendre en charge de notre nouveau type d’entité utilisateur.</span><span class="sxs-lookup"><span data-stu-id="24826-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="24826-195">Avec le bouton droit sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle...** , Entity Framework calculera un script pour recréer un schéma basé sur le modèle mis à jour.</span><span class="sxs-lookup"><span data-stu-id="24826-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="24826-196">Cliquez sur **terminer**</span><span class="sxs-lookup"><span data-stu-id="24826-196">Click **Finish**</span></span>
-   <span data-ttu-id="24826-197">Vous pouvez recevoir des avertissements relatifs à remplacer le script DDL existant et les parties de stockage et de mappage du modèle, cliquez sur **Oui** pour ces deux avertissements</span><span class="sxs-lookup"><span data-stu-id="24826-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="24826-198">Le script SQL mis à jour pour créer la base de données est ouvert pour vous</span><span class="sxs-lookup"><span data-stu-id="24826-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="24826-199">*Le script généré supprime toutes les tables existantes et puis recréer le schéma à partir de zéro. Cela peut fonctionner pour un développement local, mais n’est pas viable pour l’envoi des modifications à une base de données qui a déjà été déployé. Si vous avez besoin publier les modifications dans une base de données qui a déjà été déployé, vous devez modifier le script ou utiliser un outil de comparaison de schéma pour calculer un script de migration.*</span><span class="sxs-lookup"><span data-stu-id="24826-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="24826-200">Avec le bouton droit sur le script et sélectionnez **Execute**, vous devrez spécifier la base de données pour se connecter à, spécifiez LocalDB ou Express de SQL Server, selon la version de Visual Studio que vous utilisez</span><span class="sxs-lookup"><span data-stu-id="24826-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="24826-201">Résumé</span><span class="sxs-lookup"><span data-stu-id="24826-201">Summary</span></span>

<span data-ttu-id="24826-202">Dans cette procédure pas à pas, nous avons étudié développement Model First, qui nous a permis de créer un modèle dans le Concepteur EF et puis générer une base de données à partir de ce modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="24826-203">Ensuite, nous avons utilisé le modèle pour lire et écrire des données à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="24826-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="24826-204">Enfin, nous mis à jour le modèle et puis recréer le schéma de base de données pour faire correspondre le modèle.</span><span class="sxs-lookup"><span data-stu-id="24826-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
