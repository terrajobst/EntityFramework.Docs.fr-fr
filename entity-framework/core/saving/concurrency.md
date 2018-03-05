---
title: "La gestion des conflits d’accès concurrentiel - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="630db-102">Gestion de conflits d'accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="630db-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="630db-103">Cette page décrit le fonctionne de la concurrence dans EF Core et comment gérer les conflits d’accès concurrentiel dans votre application.</span><span class="sxs-lookup"><span data-stu-id="630db-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="630db-104">Consultez [jetons d’accès concurrentiel](xref:core/modeling/concurrency) pour plus d’informations sur la configuration des jetons d’accès concurrentiel dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="630db-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="630db-105">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="630db-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="630db-106">_Concurrence de la base de données_ fait référence aux situations dans lesquelles plusieurs processus ou les utilisateurs accéder ou modifier les mêmes données dans une base de données en même temps.</span><span class="sxs-lookup"><span data-stu-id="630db-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="630db-107">_Contrôle d’accès concurrentiel_ fait référence à des mécanismes spécifiques permettant de garantir la cohérence des données en présence de modifications simultanées.</span><span class="sxs-lookup"><span data-stu-id="630db-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="630db-108">EF Core implémente _contrôle d’accès concurrentiel optimiste_, ce qui signifie qu’il vous permet de plusieurs processus ou utilisateurs apportent des modifications indépendamment sans la surcharge de synchronisation ou de verrouillage.</span><span class="sxs-lookup"><span data-stu-id="630db-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="630db-109">Dans l’idéal, ces modifications n’interfèrent pas avec eux et seront donc en mesure de réussir.</span><span class="sxs-lookup"><span data-stu-id="630db-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="630db-110">Dans le pire des cas, deux ou plusieurs processus va tenter d’apporter des modifications en conflit et seulement un d’eux doit réussir.</span><span class="sxs-lookup"><span data-stu-id="630db-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="630db-111">Fonctionne du contrôle d’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="630db-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="630db-112">Propriétés configurées en tant que jetons d’accès concurrentiel sont utilisés pour implémenter un contrôle d’accès concurrentiel optimiste : chaque fois qu’une opération update ou delete est effectuée pendant `SaveChanges`, la valeur du jeton d’accès concurrentiel sur la base de données est comparée à la version d’origine valeur lue EF Core.</span><span class="sxs-lookup"><span data-stu-id="630db-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="630db-113">Si les valeurs correspondent, l’opération peut s’effectuer.</span><span class="sxs-lookup"><span data-stu-id="630db-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="630db-114">Si les valeurs ne correspondent pas, EF Core suppose qu’un autre utilisateur a effectué une opération conflictuelle et abandonne la transaction en cours.</span><span class="sxs-lookup"><span data-stu-id="630db-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="630db-115">Le cas où un autre utilisateur a effectué une opération qui est en conflit avec l’opération en cours est appelé _conflit d’accès concurrentiel_.</span><span class="sxs-lookup"><span data-stu-id="630db-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="630db-116">Les fournisseurs de base de données sont responsables de l’implémentation de la comparaison des valeurs de jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="630db-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="630db-117">Sur les bases de données relationnelles EF Core inclut une vérification de la valeur du jeton d’accès concurrentiel dans le `WHERE` clause de n’importe quel `UPDATE` ou `DELETE` instructions.</span><span class="sxs-lookup"><span data-stu-id="630db-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="630db-118">Après l’exécution des instructions, EF Core lit le nombre de lignes qui ont été affectés.</span><span class="sxs-lookup"><span data-stu-id="630db-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="630db-119">Si aucune ligne n’est affectée, un conflit d’accès concurrentiel est détecté, et EF Core lève `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="630db-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="630db-120">Par exemple, nous pouvons choisir de configurer `LastName` sur `Person` un jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="630db-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="630db-121">Toute opération de mise à jour sur la personne qui inclut le contrôle d’accès concurrentiel dans le `WHERE` clause :</span><span class="sxs-lookup"><span data-stu-id="630db-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="630db-122">Résolution des conflits d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="630db-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="630db-123">Poursuivre l’exemple précédent, si un utilisateur tente d’enregistrer certaines modifications apportées à un `Person`, mais un autre utilisateur a déjà modifié le `LastName` l’une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="630db-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="630db-124">À ce stade, l’application peut simplement indiquer à l’utilisateur que la mise à jour a échoué en raison de modifications en conflit et déplacer sur.</span><span class="sxs-lookup"><span data-stu-id="630db-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="630db-125">Mais il peut être souhaitable pour inviter l’utilisateur pour s’assurer de que cet enregistrement représente toujours la même personne réelle et recommencez l’opération.</span><span class="sxs-lookup"><span data-stu-id="630db-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="630db-126">Ce processus est un exemple de _résoudre un conflit d’accès concurrentiel_.</span><span class="sxs-lookup"><span data-stu-id="630db-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="630db-127">Résolution d’un conflit d’accès concurrentiel consiste à fusionner les modifications en attente à partir du `DbContext` avec les valeurs dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="630db-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="630db-128">Les valeurs de fusion varie en fonction de l’application et peut être dirigé par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="630db-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="630db-129">**Il existe trois ensembles de valeurs disponibles pour aider à résoudre un conflit d’accès concurrentiel :**</span><span class="sxs-lookup"><span data-stu-id="630db-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="630db-130">**Valeurs actuelles** sont les valeurs que l’application a tenté d’écrire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="630db-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="630db-131">**Les valeurs d’origine** sont les valeurs qui ont été récupérées à l’origine à partir de la base de données, avant que toutes les modifications ont été apportées.</span><span class="sxs-lookup"><span data-stu-id="630db-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="630db-132">**Valeurs de base de données** sont les valeurs actuellement stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="630db-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="630db-133">L’approche générale pour gérer les conflits d’accès concurrentiel est la suivante :</span><span class="sxs-lookup"><span data-stu-id="630db-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="630db-134">Catch `DbUpdateConcurrencyException` pendant `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="630db-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="630db-135">Utilisez `DbUpdateConcurrencyException.Entries` pour préparer un nouvel ensemble de modifications pour les entités concernées.</span><span class="sxs-lookup"><span data-stu-id="630db-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="630db-136">Actualiser les valeurs d’origine du jeton d’accès concurrentiel pour refléter les valeurs actuelles dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="630db-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="630db-137">Recommencez le processus jusqu'à ce qu’aucun conflit se produire.</span><span class="sxs-lookup"><span data-stu-id="630db-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="630db-138">Dans l’exemple suivant, `Person.FirstName` et `Person.LastName` sont configurés en tant que jetons d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="630db-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="630db-139">Il existe un `// TODO:` commentaire à l’emplacement où vous inclure la logique spécifique de l’application pour choisir la valeur doit être enregistré.</span><span class="sxs-lookup"><span data-stu-id="630db-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
