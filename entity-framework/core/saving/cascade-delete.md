---
title: 'Suppression en cascade : EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417613"
---
# <a name="cascade-delete"></a><span data-ttu-id="97450-102">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="97450-102">Cascade Delete</span></span>

<span data-ttu-id="97450-103">La suppression en cascade est couramment utilisée dans la terminologie de base de données pour décrire une caractéristique qui permet à la suppression d’une ligne de déclencher automatiquement la suppression de lignes associées.</span><span class="sxs-lookup"><span data-stu-id="97450-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="97450-104">Un concept étroitement lié également couvert par les comportements de suppression d’EF cœur est la suppression automatique d’une entité enfant lorsque sa relation à un parent a été interrompue. Cela est communément appelé « suppression des orphelins ».</span><span class="sxs-lookup"><span data-stu-id="97450-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="97450-105">EF Core implémente plusieurs comportements de suppression différents et permet la configuration de comportements de suppression de relations individuelles.</span><span class="sxs-lookup"><span data-stu-id="97450-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="97450-106">EF Core implémente également des conventions qui configurent automatiquement les comportements de suppression par défaut utiles pour chaque relation en fonction de la [nécessité de la relation](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="97450-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="97450-107">Comportements de suppression</span><span class="sxs-lookup"><span data-stu-id="97450-107">Delete behaviors</span></span>

<span data-ttu-id="97450-108">Les comportements de suppression sont définis dans le type énumérateur *deleteBehavior()* et peuvent être passés à l’API Fluent *OnDelete* pour contrôler si la suppression d’une entité principale/parente ou l’interruption de la relation avec les entités dépendantes/enfant doit avoir un effet secondaire sur les entités dépendantes/enfant.</span><span class="sxs-lookup"><span data-stu-id="97450-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="97450-109">Il existe trois mesures qu'EF peut prendre lorsqu’une entité principale/parent est supprimée ou que la relation avec l’enfant est interrompue :</span><span class="sxs-lookup"><span data-stu-id="97450-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="97450-110">L’entité dépendante/enfant peut être supprimée</span><span class="sxs-lookup"><span data-stu-id="97450-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="97450-111">Les valeurs de clé étrangère de l’enfant peuvent être définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="97450-112">L’enfant reste inchangé</span><span class="sxs-lookup"><span data-stu-id="97450-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="97450-113">Le comportement de suppression configuré dans le modèle EF Core est uniquement appliqué lorsque l’entité principale est supprimée à l’aide d’EF Core et que les entités dépendantes sont chargées en mémoire (par exemple, pour les suivis dépendants).</span><span class="sxs-lookup"><span data-stu-id="97450-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="97450-114">Un comportement de cascade correspondant doit être configuré dans la base de données pour s’assurer que les données qui ne sont pas suivies par le contexte disposent de la bonne action appliquée.</span><span class="sxs-lookup"><span data-stu-id="97450-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="97450-115">Si vous utilisez EF Core pour créer la base de données, ce comportement en cascade sera configuré pour vous.</span><span class="sxs-lookup"><span data-stu-id="97450-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="97450-116">Pour la deuxième action ci-dessus, la définition d’une clé étrangère sur la valeur null n’est pas valide si cette clé étrangère n’accepte pas la valeur null.</span><span class="sxs-lookup"><span data-stu-id="97450-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="97450-117">(Une clé étrangère non annulable équivaut à une relation requise.) Dans ces cas, EF Core suit que la propriété clé étrangère a été marquée comme nulle jusqu’à ce que SaveChanges soit appelé, à ce moment-là une exception est jetée parce que le changement ne peut pas être persisté à la base de données.</span><span class="sxs-lookup"><span data-stu-id="97450-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="97450-118">Cela est similaire à une violation de contrainte de la base de données.</span><span class="sxs-lookup"><span data-stu-id="97450-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="97450-119">Il existe quatre comportements de suppression, répertoriés dans les tableaux ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="97450-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="97450-120">Relations facultatives</span><span class="sxs-lookup"><span data-stu-id="97450-120">Optional relationships</span></span>

<span data-ttu-id="97450-121">Pour les relations facultatives (clé étrangère acceptant la valeur null) il _est_ possible d’enregistrer une valeur null de clé étrangère, ce qui entraîne les conséquences suivantes :</span><span class="sxs-lookup"><span data-stu-id="97450-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="97450-122">Nom du comportement</span><span class="sxs-lookup"><span data-stu-id="97450-122">Behavior Name</span></span>               | <span data-ttu-id="97450-123">Effet sur les entités dépendantes/enfant en mémoire</span><span class="sxs-lookup"><span data-stu-id="97450-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="97450-124">Effet sur les entités dépendantes/enfant dans la base de données</span><span class="sxs-lookup"><span data-stu-id="97450-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="97450-125">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="97450-125">**Cascade**</span></span>                 | <span data-ttu-id="97450-126">Les entités sont supprimées</span><span class="sxs-lookup"><span data-stu-id="97450-126">Entities are deleted</span></span>                   | <span data-ttu-id="97450-127">Les entités sont supprimées</span><span class="sxs-lookup"><span data-stu-id="97450-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="97450-128">**ClientSetNull** (par défaut)</span><span class="sxs-lookup"><span data-stu-id="97450-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="97450-129">Les propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="97450-130">None</span><span class="sxs-lookup"><span data-stu-id="97450-130">None</span></span>                                   |
| <span data-ttu-id="97450-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="97450-131">**SetNull**</span></span>                 | <span data-ttu-id="97450-132">Les propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="97450-133">Les propriétés de clé étrangère sont définies avec la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="97450-134">**Restreindre**</span><span class="sxs-lookup"><span data-stu-id="97450-134">**Restrict**</span></span>                | <span data-ttu-id="97450-135">None</span><span class="sxs-lookup"><span data-stu-id="97450-135">None</span></span>                                   | <span data-ttu-id="97450-136">None</span><span class="sxs-lookup"><span data-stu-id="97450-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="97450-137">Relations requises</span><span class="sxs-lookup"><span data-stu-id="97450-137">Required relationships</span></span>

<span data-ttu-id="97450-138">Pour les relations requises (clé étrangère n’acceptant pas la valeur null) il _n’est pas_ possible d’enregistrer une valeur null de clé étrangère, ce qui entraîne les conséquences suivantes :</span><span class="sxs-lookup"><span data-stu-id="97450-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="97450-139">Nom du comportement</span><span class="sxs-lookup"><span data-stu-id="97450-139">Behavior Name</span></span>         | <span data-ttu-id="97450-140">Effet sur les entités dépendantes/enfant en mémoire</span><span class="sxs-lookup"><span data-stu-id="97450-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="97450-141">Effet sur les entités dépendantes/enfant dans la base de données</span><span class="sxs-lookup"><span data-stu-id="97450-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="97450-142">**Cascade** (par défaut)</span><span class="sxs-lookup"><span data-stu-id="97450-142">**Cascade** (Default)</span></span> | <span data-ttu-id="97450-143">Les entités sont supprimées</span><span class="sxs-lookup"><span data-stu-id="97450-143">Entities are deleted</span></span>                | <span data-ttu-id="97450-144">Les entités sont supprimées</span><span class="sxs-lookup"><span data-stu-id="97450-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="97450-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="97450-145">**ClientSetNull**</span></span>     | <span data-ttu-id="97450-146">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-146">SaveChanges throws</span></span>                  | <span data-ttu-id="97450-147">None</span><span class="sxs-lookup"><span data-stu-id="97450-147">None</span></span>                                  |
| <span data-ttu-id="97450-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="97450-148">**SetNull**</span></span>           | <span data-ttu-id="97450-149">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-149">SaveChanges throws</span></span>                  | <span data-ttu-id="97450-150">Lève une exception SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="97450-151">**Restreindre**</span><span class="sxs-lookup"><span data-stu-id="97450-151">**Restrict**</span></span>          | <span data-ttu-id="97450-152">None</span><span class="sxs-lookup"><span data-stu-id="97450-152">None</span></span>                                | <span data-ttu-id="97450-153">None</span><span class="sxs-lookup"><span data-stu-id="97450-153">None</span></span>                                  |

<span data-ttu-id="97450-154">Dans les tableaux ci-dessus, *Aucun* peut entraîner une violation de contrainte.</span><span class="sxs-lookup"><span data-stu-id="97450-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="97450-155">Par exemple, si une entité principale/enfant est supprimée, mais qu’aucune action n’est effectuée pour modifier la clé étrangère d’une entité dépendante/enfant, la base de données lèvera probablement une exception sur SaveChanges en raison d’une violation de contrainte étrangère.</span><span class="sxs-lookup"><span data-stu-id="97450-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="97450-156">À un niveau élevé :</span><span class="sxs-lookup"><span data-stu-id="97450-156">At a high level:</span></span>

* <span data-ttu-id="97450-157">Si vous avez des entités qui ne peuvent pas exister sans un parent, et que vous souhaitez qu’EF se charge de la suppression des enfants automatiquement, utilisez *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="97450-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="97450-158">Les entités qui ne peuvent pas exister sans un parent utilisent généralement les relations requises, dont *Cascade* est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="97450-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="97450-159">Si vous avez des entités qui peuvent ou ne peuvent pas avoir un parent, et que vous souhaitez qu’EF prenne en charge la définition de la clé étrangère sur null pour vous, utilisez *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="97450-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="97450-160">Les entités qui peuvent exister sans un parent utilisent généralement les relations optionnelles, dont *ClientSetNull* est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="97450-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="97450-161">Si vous souhaitez que la base de données essaye également de propager les valeurs null aux clés étrangères enfant même lorsque l’entité enfant n’est pas chargée, utilisez *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="97450-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="97450-162">Toutefois, notez que la base de données doit prendre en charge cela, et que la configuration de la base de données de cette façon peut entraîner d’autres restrictions, qui dans la pratique rendent souvent cette option inappropriée.</span><span class="sxs-lookup"><span data-stu-id="97450-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="97450-163">C’est pourquoi *SetNull* n’est pas la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="97450-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="97450-164">Si vous souhaitez qu’EF Core ne supprime jamais une entité automatiquement ou règle automatiquement la clé étrangère sur null, utilisez *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="97450-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="97450-165">Notez que dans ce cas votre code doit synchroniser les entités enfant et leurs valeurs de clé étrangère manuellement. Dans le cas contraire des exceptions de contrainte seront levées.</span><span class="sxs-lookup"><span data-stu-id="97450-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="97450-166">Dans EF Core, contrairement à EF6, les effets en cascade ne se produisent pas immédiatement, mais à la place uniquement lorsque SaveChanges est appelé.</span><span class="sxs-lookup"><span data-stu-id="97450-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="97450-167">**Changements dans EF Core 2.0 :** dans les versions précédentes, *Restrict* provoquerait la définition des propriétés de clé étrangère facultatives dans des entités dépendantes suivies sur null. C’était le comportement de suppression par défaut pour les relations optionnelles.</span><span class="sxs-lookup"><span data-stu-id="97450-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="97450-168">Dans EF Core 2.0, l’option *ClientSetNull* a été introduite pour représenter ce comportement et est devenue la valeur par défaut pour les relations facultatives.</span><span class="sxs-lookup"><span data-stu-id="97450-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="97450-169">Le comportement de *Restrict* a été ajusté pour ne jamais avoir d’effets sur les entités dépendantes.</span><span class="sxs-lookup"><span data-stu-id="97450-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="97450-170">Exemples de suppression d’entité</span><span class="sxs-lookup"><span data-stu-id="97450-170">Entity deletion examples</span></span>

<span data-ttu-id="97450-171">Le code suivant fait partie d’un [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) qui peut être téléchargé et exécuté.</span><span class="sxs-lookup"><span data-stu-id="97450-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="97450-172">L’exemple montre ce qui se passe pour chaque comportement de suppression pour les relations facultatives et requises lorsqu’une entité parente est supprimée.</span><span class="sxs-lookup"><span data-stu-id="97450-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="97450-173">Nous allons étudier chaque variation de comprendre ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="97450-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="97450-174">DeleteBehavior.Cascade avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="97450-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="97450-175">Le blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="97450-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="97450-176">Les billets restent initialement inchangés étant donné que les cascades ne se produisent pas avant SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="97450-177">SaveChanges envoie des suppressions pour les entités dépendantes/enfant (les billets), puis pour l’entité principale/parent (le blog)</span><span class="sxs-lookup"><span data-stu-id="97450-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="97450-178">Après l’enregistrement, toutes les entités sont détachées, car elles ont été supprimées de la base de données</span><span class="sxs-lookup"><span data-stu-id="97450-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="97450-179">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation requise</span><span class="sxs-lookup"><span data-stu-id="97450-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="97450-180">Le blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="97450-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="97450-181">Les billets restent initialement inchangés étant donné que les cascades ne se produisent pas avant SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="97450-182">SaveChanges tente de définir la clé étrangère du billet sur null, mais cette opération échoue car la clé étrangère ne peut pas avoir la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="97450-183">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative</span><span class="sxs-lookup"><span data-stu-id="97450-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="97450-184">Le blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="97450-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="97450-185">Les billets restent initialement inchangés étant donné que les cascades ne se produisent pas avant SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="97450-186">SaveChanges tente de définir clé étrangère des entités dépendantes/enfant (les billets) sur null avant de supprimer l’entité principale/parent (le blog)</span><span class="sxs-lookup"><span data-stu-id="97450-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="97450-187">Après l’enregistrement, l’entité principale/parent (le blog) est supprimée, mais les entités dépendantes/enfant (les billets) sont toujours suivies</span><span class="sxs-lookup"><span data-stu-id="97450-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="97450-188">Les entités dépendantes/enfant suivies (les billets) ont maintenant des valeurs de clé étrangère null et leur référence à l’entité principale/parent (le blog) a été supprimée</span><span class="sxs-lookup"><span data-stu-id="97450-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="97450-189">DeleteBehavior.Restrict avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="97450-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="97450-190">Le blog est marqué comme supprimé</span><span class="sxs-lookup"><span data-stu-id="97450-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="97450-191">Les billets restent initialement inchangés étant donné que les cascades ne se produisent pas avant SaveChanges</span><span class="sxs-lookup"><span data-stu-id="97450-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="97450-192">Étant donné que *Restrict* indique à EF de ne pas automatiquement définir la clé étrangère sur null, elle reste non null et SaveChanges lève une exception sans enregistrer</span><span class="sxs-lookup"><span data-stu-id="97450-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="97450-193">Exemples de suppression d’orphelins</span><span class="sxs-lookup"><span data-stu-id="97450-193">Delete orphans examples</span></span>

<span data-ttu-id="97450-194">Le code suivant fait partie d’un [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) qui peut être téléchargé et exécuté.</span><span class="sxs-lookup"><span data-stu-id="97450-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="97450-195">L’exemple montre ce qui se passe pour chaque comportement de suppression pour les relations facultatives et requises lorsque la relation entre une entité principale/parent et ses entités dépendantes/enfant est interrompue.</span><span class="sxs-lookup"><span data-stu-id="97450-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="97450-196">Dans cet exemple, la relation est rompue en supprimant les entités dépendantes/enfant (les billets) de la propriété de navigation de collection sur l’entité principale/parent (le blog).</span><span class="sxs-lookup"><span data-stu-id="97450-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="97450-197">Toutefois, le comportement est le même si la référence de l’entité dépendante/enfant vers l’entité principale/parent est annulée à la place.</span><span class="sxs-lookup"><span data-stu-id="97450-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="97450-198">Nous allons étudier chaque variation de comprendre ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="97450-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="97450-199">DeleteBehavior.Cascade avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="97450-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="97450-200">Les billets sont marqués comme modifiés, car l’interruption de la relation a provoqué la définition de la clé étrangère sur null</span><span class="sxs-lookup"><span data-stu-id="97450-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="97450-201">Si la clé étrangère ne peut pas être null, la valeur réelle ne changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="97450-202">SaveChanges envoie les suppressions pour les entités dépendantes/enfant (les billets)</span><span class="sxs-lookup"><span data-stu-id="97450-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="97450-203">Après l’enregistrement, les entités dépendantes/enfant (les billets) sont détachées, car elles ont été supprimées de la base de données</span><span class="sxs-lookup"><span data-stu-id="97450-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="97450-204">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation requise</span><span class="sxs-lookup"><span data-stu-id="97450-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="97450-205">Les billets sont marqués comme modifiés, car l’interruption de la relation a provoqué la définition de la clé étrangère sur null</span><span class="sxs-lookup"><span data-stu-id="97450-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="97450-206">Si la clé étrangère ne peut pas être null, la valeur réelle ne changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="97450-207">SaveChanges tente de définir la clé étrangère du billet sur null, mais cette opération échoue car la clé étrangère ne peut pas avoir la valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="97450-208">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative</span><span class="sxs-lookup"><span data-stu-id="97450-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="97450-209">Les billets sont marqués comme modifiés, car l’interruption de la relation a provoqué la définition de la clé étrangère sur null</span><span class="sxs-lookup"><span data-stu-id="97450-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="97450-210">Si la clé étrangère ne peut pas être null, la valeur réelle ne changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="97450-211">SaveChanges définit la clé étrangère des entités dépendantes/enfant (billets) sur null</span><span class="sxs-lookup"><span data-stu-id="97450-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="97450-212">Après enregistrement, les entités dépendantes/enfant suivies (les billets) ont maintenant des valeurs de clé étrangère null et leur référence à l’entité principale/parent (le blog) a été supprimée</span><span class="sxs-lookup"><span data-stu-id="97450-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="97450-213">DeleteBehavior.Restrict avec une relation obligatoire ou facultative</span><span class="sxs-lookup"><span data-stu-id="97450-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="97450-214">Les billets sont marqués comme modifiés, car l’interruption de la relation a provoqué la définition de la clé étrangère sur null</span><span class="sxs-lookup"><span data-stu-id="97450-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="97450-215">Si la clé étrangère ne peut pas être null, la valeur réelle ne changera pas même si elle est marquée en tant que valeur null</span><span class="sxs-lookup"><span data-stu-id="97450-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="97450-216">Étant donné que *Restrict* indique à EF de ne pas automatiquement définir la clé étrangère sur null, elle reste non null et SaveChanges lève une exception sans enregistrer</span><span class="sxs-lookup"><span data-stu-id="97450-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="97450-217">Cascade pour les entités non suivies</span><span class="sxs-lookup"><span data-stu-id="97450-217">Cascading to untracked entities</span></span>

<span data-ttu-id="97450-218">Lorsque vous appelez *SaveChanges*, les règles de suppression en cascade s’appliqueront à toutes les entités qui sont suivies par le contexte.</span><span class="sxs-lookup"><span data-stu-id="97450-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="97450-219">C’est le cas dans tous les exemples ci-dessus, c’est pourquoi le SQL généré pour supprimer l’entité principale/parent (le blog) et toutes les entités dépendantes/enfant (les billets) :</span><span class="sxs-lookup"><span data-stu-id="97450-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="97450-220">Si seule l’entité principale est chargée, par exemple lorsqu’une requête est faite sur un blog sans `Include(b => b.Posts)` pour inclure également les billets, alors SaveChanges génère uniquement le SQL pour supprimer l’entité principale/parent :</span><span class="sxs-lookup"><span data-stu-id="97450-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="97450-221">Les entités dépendantes/enfant (les billets) seront supprimées uniquement si la base de données a un comportement de cascade correspondant configuré.</span><span class="sxs-lookup"><span data-stu-id="97450-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="97450-222">Si vous utilisez EF pour créer la base de données, ce comportement en cascade sera configuré pour vous.</span><span class="sxs-lookup"><span data-stu-id="97450-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
