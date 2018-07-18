---
title: Première insertion de code, mettre à jour et supprimer des procédures stockées - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121466"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="5cdc9-102">Code première insertion, de mettre à jour et supprimer des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="5cdc9-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="5cdc9-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="5cdc9-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="5cdc9-105">Par défaut, Code First configure toutes les entités pour effectuer l’insertion, de mettre à jour et de supprimer des commandes à l’aide de l’accès direct à la table.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="5cdc9-106">En commençant dans EF6, vous pouvez configurer votre modèle Code First pour utiliser des procédures stockées pour certaines ou toutes les entités dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="5cdc9-107">Mappage d’entité de base</span><span class="sxs-lookup"><span data-stu-id="5cdc9-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="5cdc9-108">Vous pouvez opter pour l’utilisation de procédures stockées pour insérer, mettre à jour et supprimer à l’aide de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="5cdc9-109">Cela provoquera Code First utiliser certaines conventions pour créer la forme attendue des procédures stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="5cdc9-110">Trois procédures nommé  **\<type_name\>_Insérer**,  **\<type_name\>_mettre à jour** et  **\<type_ nom\>_Supprimer** (par exemple, Blog_Insert, Blog_Update et Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="5cdc9-111">Noms de paramètres correspondent aux noms de propriété.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="5cdc9-112">Si vous utilisez HasColumnName() ou l’attribut de colonne pour renommer la colonne pour une propriété donnée ce nom est utilisé pour les paramètres au lieu du nom de propriété.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="5cdc9-113">**La procédure stockée insert** aura un paramètre pour chaque propriété, à l’exception de celles qui sont marquées comme magasin généré (identité ou calculée).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="5cdc9-114">La procédure stockée doit retourner un jeu de résultats avec une colonne pour chaque propriété de base généré.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="5cdc9-115">**La mise à jour de procédure stockée** aura un paramètre pour chaque propriété, à l’exception de celles qui sont marquées avec un modèle de magasin généré de « Calculé ».</span><span class="sxs-lookup"><span data-stu-id="5cdc9-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="5cdc9-116">Bien que certains jetons d’accès concurrentiel requièrent un paramètre pour la valeur d’origine, consultez la *jetons d’accès concurrentiel* section ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="5cdc9-117">La procédure stockée doit retourner un jeu de résultats avec une colonne pour chaque propriété calculée.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="5cdc9-118">**La suppression d’une procédure stockée** doit avoir un paramètre pour la valeur de clé de l’entité (ou plusieurs paramètres si l’entité a une clé composite).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="5cdc9-119">En outre, la procédure de suppression doit également avoir des paramètres pour toutes les clés étrangères association indépendante sur la table cible (les relations qui n’ont pas de propriétés de clé étrangère correspondantes déclarées dans l’entité).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="5cdc9-120">Bien que certains jetons d’accès concurrentiel requièrent un paramètre pour la valeur d’origine, consultez la *jetons d’accès concurrentiel* section ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="5cdc9-121">À l’aide de la classe suivante comme exemple :</span><span class="sxs-lookup"><span data-stu-id="5cdc9-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="5cdc9-122">La valeur par défaut des procédures stockées serait :</span><span class="sxs-lookup"><span data-stu-id="5cdc9-122">The default stored procedures would be:</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="5cdc9-123">Substituer les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="5cdc9-123">Overriding the Defaults</span></span>  

<span data-ttu-id="5cdc9-124">Vous pouvez remplacer tout ou partie de ce qui a été configuré par défaut.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="5cdc9-125">Vous pouvez modifier le nom d’une ou plusieurs procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="5cdc9-126">Cet exemple renomme la procédure stockée update uniquement.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="5cdc9-127">Cet exemple renomme tous les trois procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="5cdc9-128">Dans ces exemples, les appels sont chaînées ensemble, mais vous pouvez également utiliser la syntaxe de bloc d’expression lambda.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

<span data-ttu-id="5cdc9-129">Cet exemple renomme le paramètre pour la propriété BlogId sur la procédure stockée update.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="5cdc9-130">Ces appels sont enchaînées et composable.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="5cdc9-131">Voici un exemple qui renomme les trois procédures stockées et leurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

<span data-ttu-id="5cdc9-132">Vous pouvez également modifier le nom des colonnes du jeu de résultats qui contient des valeurs de la base de données générée.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="5cdc9-133">Relations sans une clé étrangère dans la classe (Associations indépendantes)</span><span class="sxs-lookup"><span data-stu-id="5cdc9-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="5cdc9-134">Lorsqu’une propriété de clé étrangère est incluse dans la définition de classe, le paramètre correspondant peut être renommé dans la même façon que toute autre propriété.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="5cdc9-135">Lorsqu’il existe une relation sans une propriété de clé étrangère dans la classe, le nom de paramètre par défaut est  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="5cdc9-136">Par exemple, les définitions de classe suivantes entraînerait un paramètre Blog_BlogId qui est attendu dans les procédures stockées pour insérer et mettre à jour des publications.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="5cdc9-137">Substituer les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="5cdc9-137">Overriding the Defaults</span></span>  

<span data-ttu-id="5cdc9-138">Vous pouvez modifier les paramètres pour les clés étrangères qui ne figurent pas dans la classe en fournissant le chemin d’accès à la propriété de clé primaire à la méthode de paramètre.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="5cdc9-139">Si vous n’avez pas une propriété de navigation sur l’entité dépendante (ex.)</span><span class="sxs-lookup"><span data-stu-id="5cdc9-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="5cdc9-140">aucune propriété Post.Blog) vous pouvez ensuite utiliser la méthode d’Association pour identifier l’autre extrémité de la relation, puis configurer les paramètres qui correspondent à chacune des propriétés de clé ne.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="5cdc9-141">Jetons d'accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="5cdc9-141">Concurrency Tokens</span></span>  

<span data-ttu-id="5cdc9-142">Mise à jour et supprimer les procédures serez peut-être amené à gérer avec l’accès concurrentiel :</span><span class="sxs-lookup"><span data-stu-id="5cdc9-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="5cdc9-143">Si l’entité contient des jetons d’accès concurrentiel, la procédure stockée peut éventuellement avoir un paramètre de sortie qui retourne le nombre de lignes mises à jour/supprimées (lignes affectées).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="5cdc9-144">Un tel paramètre doit être configuré à l’aide de la méthode RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="5cdc9-145">Par défaut, EF utilise la valeur de retour à partir de la méthode ExecuteNonQuery pour déterminer combien de lignes ont été affectés.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="5cdc9-146">Spécification d’un paramètre de sortie de lignes affectées est utile si vous effectuez une logique dans votre procédure stockée qui entraînerait la valeur de retour de ExecuteNonQuery qui est incorrect (du point de vue d’Entity Framework) à la fin de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="5cdc9-147">Pour chaque d’accès concurrentiel, il le jeton sera un paramètre nommé  **\<property_name\>_Original** (par exemple, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="5cdc9-148">Cela recevront la valeur d’origine de cette propriété : la valeur lorsqu’il est interrogé à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="5cdc9-149">Les jetons d’accès concurrentiel qui sont calculées par la base de données – tels que les horodatages – a uniquement un paramètre de valeur d’origine.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="5cdc9-150">Propriétés non calculées qui sont définies en tant que jetons d’accès concurrentiel aura également un paramètre pour la nouvelle valeur dans la procédure de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="5cdc9-151">Cet exemple utilise les conventions d’affectation de noms déjà vu pour les nouvelles valeurs.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="5cdc9-152">Un exemple de ce jeton serait être à l’aide URL d’un Blog sous forme d’un jeton d’accès concurrentiel, la nouvelle valeur est requise, car cela peut être mis à jour vers une nouvelle valeur par votre code (contrairement à un jeton de l’horodatage qui est uniquement mis à jour par la base de données).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="5cdc9-153">Il s’agit d’un exemple de classe et de mettre à jour de la procédure stockée avec un jeton d’accès concurrentiel d’horodatage.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

<span data-ttu-id="5cdc9-154">Voici un exemple de classe et de mettre à jour de la procédure stockée avec un jeton d’accès concurrentiel de non calculée.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="5cdc9-155">Substituer les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="5cdc9-155">Overriding the Defaults</span></span>  

<span data-ttu-id="5cdc9-156">Vous pouvez éventuellement introduire un paramètre des lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="5cdc9-157">Pour les jetons d’accès concurrentiel calculée de la base de données – où seule la valeur d’origine est passée : vous pouvez simplement utiliser le mécanisme de changement de nom de paramètre standard pour renommer le paramètre pour la valeur d’origine.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="5cdc9-158">Pour les jetons d’accès concurrentiel de non calculée – où est passée à la fois la valeur d’origine et nouvelle – vous pouvez utiliser une surcharge de paramètre qui vous permet de fournir un nom pour chaque paramètre.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="5cdc9-159">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="5cdc9-159">Many to Many Relationships</span></span>  

<span data-ttu-id="5cdc9-160">Nous allons utiliser les classes suivantes comme exemple dans cette section.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-160">We’ll use the following classes as an example in this section.</span></span>  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="5cdc9-161">Plusieurs à plusieurs relations peuvent être mappées à des procédures stockées avec la syntaxe suivante.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="5cdc9-162">Si aucune autre configuration n’est pas fournie la forme de procédure stockée suivante est utilisée par défaut.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="5cdc9-163">Deux procédures nommés stockées  **\<type_one\>\<type_two\>_Insérer** et  **\<type_one\>\<type_two \>_Supprimer** (par exemple, PostTag_Insert et PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="5cdc9-164">Les paramètres seront les valeurs de clés pour chaque type.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="5cdc9-165">Le nom de chaque paramètre en cours **\<type_name\>_\<property_name\>** (par exemple, Post_PostId et Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="5cdc9-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="5cdc9-166">Voici des exemples insérer et mettre à jour des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-166">Here are example insert and update stored procedures.</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="5cdc9-167">Substituer les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="5cdc9-167">Overriding the Defaults</span></span>  

<span data-ttu-id="5cdc9-168">Les noms de procédure et le paramètre peuvent être configurés de manière similaire aux procédures de l’entité stockée.</span><span class="sxs-lookup"><span data-stu-id="5cdc9-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
