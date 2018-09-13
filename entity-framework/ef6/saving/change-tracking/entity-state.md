---
title: Utilisation des États de l’entité - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490642"
---
# <a name="working-with-entity-states"></a>Utilisation des États des entités
Cette section explique comment ajouter et joindre des entités à un contexte et comment Entity Framework traite ces pendant SaveChanges.
Entity Framework effectue le suivi de l’état des entités pendant qu’ils sont connectés à un contexte, mais dans les scénarios déconnectés ou applications multicouches vous pouvez laisser EF savoir quel état vos entités doit être dans.
Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

## <a name="entity-states-and-savechanges"></a>États des entités et SaveChanges

Une entité peut avoir l’un des cinq états tel que défini par l’énumération de l’état d’entité. Ces États sont :  

- Ajout : l’entité est suivie par le contexte, mais n’existe pas encore dans la base de données  
- Unchanged : l’entité est suivie par le contexte et existe dans la base de données et ses valeurs de propriété n’ont pas changé à partir des valeurs dans la base de données  
- Modifié : l’entité est suivie par le contexte et existe dans la base de données, et tout ou partie de ses valeurs de propriété ont été modifiées.  
- Supprimé : l’entité est suivie par le contexte et existe dans la base de données, mais a été marquée pour suppression à partir de la base de données lors du prochain qu'appel de SaveChanges  
- Détaché : l’entité n'est pas suivie par le contexte  

SaveChanges effectue des opérations différentes pour les entités dans différents états :  

- Entités inchangées ne sont pas touchées par SaveChanges. Mises à jour ne sont pas envoyées à la base de données pour les entités dans un état Unchanged.  
- Ajouté des entités sont insérées dans la base de données et devient inchangées lorsque SaveChanges retourne.  
- Entités modifiées sont mises à jour dans la base de données et devenir inchangées lorsque SaveChanges retourne.  
- Entités supprimées sont supprimées de la base de données et sont ensuite détachées à partir du contexte.  

Les exemples suivants montrent des façons dans lequel l’état d’une entité ou un graphique d’entité peut être modifié.  

## <a name="adding-a-new-entity-to-the-context"></a>Ajout d’une nouvelle entité au contexte  

Une nouvelle entité peut être ajoutée au contexte en appelant la méthode Add sur DbSet.
Cela place l’entité dans l’état Added, ce qui signifie qu’elle sera insérée dans la base de données la prochaine fois que SaveChanges est appelée.
Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Une autre façon d’ajouter une nouvelle entité au contexte consiste à modifier son état Added. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Enfin, vous pouvez ajouter une nouvelle entité au contexte par elle liant à une autre entité qui est déjà suivie.
Il peut s’agir en ajoutant la nouvelle entité à la propriété de navigation de collection d’une autre entité ou en définissant une propriété de navigation de référence d’une autre entité pour pointer vers la nouvelle entité. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Notez que, pour tous ces exemples, si l’entité en cours d’ajout a des références à d’autres entités qui ne sont pas encore suivies ensuite ces nouvelles entités est également ajouté au contexte et sera insérée dans la base de données la prochaine fois que SaveChanges est appelée.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Attachement d’une entité existante au contexte  

Si vous avez une entité dont vous savez déjà existe dans la base de données mais qui n’est pas actuellement en cours suivi par le contexte vous pouvez indiquer le contexte pour effectuer le suivi de l’entité à l’aide de la méthode d’attachement sur DbSet. L’entité sera à l’état inchangé dans le contexte. Exemple :  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Notez qu’aucune modification n’est apportées à la base de données si SaveChanges est appelée sans effectuer toute autre manipulation de l’entité attachée. Il s’agit, car l’entité est à l’état inchangé.  

Une autre façon d’attacher une entité existante au contexte consiste à modifier son état à « Unchanged ». Exemple :  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Notez que pour ces deux exemples si l’entité qui est attachée a des références à d’autres entités qui ne sont pas encore suivies ensuite ces nouvelles entités seront également attachés au contexte à l’état inchangé.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Attachement d’un existant mais l’entité modifiée au contexte  

Si vous avez une entité dont vous savez déjà existe dans la base de données, mais pour les modifications qui ont été apportées vous pouvez indiquer le contexte pour attacher l’entité et de définir son état sur Modified.
Exemple :  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Lorsque vous modifiez l’état Modified toutes les propriétés de l’entité seront marquées comme modifiée et toutes les valeurs de propriété seront envoyés à la base de données lorsque SaveChanges est appelée.  

Notez que si l’entité qui est attachée a des références à d’autres entités qui ne sont pas encore suivies, ces nouvelles entités sera attaché au contexte dans un état Unchanged, celles-ci ne sont pas automatiquement converties Modified.
Si vous disposez de plusieurs entités qui doivent être marqués Modified vous devez définir l’état pour chacune de ces entités individuellement.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Modification de l’état d’une entité de suivi  

Vous pouvez modifier l’état d’une entité est déjà suivie en définissant la propriété d’état sur son entrée. Exemple :  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Notez que l’appel Add ou attacher pour une entité qui est déjà suivie peut également servir pour modifier l’état d’entité. Par exemple, attacher l’appel d’une entité qui se trouve actuellement dans l’état Added modifie son état à « Unchanged ».  

## <a name="insert-or-update-pattern"></a>Insérer ou mettre à jour de modèle  

Un modèle courant pour certaines applications consiste à ajouter une entité en tant que nouveau (ce qui entraîne l’insertion d’une base de données) ou attacher une entité comme existante et la marquer comme modifié (ce qui entraîne une mise à jour de la base de données) en fonction de la valeur de la clé primaire.
Par exemple, lors de l’utilisation de clés primaires de base de données générée entier, il est courant de traiter une entité avec une clé en tant que nouvelle de zéro et une entité avec une clé différente de zéro comme existant.
Ce modèle peut être obtenu en définissant l’état d’entité selon une vérification de la valeur de clé primaire. Exemple :  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Notez que lorsque vous modifiez l’état Modified toutes les propriétés de l’entité seront marquées comme modifiée et toutes les valeurs de propriété seront envoyés à la base de données lorsque SaveChanges est appelée.  
