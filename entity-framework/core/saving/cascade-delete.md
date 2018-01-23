---
title: Cascade Delete - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a><span data-ttu-id="926e0-102">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="926e0-102">Cascade Delete</span></span>

<span data-ttu-id="926e0-103">Suppression en cascade est couramment utilisée dans la terminologie de base de données pour décrire une caractéristique qui permet la suppression d’une ligne pour déclencher automatiquement la suppression de lignes connexes.</span><span class="sxs-lookup"><span data-stu-id="926e0-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="926e0-104">Un concept étroitement lié également couvert par les comportements de suppression EF cœur est la suppression automatique d’une entité enfant lorsqu’il est la relation à un parent a été interrompue--cette i communément appelée « suppression des orphelins ».</span><span class="sxs-lookup"><span data-stu-id="926e0-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="926e0-105">EF Core implémente plusieurs comportements de suppression différente et permet la configuration des comportements de suppression de relations individuelles.</span><span class="sxs-lookup"><span data-stu-id="926e0-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="926e0-106">EF Core implémente également des conventions de configurer automatiquement les comportements de suppression par défaut utiles pour chaque relation basée sur le [requiredness de la relation] (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="926e0-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="926e0-107">Supprimer des comportements</span><span class="sxs-lookup"><span data-stu-id="926e0-107">Delete behaviors</span></span>
<span data-ttu-id="926e0-108">Supprimer les comportements sont définis dans le *deleteBehavior()* énumérateur de type et peut être passée à la *OnDelete* API fluent au contrôle si la suppression d’une entité principal/le parent ou l’interruption de la relation à des entités dépendantes/enfant doit avoir un effet secondaire sur les entités dépendantes/enfant.</span><span class="sxs-lookup"><span data-stu-id="926e0-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="926e0-109">Il existe trois actions QU'EF peut prendre lorsqu’un principal/le parent est supprimée ou que la relation à l’enfant est interrompue :</span><span class="sxs-lookup"><span data-stu-id="926e0-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="926e0-110">L’enfant/dépendant peut être supprimé.</span><span class="sxs-lookup"><span data-stu-id="926e0-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="926e0-111">Les valeurs de clé étrangère de l’enfant peuvent être définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="926e0-112">L’enfant reste inchangé</span><span class="sxs-lookup"><span data-stu-id="926e0-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="926e0-113">Le comportement de suppression configuré dans le modèle EF principal est uniquement appliqué lorsque l’entité principale est supprimée à l’aide de EF Core et les entités dépendantes sont chargées en mémoire (par exemple, pour les suivis dépendants).</span><span class="sxs-lookup"><span data-stu-id="926e0-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="926e0-114">Un comportement de cascade correspondant doit se trouver le programme d’installation dans la base de données qui ne sont pas suivies par le contexte a l’action requise est appliquée.</span><span class="sxs-lookup"><span data-stu-id="926e0-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="926e0-115">Si vous utilisez EF Core pour créer la base de données, ce comportement en cascade sera installé pour vous.</span><span class="sxs-lookup"><span data-stu-id="926e0-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="926e0-116">Pour la seconde action ci-dessus, il n’est pas valide si la clé étrangère n’accepte pas d’affectant une valeur de clé étrangère la valeur null.</span><span class="sxs-lookup"><span data-stu-id="926e0-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="926e0-117">(Une clé étrangère non nullable est équivalente à une relation requise.) Dans ces cas, EF Core suit que la propriété de clé étrangère a été marquée comme null jusqu'à ce que SaveChanges est appelée, auquel cas une exception est levée, car la modification ne peut pas être conservée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="926e0-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="926e0-118">Cela est similaire à une violation de contrainte à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="926e0-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="926e0-119">Il existe quatre supprimer des comportements, répertoriés dans les tableaux ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="926e0-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="926e0-120">Pour les relations facultatif (clé étrangère nullable) il _est_ possible d’enregistrer une valeur null valeur de clé étrangère, ce qui entraîne les conséquences suivantes :</span><span class="sxs-lookup"><span data-stu-id="926e0-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="926e0-121">Nom du comportement</span><span class="sxs-lookup"><span data-stu-id="926e0-121">Behavior Name</span></span> | <span data-ttu-id="926e0-122">Effet sur dépendant/enfant dans la mémoire</span><span class="sxs-lookup"><span data-stu-id="926e0-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="926e0-123">Effet sur dépendant/enfant dans la base de données</span><span class="sxs-lookup"><span data-stu-id="926e0-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="926e0-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="926e0-124">**Cascade**</span></span> | <span data-ttu-id="926e0-125">Les entités sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="926e0-125">Entities are deleted</span></span> | <span data-ttu-id="926e0-126">Les entités sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="926e0-126">Entities are deleted</span></span>
| <span data-ttu-id="926e0-127">**ClientSetNull** (par défaut)</span><span class="sxs-lookup"><span data-stu-id="926e0-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="926e0-128">Propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="926e0-129">None</span><span class="sxs-lookup"><span data-stu-id="926e0-129">None</span></span>
| <span data-ttu-id="926e0-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="926e0-130">**SetNull**</span></span> | <span data-ttu-id="926e0-131">Propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="926e0-132">Propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="926e0-133">**Restreindre**</span><span class="sxs-lookup"><span data-stu-id="926e0-133">**Restrict**</span></span> | <span data-ttu-id="926e0-134">None</span><span class="sxs-lookup"><span data-stu-id="926e0-134">None</span></span> | <span data-ttu-id="926e0-135">None</span><span class="sxs-lookup"><span data-stu-id="926e0-135">None</span></span>

<span data-ttu-id="926e0-136">Pour les relations requises (clé étrangère non nullable) il est _pas_ possible d’enregistrer une valeur null valeur de clé étrangère, ce qui entraîne les conséquences suivantes :</span><span class="sxs-lookup"><span data-stu-id="926e0-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="926e0-137">Nom du comportement</span><span class="sxs-lookup"><span data-stu-id="926e0-137">Behavior Name</span></span> | <span data-ttu-id="926e0-138">Effet sur dépendant/enfant dans la mémoire</span><span class="sxs-lookup"><span data-stu-id="926e0-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="926e0-139">Effet sur dépendant/enfant dans la base de données</span><span class="sxs-lookup"><span data-stu-id="926e0-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="926e0-140">**Cascade** (par défaut)</span><span class="sxs-lookup"><span data-stu-id="926e0-140">**Cascade** (Default)</span></span> | <span data-ttu-id="926e0-141">Les entités sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="926e0-141">Entities are deleted</span></span> | <span data-ttu-id="926e0-142">Les entités sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="926e0-142">Entities are deleted</span></span>
| <span data-ttu-id="926e0-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="926e0-143">**ClientSetNull**</span></span> | <span data-ttu-id="926e0-144">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-144">SaveChanges throws</span></span> | <span data-ttu-id="926e0-145">None</span><span class="sxs-lookup"><span data-stu-id="926e0-145">None</span></span>
| <span data-ttu-id="926e0-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="926e0-146">**SetNull**</span></span> | <span data-ttu-id="926e0-147">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-147">SaveChanges throws</span></span> | <span data-ttu-id="926e0-148">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-148">SaveChanges throws</span></span>
| <span data-ttu-id="926e0-149">**Restreindre**</span><span class="sxs-lookup"><span data-stu-id="926e0-149">**Restrict**</span></span> | <span data-ttu-id="926e0-150">None</span><span class="sxs-lookup"><span data-stu-id="926e0-150">None</span></span> | <span data-ttu-id="926e0-151">None</span><span class="sxs-lookup"><span data-stu-id="926e0-151">None</span></span>

<span data-ttu-id="926e0-152">Dans les tableaux ci-dessus, *aucun* peut entraîner une violation de contrainte.</span><span class="sxs-lookup"><span data-stu-id="926e0-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="926e0-153">Par exemple, si une entité principal/enfant est supprimée, mais aucune action n’est effectuée pour modifier la clé étrangère de dépendant/enfant, puis la base de données probablement lève sur SaveChanges en raison d’une violation de contrainte foreign.</span><span class="sxs-lookup"><span data-stu-id="926e0-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="926e0-154">À un niveau élevé :</span><span class="sxs-lookup"><span data-stu-id="926e0-154">At a high level:</span></span>
* <span data-ttu-id="926e0-155">Si vous avez des entités qui ne peut pas exister sans un parent, et vous souhaitez EF pour prendre en charge pour la suppression des enfants automatiquement, puis utilisez *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="926e0-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="926e0-156">Entités qui ne peut pas exister sans un parent Assurez-vous généralement utiliser les relations requises, pour lequel *Cascade* est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="926e0-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="926e0-157">Si vous avez des entités qui peuvent ou ne peuvent pas avoir un parent, et vous souhaitez EF pour prendre en charge de la mise à la clé étrangère pour vous, puis utilisez *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="926e0-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="926e0-158">Entités qui peuvent exister sans un parent Assurez-vous généralement utiliser des relations facultatif, pour lequel *ClientSetNull* est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="926e0-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="926e0-159">Si vous souhaitez que la base de données également essayer de propager les valeurs null pour les clés étrangères enfants même lorsque l’entité enfant n’est pas chargée, puis utilisez *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="926e0-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="926e0-160">Toutefois, notez que la base de données doit prendre en charge cette et configuration de la base de données, comme cela peut entraîner dans d’autres restrictions, qui dans la pratique rend souvent cette option inapproprié.</span><span class="sxs-lookup"><span data-stu-id="926e0-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="926e0-161">C’est pourquoi *SetNull* n’est pas la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="926e0-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="926e0-162">Si vous ne souhaitez pas Core EF jamais supprimer une entité automatiquement ou annuler automatiquement la clé étrangère, puis utiliser *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="926e0-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="926e0-163">Notez que cela nécessite que votre code de synchroniser les entités enfants et leurs valeurs de clé étrangère manuellement dans le cas contraire les exceptions de contrainte seront levées.</span><span class="sxs-lookup"><span data-stu-id="926e0-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="926e0-164">Dans Core EF, contrairement à EF6, effets en cascade ne produisent pas immédiatement, mais à la place uniquement lorsque SaveChanges est appelée.</span><span class="sxs-lookup"><span data-stu-id="926e0-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="926e0-165">**Modifications dans EF Core 2.0 :** dans les versions précédentes, *Restrict* provoquerait des propriétés de clé étrangère facultatif dans des entités dépendantes suivies soit définie sur null et a été le comportement pour les relations de l’optionnels de suppression de la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="926e0-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="926e0-166">Dans EF Core 2.0, le *ClientSetNull* a été introduit pour représenter ce comportement et est devenue la valeur par défaut pour les relations facultatif.</span><span class="sxs-lookup"><span data-stu-id="926e0-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="926e0-167">Le comportement de *Restrict* a été ajusté pour ne jamais avoir d’effets sur les entités dépendantes.</span><span class="sxs-lookup"><span data-stu-id="926e0-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="926e0-168">Exemples de suppression d’entité</span><span class="sxs-lookup"><span data-stu-id="926e0-168">Entity deletion examples</span></span>

<span data-ttu-id="926e0-169">Le code suivant fait partie d’un [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) une exécution qui peut être téléchargé.</span><span class="sxs-lookup"><span data-stu-id="926e0-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="926e0-170">L’exemple montre que se passe-t-il pour chaque comportement de suppression pour les relations facultatives et requises lorsqu’une entité parente est supprimée.</span><span class="sxs-lookup"><span data-stu-id="926e0-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="926e0-171">Nous allons étudier chaque variation de comprendre ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="926e0-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="926e0-172">DeleteBehavior.Cascade avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="926e0-173">Blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="926e0-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="926e0-174">Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="926e0-175">SaveChanges envoie des suppressions pour les dépendances/enfants (publie), puis le principal/parent (blog)</span><span class="sxs-lookup"><span data-stu-id="926e0-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="926e0-176">Après l’enregistrement, toutes les entités sont détachées, car ils ont été supprimées à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="926e0-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="926e0-177">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec relation requise</span><span class="sxs-lookup"><span data-stu-id="926e0-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="926e0-178">Blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="926e0-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="926e0-179">Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="926e0-180">SaveChanges tente de définir la publication FK null, mais cette opération échoue car la clé étrangère ne peut pas être null</span><span class="sxs-lookup"><span data-stu-id="926e0-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="926e0-181">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="926e0-182">Blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="926e0-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="926e0-183">Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="926e0-184">Tentatives de SaveChanges définit la clé étrangère à la fois (publie) / enfants dépendants null avant de supprimer le principal/parent (blog)</span><span class="sxs-lookup"><span data-stu-id="926e0-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="926e0-185">Après l’enregistrement, le principal/parent (blog) est supprimé, mais les objets dépendants/enfants (publie) sont toujours suivies</span><span class="sxs-lookup"><span data-stu-id="926e0-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="926e0-186">Les objets dépendants suivies/enfants (publie) ont maintenant des valeurs FK null et leur référence vers le principal/parents (blog) a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="926e0-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="926e0-187">DeleteBehavior.Restrict avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="926e0-188">Blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="926e0-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="926e0-189">Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges</span><span class="sxs-lookup"><span data-stu-id="926e0-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="926e0-190">Étant donné que *Restrict* indique EF pas automatiquement de définir la clé étrangère sur null, elle reste non null et SaveChanges lève sans enregistrer</span><span class="sxs-lookup"><span data-stu-id="926e0-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="926e0-191">Supprimer les exemples orphelins</span><span class="sxs-lookup"><span data-stu-id="926e0-191">Delete orphans examples</span></span>

<span data-ttu-id="926e0-192">Le code suivant fait partie d’un [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) une exécution qui peut être téléchargé.</span><span class="sxs-lookup"><span data-stu-id="926e0-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="926e0-193">L’exemple montre que se passe-t-il pour chaque comportement de suppression pour les relations facultatives et requises lors de la relation entre un parent/principal et ses éléments enfants/dépendants est interrompue.</span><span class="sxs-lookup"><span data-stu-id="926e0-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="926e0-194">Dans cet exemple, la relation est rompue en supprimant les objets dépendants/enfants (publie) à partir de la propriété de navigation de collection sur le principal/parent (blog).</span><span class="sxs-lookup"><span data-stu-id="926e0-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="926e0-195">Toutefois, le comportement est le même si la référence de dépendant/enfant au principal/le parent est annulée à la place.</span><span class="sxs-lookup"><span data-stu-id="926e0-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="926e0-196">Nous allons étudier chaque variation de comprendre ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="926e0-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="926e0-197">DeleteBehavior.Cascade avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="926e0-198">Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null</span><span class="sxs-lookup"><span data-stu-id="926e0-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="926e0-199">Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="926e0-200">SaveChanges envoie les suppression pour les objets dépendants/enfants (publications)</span><span class="sxs-lookup"><span data-stu-id="926e0-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="926e0-201">Après l’enregistrement, les objets dépendants/enfants (publie) sont détachés, car ils ont été supprimées à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="926e0-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="926e0-202">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec relation requise</span><span class="sxs-lookup"><span data-stu-id="926e0-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="926e0-203">Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null</span><span class="sxs-lookup"><span data-stu-id="926e0-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="926e0-204">Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="926e0-205">SaveChanges tente de définir la publication FK null, mais cette opération échoue car la clé étrangère ne peut pas être null</span><span class="sxs-lookup"><span data-stu-id="926e0-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="926e0-206">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="926e0-207">Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null</span><span class="sxs-lookup"><span data-stu-id="926e0-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="926e0-208">Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="926e0-209">SaveChanges définit la clé étrangère à la fois (publie) / enfants dépendants null</span><span class="sxs-lookup"><span data-stu-id="926e0-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="926e0-210">Une fois que l’enregistrement, les objets dépendants/enfants (publie) ont maintenant des valeurs FK null et leur référence vers le principal/parents (blog) a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="926e0-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="926e0-211">DeleteBehavior.Restrict avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="926e0-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="926e0-212">Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null</span><span class="sxs-lookup"><span data-stu-id="926e0-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="926e0-213">Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="926e0-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="926e0-214">Étant donné que *Restrict* indique EF pas automatiquement de définir la clé étrangère sur null, elle reste non null et SaveChanges lève sans enregistrer</span><span class="sxs-lookup"><span data-stu-id="926e0-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="926e0-215">En cascade pour les entités non chaînées</span><span class="sxs-lookup"><span data-stu-id="926e0-215">Cascading to untracked entities</span></span>

<span data-ttu-id="926e0-216">Lorsque vous appelez *SaveChanges*, les règles de suppression en cascade s’appliquera à toutes les entités qui sont suivies par le contexte.</span><span class="sxs-lookup"><span data-stu-id="926e0-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="926e0-217">C’est le cas dans tous les exemples ci-dessus, c’est pourquoi le SQL généré pour supprimer le principal/parent (blog) et tous les objets dépendants/enfants (publie) :</span><span class="sxs-lookup"><span data-stu-id="926e0-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="926e0-218">Si seul le principal est chargé--par exemple, lorsqu’une requête est faite pour un blog sans un `Include(b => b.Posts)` d’inclure également les publications--puis SaveChanges génère uniquement SQL pour supprimer le principal/parent :</span><span class="sxs-lookup"><span data-stu-id="926e0-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="926e0-219">Les objets dépendants/enfants (publie) être supprimés uniquement si la base de données a un comportement de cascade correspondant configuré.</span><span class="sxs-lookup"><span data-stu-id="926e0-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="926e0-220">Si vous utilisez EF pour créer la base de données, ce comportement en cascade sera installé pour vous.</span><span class="sxs-lookup"><span data-stu-id="926e0-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
