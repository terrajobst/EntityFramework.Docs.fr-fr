---
title: Procédures stockées avec plusieurs jeux de résultats - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: bb104ac5f584d26d279259a173de9afe3f018968
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996173"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Procédures stockées avec plusieurs jeux de résultats
Parfois, lorsque vous utilisez stockées procédures, vous devez retourner plusieurs résultats définissent. Ce scénario est couramment utilisé pour réduire le nombre de base de données allers-retours requis pour composer un seul écran. Avant d’EF5, Entity Framework permettrait la procédure stockée à appeler, mais ne renvoie que le premier jeu de résultats au code appelant.

Cet article vous montrera deux méthodes que vous pouvez utiliser pour accéder à plus d’un jeu de résultats à partir d’une procédure stockée dans Entity Framework. Une qui utilise seulement le code et fonctionne avec les deux du Code tout d’abord et le Concepteur EF et une qui ne fonctionne qu’avec le Concepteur EF. Les outils et la prise en charge de l’API pour cela doivent améliorer dans les futures versions d’Entity Framework.

## <a name="model"></a>Modèle

Les exemples de cet article utilisent un Blog de base et modèle de billets où un blog a un billet et nombreuses publications appartient à un blog unique. Nous allons utiliser une procédure stockée dans la base de données qui retourne tous les blogs et des publications, quelque chose comme suit :

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>L’accès aux résultats multiples définit avec le Code

Nous pouvons exécuter du code utilisé pour émettre une commande SQL brutes pour exécuter notre procédure stockée. L’avantage de cette approche est qu’elle fonctionne avec les deux Code tout d’abord et le Concepteur EF.

Afin d’obtenir des résultats multiples définit le travail que nous devons supprimer à l’API ObjectContext à l’aide de l’interface IObjectContextAdapter.

Une fois que nous avons un ObjectContext nous pouvons utiliser la méthode Translate pour traduire les résultats de notre procédure stockée dans les entités qui peuvent être suivies et utilisées dans EF comme d’habitude. L’exemple de code suivant illustre cela en action.

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

La méthode Translate accepte le lecteur que nous avons reçu lorsque nous avons exécuté la procédure, un nom de l’EntitySet et une MergeOption. Le nom EntitySet sera identique à la propriété DbSet sur votre contexte dérivé. L’énumération MergeOption contrôle la gestion des résultats si la même entité existe déjà dans la mémoire.

Ici, nous itérer dans la collection de blogs avant l’appel NextResult, il est important étant donné le code ci-dessus, car le premier jeu de résultats doit être utilisé avant de passer au jeu de résultats suivant.

Une fois traduire les deux méthodes sont appelées les entités de Blog et Post sont suivies par Entity Framework de la même façon que toute autre entité puis donc peut être modifié ou supprimé et enregistré comme d’habitude.

>[!NOTE]
> Entity Framework ne prend pas en compte un mappage de lorsqu’il crée des entités à l’aide de la méthode Translate. Elle correspondra simplement des noms de colonnes dans le jeu de résultats avec des noms de propriété de vos classes.

>[!NOTE]
> Que si vous avez activé, le chargement différé en accédant à la propriété de billets sur l’une des entités blog puis EF se connectera à la base de données charger en différé toutes ses publications, même si nous avons déjà chargé tous les. Il s’agit, car Entity Framework ne peut pas savoir si vous avez chargé tous les billets ou s’il en existe plus dans la base de données. Si vous souhaitez éviter ce problème vous devrez désactiver le chargement différé.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Plusieurs jeux de résultats avec EDMX configuré dans

>[!NOTE]
> Vous devez cibler .NET Framework 4.5 pour être en mesure de configurer plusieurs jeux de résultats dans EDMX. Si vous ciblez le .NET 4.0, vous pouvez utiliser la méthode basée sur le code indiquée dans la section précédente.

Si vous utilisez le Concepteur EF, vous pouvez également modifier votre modèle afin qu’il sache sur les jeux de résultats qui seront renvoyées. Une chose à savoir avant de la main est que les outils ne sont pas plusieurs résultats défini prenant en charge, vous devez modifier manuellement le fichier edmx. Modification du fichier edmx que cela fonctionne, mais elle entraîne également l’arrêt la validation du modèle dans Visual Studio. Par conséquent, si vous validez votre modèle vous obtiendrez toujours des erreurs.

-   Pour ce faire, vous devez ajouter la procédure stockée à votre modèle comme vous le feriez pour une requête de jeu de résultats unique.
-   Une fois que vous avez obtenu cela, vous devez cliquez avec le bouton droit sur votre modèle et sélectionnez **ouvrir avec...** puis **Xml**

    ![OpenAs](~/ef6/media/openas.png)

Une fois que le modèle ouvert en tant que XML, vous devez procédez comme suit :

-   Rechercher l’importation de fonction et de type complexe dans votre modèle :

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

 

-   Supprimer le type complexe
-   Mettre à jour de l’importation de fonction afin qu’elle correspond à vos entités, dans notre cas, qu'il doit ressembler à ce qui suit :

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Cela indique le modèle que la procédure stockée retourne deux collections, une des entrées de blog et celui de la validation d’entrées.

-   Recherchez l’élément de mappage de fonction :

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

-   Remplacez le mappage de résultat avec l’un pour chaque entité renvoyée, tels que les éléments suivants :

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

Il est également possible de mapper les jeux de résultats à des types complexes, tel que celui créé par défaut. Pour ce faire, vous créez un nouveau type complexe, au lieu de les supprimer et utilisez les types complexes partout que que vous aviez utilisé les noms d’entité dans les exemples ci-dessus.

Une fois que ces mappages ont été modifiées, vous pouvez enregistrer le modèle et exécutez le code suivant pour utiliser la procédure stockée :

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
> Si vous modifiez manuellement le fichier edmx pour votre modèle, il sera remplacé si vous régénérez jamais le modèle à partir de la base de données.

## <a name="summary"></a>Récapitulatif

Ici, nous avons décrit deux méthodes d’accès aux résultats multiples définit à l’aide d’Entity Framework. Les deux d'entre eux sont valides en fonction de votre situation et préférences et vous devez choisir celui qui semble mieux à votre situation. Il est prévu que la prise en charge pour résultat plusieurs jeux sera améliorée dans les futures versions d’Entity Framework et que les étapes dans ce document ne sera plus nécessaire.
