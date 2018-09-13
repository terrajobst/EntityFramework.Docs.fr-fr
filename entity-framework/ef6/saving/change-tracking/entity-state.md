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
# <a name="working-with-entity-states"></a><span data-ttu-id="01efc-102">Utilisation des États des entités</span><span class="sxs-lookup"><span data-stu-id="01efc-102">Working with entity states</span></span>
<span data-ttu-id="01efc-103">Cette section explique comment ajouter et joindre des entités à un contexte et comment Entity Framework traite ces pendant SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="01efc-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="01efc-104">Entity Framework effectue le suivi de l’état des entités pendant qu’ils sont connectés à un contexte, mais dans les scénarios déconnectés ou applications multicouches vous pouvez laisser EF savoir quel état vos entités doit être dans.</span><span class="sxs-lookup"><span data-stu-id="01efc-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="01efc-105">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="01efc-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="01efc-106">États des entités et SaveChanges</span><span class="sxs-lookup"><span data-stu-id="01efc-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="01efc-107">Une entité peut avoir l’un des cinq états tel que défini par l’énumération de l’état d’entité.</span><span class="sxs-lookup"><span data-stu-id="01efc-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="01efc-108">Ces États sont :</span><span class="sxs-lookup"><span data-stu-id="01efc-108">These states are:</span></span>  

- <span data-ttu-id="01efc-109">Ajout : l’entité est suivie par le contexte, mais n’existe pas encore dans la base de données</span><span class="sxs-lookup"><span data-stu-id="01efc-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="01efc-110">Unchanged : l’entité est suivie par le contexte et existe dans la base de données et ses valeurs de propriété n’ont pas changé à partir des valeurs dans la base de données</span><span class="sxs-lookup"><span data-stu-id="01efc-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="01efc-111">Modifié : l’entité est suivie par le contexte et existe dans la base de données, et tout ou partie de ses valeurs de propriété ont été modifiées.</span><span class="sxs-lookup"><span data-stu-id="01efc-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="01efc-112">Supprimé : l’entité est suivie par le contexte et existe dans la base de données, mais a été marquée pour suppression à partir de la base de données lors du prochain qu'appel de SaveChanges</span><span class="sxs-lookup"><span data-stu-id="01efc-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="01efc-113">Détaché : l’entité n'est pas suivie par le contexte</span><span class="sxs-lookup"><span data-stu-id="01efc-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="01efc-114">SaveChanges effectue des opérations différentes pour les entités dans différents états :</span><span class="sxs-lookup"><span data-stu-id="01efc-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="01efc-115">Entités inchangées ne sont pas touchées par SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="01efc-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="01efc-116">Mises à jour ne sont pas envoyées à la base de données pour les entités dans un état Unchanged.</span><span class="sxs-lookup"><span data-stu-id="01efc-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="01efc-117">Ajouté des entités sont insérées dans la base de données et devient inchangées lorsque SaveChanges retourne.</span><span class="sxs-lookup"><span data-stu-id="01efc-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="01efc-118">Entités modifiées sont mises à jour dans la base de données et devenir inchangées lorsque SaveChanges retourne.</span><span class="sxs-lookup"><span data-stu-id="01efc-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="01efc-119">Entités supprimées sont supprimées de la base de données et sont ensuite détachées à partir du contexte.</span><span class="sxs-lookup"><span data-stu-id="01efc-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="01efc-120">Les exemples suivants montrent des façons dans lequel l’état d’une entité ou un graphique d’entité peut être modifié.</span><span class="sxs-lookup"><span data-stu-id="01efc-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="01efc-121">Ajout d’une nouvelle entité au contexte</span><span class="sxs-lookup"><span data-stu-id="01efc-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="01efc-122">Une nouvelle entité peut être ajoutée au contexte en appelant la méthode Add sur DbSet.</span><span class="sxs-lookup"><span data-stu-id="01efc-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="01efc-123">Cela place l’entité dans l’état Added, ce qui signifie qu’elle sera insérée dans la base de données la prochaine fois que SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="01efc-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="01efc-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="01efc-125">Une autre façon d’ajouter une nouvelle entité au contexte consiste à modifier son état Added.</span><span class="sxs-lookup"><span data-stu-id="01efc-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="01efc-126">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="01efc-127">Enfin, vous pouvez ajouter une nouvelle entité au contexte par elle liant à une autre entité qui est déjà suivie.</span><span class="sxs-lookup"><span data-stu-id="01efc-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="01efc-128">Il peut s’agir en ajoutant la nouvelle entité à la propriété de navigation de collection d’une autre entité ou en définissant une propriété de navigation de référence d’une autre entité pour pointer vers la nouvelle entité.</span><span class="sxs-lookup"><span data-stu-id="01efc-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="01efc-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-129">For example:</span></span>  

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

<span data-ttu-id="01efc-130">Notez que, pour tous ces exemples, si l’entité en cours d’ajout a des références à d’autres entités qui ne sont pas encore suivies ensuite ces nouvelles entités est également ajouté au contexte et sera insérée dans la base de données la prochaine fois que SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="01efc-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="01efc-131">Attachement d’une entité existante au contexte</span><span class="sxs-lookup"><span data-stu-id="01efc-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="01efc-132">Si vous avez une entité dont vous savez déjà existe dans la base de données mais qui n’est pas actuellement en cours suivi par le contexte vous pouvez indiquer le contexte pour effectuer le suivi de l’entité à l’aide de la méthode d’attachement sur DbSet.</span><span class="sxs-lookup"><span data-stu-id="01efc-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="01efc-133">L’entité sera à l’état inchangé dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="01efc-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="01efc-134">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="01efc-135">Notez qu’aucune modification n’est apportées à la base de données si SaveChanges est appelée sans effectuer toute autre manipulation de l’entité attachée.</span><span class="sxs-lookup"><span data-stu-id="01efc-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="01efc-136">Il s’agit, car l’entité est à l’état inchangé.</span><span class="sxs-lookup"><span data-stu-id="01efc-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="01efc-137">Une autre façon d’attacher une entité existante au contexte consiste à modifier son état à « Unchanged ».</span><span class="sxs-lookup"><span data-stu-id="01efc-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="01efc-138">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="01efc-139">Notez que pour ces deux exemples si l’entité qui est attachée a des références à d’autres entités qui ne sont pas encore suivies ensuite ces nouvelles entités seront également attachés au contexte à l’état inchangé.</span><span class="sxs-lookup"><span data-stu-id="01efc-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="01efc-140">Attachement d’un existant mais l’entité modifiée au contexte</span><span class="sxs-lookup"><span data-stu-id="01efc-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="01efc-141">Si vous avez une entité dont vous savez déjà existe dans la base de données, mais pour les modifications qui ont été apportées vous pouvez indiquer le contexte pour attacher l’entité et de définir son état sur Modified.</span><span class="sxs-lookup"><span data-stu-id="01efc-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="01efc-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="01efc-143">Lorsque vous modifiez l’état Modified toutes les propriétés de l’entité seront marquées comme modifiée et toutes les valeurs de propriété seront envoyés à la base de données lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="01efc-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="01efc-144">Notez que si l’entité qui est attachée a des références à d’autres entités qui ne sont pas encore suivies, ces nouvelles entités sera attaché au contexte dans un état Unchanged, celles-ci ne sont pas automatiquement converties Modified.</span><span class="sxs-lookup"><span data-stu-id="01efc-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="01efc-145">Si vous disposez de plusieurs entités qui doivent être marqués Modified vous devez définir l’état pour chacune de ces entités individuellement.</span><span class="sxs-lookup"><span data-stu-id="01efc-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="01efc-146">Modification de l’état d’une entité de suivi</span><span class="sxs-lookup"><span data-stu-id="01efc-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="01efc-147">Vous pouvez modifier l’état d’une entité est déjà suivie en définissant la propriété d’état sur son entrée.</span><span class="sxs-lookup"><span data-stu-id="01efc-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="01efc-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-148">For example:</span></span>  

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

<span data-ttu-id="01efc-149">Notez que l’appel Add ou attacher pour une entité qui est déjà suivie peut également servir pour modifier l’état d’entité.</span><span class="sxs-lookup"><span data-stu-id="01efc-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="01efc-150">Par exemple, attacher l’appel d’une entité qui se trouve actuellement dans l’état Added modifie son état à « Unchanged ».</span><span class="sxs-lookup"><span data-stu-id="01efc-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="01efc-151">Insérer ou mettre à jour de modèle</span><span class="sxs-lookup"><span data-stu-id="01efc-151">Insert or update pattern</span></span>  

<span data-ttu-id="01efc-152">Un modèle courant pour certaines applications consiste à ajouter une entité en tant que nouveau (ce qui entraîne l’insertion d’une base de données) ou attacher une entité comme existante et la marquer comme modifié (ce qui entraîne une mise à jour de la base de données) en fonction de la valeur de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="01efc-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="01efc-153">Par exemple, lors de l’utilisation de clés primaires de base de données générée entier, il est courant de traiter une entité avec une clé en tant que nouvelle de zéro et une entité avec une clé différente de zéro comme existant.</span><span class="sxs-lookup"><span data-stu-id="01efc-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="01efc-154">Ce modèle peut être obtenu en définissant l’état d’entité selon une vérification de la valeur de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="01efc-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="01efc-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="01efc-155">For example:</span></span>  

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

<span data-ttu-id="01efc-156">Notez que lorsque vous modifiez l’état Modified toutes les propriétés de l’entité seront marquées comme modifiée et toutes les valeurs de propriété seront envoyés à la base de données lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="01efc-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
