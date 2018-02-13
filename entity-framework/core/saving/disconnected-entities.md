---
title: "Entités déconnectées - EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a><span data-ttu-id="f1a59-102">Entités déconnectées</span><span class="sxs-lookup"><span data-stu-id="f1a59-102">Disconnected entities</span></span>

<span data-ttu-id="f1a59-103">Une instance de DbContext suivre automatiquement les entités retournées à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="f1a59-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="f1a59-104">Modifications apportées à ces entités puis seront détectées lorsque SaveChanges est appelée et la base de données sera mise à jour en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="f1a59-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="f1a59-105">Consultez [enregistrer base](basic.md) et [les données associées](related-data.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="f1a59-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="f1a59-106">Toutefois, parfois, les entités sont interrogées à l’aide d’une instance de contexte et puis enregistré à l’aide d’une autre instance.</span><span class="sxs-lookup"><span data-stu-id="f1a59-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="f1a59-107">Cela se produit souvent dans les scénarios « déconnectés » telle qu’une application web où les entités sont interrogées, envoyées au client, modifiées, envoyées sur le serveur dans une demande et puis enregistrées.</span><span class="sxs-lookup"><span data-stu-id="f1a59-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="f1a59-108">Dans ce cas, le deuxième contexte d’instance doit savoir si les entités sont nouvelle (doit être inséré) existant (doit être mis à jour).</span><span class="sxs-lookup"><span data-stu-id="f1a59-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="f1a59-109">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="f1a59-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="f1a59-110">EF Core peut suivre seulement une seule instance d’une entité avec une valeur de clé primaire donnée.</span><span class="sxs-lookup"><span data-stu-id="f1a59-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="f1a59-111">La meilleure façon d’éviter ce problème consiste à utiliser un contexte de courte durée de vie pour chaque unité de travail tels que le contexte est vide, au démarrage en cours a entités associées, enregistre ces entités, puis le contexte est supprimé et ignorée.</span><span class="sxs-lookup"><span data-stu-id="f1a59-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="f1a59-112">Identification des entités</span><span class="sxs-lookup"><span data-stu-id="f1a59-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="f1a59-113">Client identifie de nouvelles entités</span><span class="sxs-lookup"><span data-stu-id="f1a59-113">Client identifies new entities</span></span>

<span data-ttu-id="f1a59-114">Le cas le plus simple à gérer est lorsque le client informe le serveur si l’entité est nouveau ou existant.</span><span class="sxs-lookup"><span data-stu-id="f1a59-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="f1a59-115">Par exemple, souvent la demande d’insertion d’une nouvelle entité diffère de la demande pour mettre à jour une entité existante.</span><span class="sxs-lookup"><span data-stu-id="f1a59-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="f1a59-116">Le reste de cette section couvre les cas où il est nécessaire de déterminer, d’une autre manière, s’il faut insérer ou mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="f1a59-117">Avec les clés générées automatiquement</span><span class="sxs-lookup"><span data-stu-id="f1a59-117">With auto-generated keys</span></span>

<span data-ttu-id="f1a59-118">La valeur d’une clé générée automatiquement peut souvent être utilisée pour déterminer si une entité doit être insérée ou mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="f1a59-119">Si la clé n’a pas été défini (autrement dit il toujours a la valeur CLR par défaut null, zéro, etc.), puis l’entité doit être de nouveau et a besoin d’insertion.</span><span class="sxs-lookup"><span data-stu-id="f1a59-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="f1a59-120">En revanche, si la valeur de clé a été définie, puis il doit avoir déjà été précédemment enregistré et doit mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="f1a59-121">En d’autres termes, si la clé a une valeur, puis d’entité a été interrogée, envoyées au client et a maintenant revenir à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="f1a59-122">Il est facile de rechercher une clé non définie lorsque le type d’entité est connu :</span><span class="sxs-lookup"><span data-stu-id="f1a59-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="f1a59-123">Toutefois, EF a également un moyen intégré pour n’importe quel type d’entité et le type de clé :</span><span class="sxs-lookup"><span data-stu-id="f1a59-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="f1a59-124">Les clés sont définies dès que les entités sont suivies par le contexte, même si l’entité est dans l’état Added.</span><span class="sxs-lookup"><span data-stu-id="f1a59-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="f1a59-125">Ainsi, lors du parcours d’un graphique d’entités et de décider quelles actions effectuer avec chaque, telles que lors de l’utilisation de l’API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="f1a59-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="f1a59-126">La valeur de clé doit uniquement être utilisée de la manière illustrée ici _avant_ n’importe quel appel est effectué pour effectuer le suivi de l’entité.</span><span class="sxs-lookup"><span data-stu-id="f1a59-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="f1a59-127">Avec les autres clés</span><span class="sxs-lookup"><span data-stu-id="f1a59-127">With other keys</span></span>

<span data-ttu-id="f1a59-128">Un autre mécanisme est nécessaire pour identifier les nouvelles entités lorsque les valeurs de clés ne sont pas générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f1a59-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="f1a59-129">Il existe deux approches générales pour cela :</span><span class="sxs-lookup"><span data-stu-id="f1a59-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="f1a59-130">Requête pour l’entité</span><span class="sxs-lookup"><span data-stu-id="f1a59-130">Query for the entity</span></span>
 * <span data-ttu-id="f1a59-131">Passez un indicateur à partir du client</span><span class="sxs-lookup"><span data-stu-id="f1a59-131">Pass a flag from the client</span></span>

<span data-ttu-id="f1a59-132">Pour rechercher l’entité, utiliser simplement la méthode de recherche :</span><span class="sxs-lookup"><span data-stu-id="f1a59-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="f1a59-133">N’entre pas dans le cadre de ce document pour afficher le code complet pour le passage d’un indicateur d’un client.</span><span class="sxs-lookup"><span data-stu-id="f1a59-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="f1a59-134">Dans une application web, cela signifie généralement effectuant des demandes différentes pour différentes actions, ou en passant un état dans la demande, puis extraire dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f1a59-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="f1a59-135">L’enregistrement des entités uniques</span><span class="sxs-lookup"><span data-stu-id="f1a59-135">Saving single entities</span></span>

<span data-ttu-id="f1a59-136">S’il est connu ou non une insertion ou une mise à jour est nécessaire, puis ajouter ou mise à jour peut être utilisé de manière appropriée :</span><span class="sxs-lookup"><span data-stu-id="f1a59-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="f1a59-137">Toutefois, si l’entité utilise les valeurs de clés générées automatiquement, la méthode de mise à jour peut être utilisée pour les deux cas :</span><span class="sxs-lookup"><span data-stu-id="f1a59-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="f1a59-138">Normalement, la méthode de mise à jour marque l’entité pour la mise à jour, insertion pas.</span><span class="sxs-lookup"><span data-stu-id="f1a59-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="f1a59-139">Toutefois, si l’entité a une clé générée automatiquement, et aucune valeur de clé n’a été défini, puis l’entité est automatiquement marquée pour insérer.</span><span class="sxs-lookup"><span data-stu-id="f1a59-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="f1a59-140">Ce comportement a été introduit dans EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f1a59-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="f1a59-141">Pour des versions antérieures, il est toujours nécessaire de choisir explicitement ajouter ou mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="f1a59-142">Si l’entité n’utilise pas les clés générées automatiquement, puis l’application doit décider si l’entité doit être insérée ou mise à jour : par exemple :</span><span class="sxs-lookup"><span data-stu-id="f1a59-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="f1a59-143">Les étapes ici sont :</span><span class="sxs-lookup"><span data-stu-id="f1a59-143">The steps here are:</span></span>
* <span data-ttu-id="f1a59-144">Si Find retourne null, la base de données ne contient pas déjà le blog avec cet ID, afin de nous appeler, puis ajoutez le marquer pour l’insertion.</span><span class="sxs-lookup"><span data-stu-id="f1a59-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="f1a59-145">Si la recherche retourne une entité, puis il existe dans la base de données et le contexte suit maintenant l’entité existante</span><span class="sxs-lookup"><span data-stu-id="f1a59-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="f1a59-146">Ensuite, nous utilisons SetValues pour définir les valeurs de toutes les propriétés de cette entité à celles fournies par le client.</span><span class="sxs-lookup"><span data-stu-id="f1a59-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="f1a59-147">L’appel de SetValues marque l’entité à mettre à jour en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="f1a59-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="f1a59-148">SetValues uniquement marque a modifié les propriétés qui ont des valeurs différentes à ceux de l’entité suivie.</span><span class="sxs-lookup"><span data-stu-id="f1a59-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="f1a59-149">Cela signifie que lorsque la mise à jour est envoyée, uniquement les colonnes qui ont été modifiées seront mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="f1a59-150">(Et si rien n’a changé, alors aucune mise à jour ne sera envoyé à tout).</span><span class="sxs-lookup"><span data-stu-id="f1a59-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="f1a59-151">Utilisation des graphiques</span><span class="sxs-lookup"><span data-stu-id="f1a59-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="f1a59-152">Résolution d’identité</span><span class="sxs-lookup"><span data-stu-id="f1a59-152">Identity resolution</span></span>

<span data-ttu-id="f1a59-153">Comme indiqué ci-dessus, EF Core peut suivre uniquement une seule instance d’une entité avec une valeur de clé primaire donnée.</span><span class="sxs-lookup"><span data-stu-id="f1a59-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="f1a59-154">Lorsque vous travaillez avec des graphiques le graphique doit être créé dans l’idéal, telle que cet invariant est géré, et le contexte doit être utilisé pour qu’une seule unité de travail.</span><span class="sxs-lookup"><span data-stu-id="f1a59-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="f1a59-155">Si le graphique ne contient pas les doublons, il sera nécessaire traiter le graphique avant de l’envoyer à EF de consolider plusieurs instances en un seul.</span><span class="sxs-lookup"><span data-stu-id="f1a59-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="f1a59-156">Cela peut être non trivial où les instances ont des valeurs en conflit et les relations, donc les doublons de consolidation doivent être effectuées dès que possible dans le pipeline de votre application afin d’éviter la résolution des conflits.</span><span class="sxs-lookup"><span data-stu-id="f1a59-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="f1a59-157">Toutes les entités de nouveau ou l’ensemble existantes</span><span class="sxs-lookup"><span data-stu-id="f1a59-157">All new/all existing entities</span></span>

<span data-ttu-id="f1a59-158">Un exemple de l’utilisation de graphiques est insertion ou mise à jour d’un blog ainsi que sa collection de messages associés.</span><span class="sxs-lookup"><span data-stu-id="f1a59-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="f1a59-159">Si toutes les entités dans le graphique doivent être insérées, ou tous doivent être mis à jour, le processus est identique à celle décrite ci-dessus pour les entités uniques.</span><span class="sxs-lookup"><span data-stu-id="f1a59-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="f1a59-160">Par exemple, un graphique des blogs et publications créées comme suit :</span><span class="sxs-lookup"><span data-stu-id="f1a59-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="f1a59-161">peut être inséré comme suit :</span><span class="sxs-lookup"><span data-stu-id="f1a59-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="f1a59-162">L’appel Add marque le blog et toutes les publications à insérer.</span><span class="sxs-lookup"><span data-stu-id="f1a59-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="f1a59-163">De même, si toutes les entités dans un graphique doivent être mis à jour, puis Update peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="f1a59-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="f1a59-164">Le blog et toutes ses publications seront marquées pour être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="f1a59-165">Combinaison d’entités nouvelle et existantes</span><span class="sxs-lookup"><span data-stu-id="f1a59-165">Mix of new and existing entities</span></span>

<span data-ttu-id="f1a59-166">Avec les clés générées automatiquement, mise à jour de nouveau utilisable pour les insertions et mises à jour, même si le graphique contient un mélange d’entités qui nécessitent l’insertion et ceux qui nécessitent la mise à jour :</span><span class="sxs-lookup"><span data-stu-id="f1a59-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="f1a59-167">Mise à jour marque une entité dans le graphique, le blog ou post, d’insertion si elle ne dispose pas d’un ensemble de la valeur de clé, tandis que toutes les autres entités sont marquées pour la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="f1a59-168">Comme précédemment, lorsque vous n’utilisez ne pas les clés générées automatiquement, une requête et un traitement peuvent être utilisés :</span><span class="sxs-lookup"><span data-stu-id="f1a59-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="f1a59-169">Gestion des suppressions</span><span class="sxs-lookup"><span data-stu-id="f1a59-169">Handling deletes</span></span>

<span data-ttu-id="f1a59-170">DELETE peut être difficile à gérer depuis souvent l’absence d’une entité signifie qu’il doit être supprimé.</span><span class="sxs-lookup"><span data-stu-id="f1a59-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="f1a59-171">Une façon de gérer cela consiste à utiliser des « suppressions récupérables » telles que l’entité est marquée comme supprimée au lieu d’être supprimées.</span><span class="sxs-lookup"><span data-stu-id="f1a59-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="f1a59-172">Supprime, puis devient le même que les mises à jour.</span><span class="sxs-lookup"><span data-stu-id="f1a59-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="f1a59-173">Suppressions récupérables qui peuvent être implémentées à l’aide de [filtres de requête](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="f1a59-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="f1a59-174">Pour les suppressions de valeur est trues, il est courant d’utiliser une extension du modèle de requête pour effectuer ce qui est essentiellement une comparaison de graphique. Exemple :</span><span class="sxs-lookup"><span data-stu-id="f1a59-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="f1a59-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="f1a59-175">TrackGraph</span></span>

<span data-ttu-id="f1a59-176">En interne, ajouter, attacher et mise à jour utilisent traversée de graphique avec une détermination effectuée pour chaque entité que si elle doit être marquée comme Added (pour insérer), Modified (mettre à jour), Unchanged (ne rien faire), ou supprimés (à supprimer).</span><span class="sxs-lookup"><span data-stu-id="f1a59-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="f1a59-177">Ce mécanisme est exposé via l’API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="f1a59-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="f1a59-178">Par exemple, supposons que lorsque le client envoie un graphique d’entités qu’il définit certains indicateur sur chaque entité indiquant comment il doit être géré.</span><span class="sxs-lookup"><span data-stu-id="f1a59-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="f1a59-179">TrackGraph peut ensuite être utilisée pour traiter cet indicateur :</span><span class="sxs-lookup"><span data-stu-id="f1a59-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="f1a59-180">Les indicateurs sont uniquement affichés dans le cadre de l’entité pour simplifier l’exemple.</span><span class="sxs-lookup"><span data-stu-id="f1a59-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="f1a59-181">En général, les indicateurs serait partie d’un DTO ou d’un autre état inclus dans la demande.</span><span class="sxs-lookup"><span data-stu-id="f1a59-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
