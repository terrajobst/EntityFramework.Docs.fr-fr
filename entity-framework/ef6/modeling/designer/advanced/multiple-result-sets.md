---
title: Procédures stockées avec plusieurs jeux de résultats - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 56c28f05bd7efe1b54d6cadd32afe0e9c6cf38b5
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251009"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="0ac29-102">Procédures stockées avec plusieurs jeux de résultats</span><span class="sxs-lookup"><span data-stu-id="0ac29-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="0ac29-103">Parfois, lorsque vous utilisez stockées procédures, vous devez retourner plusieurs résultats définissent.</span><span class="sxs-lookup"><span data-stu-id="0ac29-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="0ac29-104">Ce scénario est couramment utilisé pour réduire le nombre de base de données allers-retours requis pour composer un seul écran.</span><span class="sxs-lookup"><span data-stu-id="0ac29-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="0ac29-105">Avant d’EF5, Entity Framework permettrait la procédure stockée à appeler, mais ne renvoie que le premier jeu de résultats au code appelant.</span><span class="sxs-lookup"><span data-stu-id="0ac29-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="0ac29-106">Cet article vous montrera deux méthodes que vous pouvez utiliser pour accéder à plus d’un jeu de résultats à partir d’une procédure stockée dans Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ac29-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="0ac29-107">Une qui utilise seulement le code et fonctionne avec les deux du Code tout d’abord et le Concepteur EF et une qui ne fonctionne qu’avec le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="0ac29-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="0ac29-108">Les outils et la prise en charge de l’API pour cela doivent améliorer dans les futures versions d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ac29-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="0ac29-109">Modèle</span><span class="sxs-lookup"><span data-stu-id="0ac29-109">Model</span></span>

<span data-ttu-id="0ac29-110">Les exemples de cet article utilisent un Blog de base et modèle de billets où un blog a un billet et nombreuses publications appartient à un blog unique.</span><span class="sxs-lookup"><span data-stu-id="0ac29-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="0ac29-111">Nous allons utiliser une procédure stockée dans la base de données qui retourne tous les blogs et des publications, quelque chose comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ac29-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="0ac29-112">L’accès aux résultats multiples définit avec le Code</span><span class="sxs-lookup"><span data-stu-id="0ac29-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="0ac29-113">Nous pouvons exécuter du code utilisé pour émettre une commande SQL brutes pour exécuter notre procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="0ac29-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="0ac29-114">L’avantage de cette approche est qu’elle fonctionne avec les deux Code tout d’abord et le Concepteur EF.</span><span class="sxs-lookup"><span data-stu-id="0ac29-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="0ac29-115">Afin d’obtenir des résultats multiples définit le travail que nous devons supprimer à l’API ObjectContext à l’aide de l’interface IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="0ac29-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="0ac29-116">Une fois que nous avons un ObjectContext nous pouvons utiliser la méthode Translate pour traduire les résultats de notre procédure stockée dans les entités qui peuvent être suivies et utilisées dans EF comme d’habitude.</span><span class="sxs-lookup"><span data-stu-id="0ac29-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="0ac29-117">L’exemple de code suivant illustre cela en action.</span><span class="sxs-lookup"><span data-stu-id="0ac29-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="0ac29-118">La méthode Translate accepte le lecteur que nous avons reçu lorsque nous avons exécuté la procédure, un nom de l’EntitySet et une MergeOption.</span><span class="sxs-lookup"><span data-stu-id="0ac29-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="0ac29-119">Le nom EntitySet sera identique à la propriété DbSet sur votre contexte dérivé.</span><span class="sxs-lookup"><span data-stu-id="0ac29-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="0ac29-120">L’énumération MergeOption contrôle la gestion des résultats si la même entité existe déjà dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="0ac29-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="0ac29-121">Ici, nous itérer dans la collection de blogs avant l’appel NextResult, il est important étant donné le code ci-dessus, car le premier jeu de résultats doit être utilisé avant de passer au jeu de résultats suivant.</span><span class="sxs-lookup"><span data-stu-id="0ac29-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="0ac29-122">Une fois traduire les deux méthodes sont appelées les entités de Blog et Post sont suivies par Entity Framework de la même façon que toute autre entité puis donc peut être modifié ou supprimé et enregistré comme d’habitude.</span><span class="sxs-lookup"><span data-stu-id="0ac29-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="0ac29-123">Entity Framework ne prend pas en compte un mappage de lorsqu’il crée des entités à l’aide de la méthode Translate.</span><span class="sxs-lookup"><span data-stu-id="0ac29-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="0ac29-124">Elle correspondra simplement des noms de colonnes dans le jeu de résultats avec des noms de propriété de vos classes.</span><span class="sxs-lookup"><span data-stu-id="0ac29-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="0ac29-125">Que si vous avez activé, le chargement différé en accédant à la propriété de billets sur l’une des entités blog puis EF se connectera à la base de données charger en différé toutes ses publications, même si nous avons déjà chargé tous les.</span><span class="sxs-lookup"><span data-stu-id="0ac29-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="0ac29-126">Il s’agit, car Entity Framework ne peut pas savoir si vous avez chargé tous les billets ou s’il en existe plus dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0ac29-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="0ac29-127">Si vous souhaitez éviter ce problème vous devrez désactiver le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="0ac29-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="0ac29-128">Plusieurs jeux de résultats avec EDMX configuré dans</span><span class="sxs-lookup"><span data-stu-id="0ac29-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="0ac29-129">Vous devez cibler .NET Framework 4.5 pour être en mesure de configurer plusieurs jeux de résultats dans EDMX.</span><span class="sxs-lookup"><span data-stu-id="0ac29-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="0ac29-130">Si vous ciblez le .NET 4.0, vous pouvez utiliser la méthode basée sur le code indiquée dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="0ac29-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="0ac29-131">Si vous utilisez le Concepteur EF, vous pouvez également modifier votre modèle afin qu’il sache sur les jeux de résultats qui seront renvoyées.</span><span class="sxs-lookup"><span data-stu-id="0ac29-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="0ac29-132">Une chose à savoir avant de la main est que les outils ne sont pas plusieurs résultats défini prenant en charge, vous devez modifier manuellement le fichier edmx.</span><span class="sxs-lookup"><span data-stu-id="0ac29-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="0ac29-133">Modification du fichier edmx que cela fonctionne, mais elle entraîne également l’arrêt la validation du modèle dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ac29-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="0ac29-134">Par conséquent, si vous validez votre modèle vous obtiendrez toujours des erreurs.</span><span class="sxs-lookup"><span data-stu-id="0ac29-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="0ac29-135">Pour ce faire, vous devez ajouter la procédure stockée à votre modèle comme vous le feriez pour une requête de jeu de résultats unique.</span><span class="sxs-lookup"><span data-stu-id="0ac29-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="0ac29-136">Une fois que vous avez obtenu cela, vous devez cliquez avec le bouton droit sur votre modèle et sélectionnez **ouvrir avec...**</span><span class="sxs-lookup"><span data-stu-id="0ac29-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="0ac29-137">puis **Xml**</span><span class="sxs-lookup"><span data-stu-id="0ac29-137">then **Xml**</span></span>

    ![Ouvrez en tant que](~/ef6/media/openas.png)

<span data-ttu-id="0ac29-139">Une fois que le modèle ouvert en tant que XML, vous devez procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ac29-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="0ac29-140">Rechercher l’importation de fonction et de type complexe dans votre modèle :</span><span class="sxs-lookup"><span data-stu-id="0ac29-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="0ac29-141">Supprimer le type complexe</span><span class="sxs-lookup"><span data-stu-id="0ac29-141">Remove the complex type</span></span>
-   <span data-ttu-id="0ac29-142">Mettre à jour de l’importation de fonction afin qu’elle correspond à vos entités, dans notre cas, qu'il doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0ac29-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="0ac29-143">Cela indique le modèle que la procédure stockée retourne deux collections, une des entrées de blog et celui de la validation d’entrées.</span><span class="sxs-lookup"><span data-stu-id="0ac29-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="0ac29-144">Recherchez l’élément de mappage de fonction :</span><span class="sxs-lookup"><span data-stu-id="0ac29-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="0ac29-145">Remplacez le mappage de résultat avec l’un pour chaque entité renvoyée, tels que les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0ac29-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="0ac29-146">Il est également possible de mapper les jeux de résultats à des types complexes, tel que celui créé par défaut.</span><span class="sxs-lookup"><span data-stu-id="0ac29-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="0ac29-147">Pour ce faire, vous créez un nouveau type complexe, au lieu de les supprimer et utilisez les types complexes partout que que vous aviez utilisé les noms d’entité dans les exemples ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="0ac29-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="0ac29-148">Une fois que ces mappages ont été modifiées, vous pouvez enregistrer le modèle et exécutez le code suivant pour utiliser la procédure stockée :</span><span class="sxs-lookup"><span data-stu-id="0ac29-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="0ac29-149">Si vous modifiez manuellement le fichier edmx pour votre modèle, il sera remplacé si vous régénérez jamais le modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="0ac29-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="0ac29-150">Résumé</span><span class="sxs-lookup"><span data-stu-id="0ac29-150">Summary</span></span>

<span data-ttu-id="0ac29-151">Ici, nous avons décrit deux méthodes d’accès aux résultats multiples définit à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ac29-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="0ac29-152">Les deux d'entre eux sont valides en fonction de votre situation et préférences et vous devez choisir celui qui semble mieux à votre situation.</span><span class="sxs-lookup"><span data-stu-id="0ac29-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="0ac29-153">Il est prévu que la prise en charge pour résultat plusieurs jeux sera améliorée dans les futures versions d’Entity Framework et que les étapes dans ce document ne sera plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="0ac29-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
