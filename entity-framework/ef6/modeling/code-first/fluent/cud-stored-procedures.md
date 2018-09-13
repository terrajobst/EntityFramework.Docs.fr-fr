---
title: Première insertion de code, mettre à jour et supprimer des procédures stockées - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489620"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code première insertion, de mettre à jour et supprimer des procédures stockées
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Par défaut, Code First configure toutes les entités pour effectuer l’insertion, de mettre à jour et de supprimer des commandes à l’aide de l’accès direct à la table. En commençant dans EF6, vous pouvez configurer votre modèle Code First pour utiliser des procédures stockées pour certaines ou toutes les entités dans votre modèle.  

## <a name="basic-entity-mapping"></a>Mappage d’entité de base  

Vous pouvez opter pour l’utilisation de procédures stockées pour insérer, mettre à jour et supprimer à l’aide de l’API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Cela provoquera Code First utiliser certaines conventions pour créer la forme attendue des procédures stockées dans la base de données.  

- Trois procédures nommé  **\<type_name\>_Insérer**,  **\<type_name\>_mettre à jour** et  **\<type_ nom\>_Supprimer** (par exemple, Blog_Insert, Blog_Update et Blog_Delete).  
- Noms de paramètres correspondent aux noms de propriété.  
  > [!NOTE]
  > Si vous utilisez HasColumnName() ou l’attribut de colonne pour renommer la colonne pour une propriété donnée ce nom est utilisé pour les paramètres au lieu du nom de propriété.  
- **La procédure stockée insert** aura un paramètre pour chaque propriété, à l’exception de celles qui sont marquées comme magasin généré (identité ou calculée). La procédure stockée doit retourner un jeu de résultats avec une colonne pour chaque propriété de base généré.  
- **La mise à jour de procédure stockée** aura un paramètre pour chaque propriété, à l’exception de celles qui sont marquées avec un modèle de magasin généré de « Calculé ». Bien que certains jetons d’accès concurrentiel requièrent un paramètre pour la valeur d’origine, consultez la *jetons d’accès concurrentiel* section ci-dessous pour plus d’informations. La procédure stockée doit retourner un jeu de résultats avec une colonne pour chaque propriété calculée.  
- **La suppression d’une procédure stockée** doit avoir un paramètre pour la valeur de clé de l’entité (ou plusieurs paramètres si l’entité a une clé composite). En outre, la procédure de suppression doit également avoir des paramètres pour toutes les clés étrangères association indépendante sur la table cible (les relations qui n’ont pas de propriétés de clé étrangère correspondantes déclarées dans l’entité). Bien que certains jetons d’accès concurrentiel requièrent un paramètre pour la valeur d’origine, consultez la *jetons d’accès concurrentiel* section ci-dessous pour plus d’informations.  

À l’aide de la classe suivante comme exemple :  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

La valeur par défaut des procédures stockées serait :  

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

### <a name="overriding-the-defaults"></a>Substituer les valeurs par défaut  

Vous pouvez remplacer tout ou partie de ce qui a été configuré par défaut.  

Vous pouvez modifier le nom d’une ou plusieurs procédures stockées. Cet exemple renomme la procédure stockée update uniquement.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Cet exemple renomme tous les trois procédures stockées.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

Dans ces exemples, les appels sont chaînées ensemble, mais vous pouvez également utiliser la syntaxe de bloc d’expression lambda.  

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

Cet exemple renomme le paramètre pour la propriété BlogId sur la procédure stockée update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Ces appels sont enchaînées et composable. Voici un exemple qui renomme les trois procédures stockées et leurs paramètres.  

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

Vous pouvez également modifier le nom des colonnes du jeu de résultats qui contient des valeurs de la base de données générée.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relations sans une clé étrangère dans la classe (Associations indépendantes)  

Lorsqu’une propriété de clé étrangère est incluse dans la définition de classe, le paramètre correspondant peut être renommé dans la même façon que toute autre propriété. Lorsqu’il existe une relation sans une propriété de clé étrangère dans la classe, le nom de paramètre par défaut est  **\<navigation_property_name\>_\<primary_key_name\>**.  

Par exemple, les définitions de classe suivantes entraînerait un paramètre Blog_BlogId qui est attendu dans les procédures stockées pour insérer et mettre à jour des publications.  

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

### <a name="overriding-the-defaults"></a>Substituer les valeurs par défaut  

Vous pouvez modifier les paramètres pour les clés étrangères qui ne figurent pas dans la classe en fournissant le chemin d’accès à la propriété de clé primaire à la méthode de paramètre.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Si vous n’avez pas une propriété de navigation sur l’entité dépendante (ex.) aucune propriété Post.Blog) vous pouvez ensuite utiliser la méthode d’Association pour identifier l’autre extrémité de la relation, puis configurer les paramètres qui correspondent à chacune des propriétés de clé ne.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Jetons d'accès concurrentiel  

Mise à jour et supprimer les procédures serez peut-être amené à gérer avec l’accès concurrentiel :  

- Si l’entité contient des jetons d’accès concurrentiel, la procédure stockée peut éventuellement avoir un paramètre de sortie qui retourne le nombre de lignes mises à jour/supprimées (lignes affectées). Un tel paramètre doit être configuré à l’aide de la méthode RowsAffectedParameter.  
Par défaut, EF utilise la valeur de retour à partir de la méthode ExecuteNonQuery pour déterminer combien de lignes ont été affectés. Spécification d’un paramètre de sortie de lignes affectées est utile si vous effectuez une logique dans votre procédure stockée qui entraînerait la valeur de retour de ExecuteNonQuery qui est incorrect (du point de vue d’Entity Framework) à la fin de l’exécution.  
- Pour chaque d’accès concurrentiel, il le jeton sera un paramètre nommé  **\<property_name\>_Original** (par exemple, Timestamp_Original). Cela recevront la valeur d’origine de cette propriété : la valeur lorsqu’il est interrogé à partir de la base de données.  
    - Les jetons d’accès concurrentiel qui sont calculées par la base de données – tels que les horodatages – a uniquement un paramètre de valeur d’origine.  
    - Propriétés non calculées qui sont définies en tant que jetons d’accès concurrentiel aura également un paramètre pour la nouvelle valeur dans la procédure de mise à jour. Cet exemple utilise les conventions d’affectation de noms déjà vu pour les nouvelles valeurs. Un exemple de ce jeton serait être à l’aide URL d’un Blog sous forme d’un jeton d’accès concurrentiel, la nouvelle valeur est requise, car cela peut être mis à jour vers une nouvelle valeur par votre code (contrairement à un jeton de l’horodatage qui est uniquement mis à jour par la base de données).  

Il s’agit d’un exemple de classe et de mettre à jour de la procédure stockée avec un jeton d’accès concurrentiel d’horodatage.  

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

Voici un exemple de classe et de mettre à jour de la procédure stockée avec un jeton d’accès concurrentiel de non calculée.  

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

### <a name="overriding-the-defaults"></a>Substituer les valeurs par défaut  

Vous pouvez éventuellement introduire un paramètre des lignes affectées.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Pour les jetons d’accès concurrentiel calculée de la base de données – où seule la valeur d’origine est passée : vous pouvez simplement utiliser le mécanisme de changement de nom de paramètre standard pour renommer le paramètre pour la valeur d’origine.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Pour les jetons d’accès concurrentiel de non calculée – où est passée à la fois la valeur d’origine et nouvelle – vous pouvez utiliser une surcharge de paramètre qui vous permet de fournir un nom pour chaque paramètre.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Relations plusieurs-à-plusieurs  

Nous allons utiliser les classes suivantes comme exemple dans cette section.  

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

Plusieurs à plusieurs relations peuvent être mappées à des procédures stockées avec la syntaxe suivante.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Si aucune autre configuration n’est pas fournie la forme de procédure stockée suivante est utilisée par défaut.  

- Deux procédures nommés stockées  **\<type_one\>\<type_two\>_Insérer** et  **\<type_one\>\<type_two \>_Supprimer** (par exemple, PostTag_Insert et PostTag_Delete).  
- Les paramètres seront les valeurs de clés pour chaque type. Le nom de chaque paramètre en cours **\<type_name\>_\<property_name\>** (par exemple, Post_PostId et Tag_TagId).

Voici des exemples insérer et mettre à jour des procédures stockées.  

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

### <a name="overriding-the-defaults"></a>Substituer les valeurs par défaut  

Les noms de procédure et le paramètre peuvent être configurés de manière similaire aux procédures de l’entité stockée.  

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
