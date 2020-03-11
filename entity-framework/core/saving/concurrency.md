---
title: 'Gestion des conflits d’accès concurrentiel : EF Core'
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417589"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="95226-102">Gestion de conflits d'accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="95226-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="95226-103">Cette page décrit le fonctionnement de l’accès concurrentiel dans EF Core et comment gérer les conflits d’accès concurrentiel dans votre application.</span><span class="sxs-lookup"><span data-stu-id="95226-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="95226-104">Consultez [Jetons d’accès concurrentiel](xref:core/modeling/concurrency) pour plus d’informations sur la configuration des jetons d’accès concurrentiel dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="95226-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="95226-105">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="95226-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="95226-106">_L’accès concurrentiel à la base de données_ fait référence aux situations dans lesquelles plusieurs processus ou utilisateurs accèdent à ou modifient les mêmes données dans une base de données en même temps.</span><span class="sxs-lookup"><span data-stu-id="95226-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="95226-107">Le _Contrôle d’accès concurrentiel_ fait référence à des mécanismes spécifiques permettant de garantir la cohérence des données en présence de modifications simultanées.</span><span class="sxs-lookup"><span data-stu-id="95226-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="95226-108">EF Core implémente un _contrôle d’accès concurrentiel optimiste_, ce qui signifie qu’il permet à plusieurs processus ou utilisateurs d’apporter des modifications indépendamment sans surcharge de synchronisation ou de verrouillage.</span><span class="sxs-lookup"><span data-stu-id="95226-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="95226-109">Dans l’idéal, ces modifications n’interfèrent pas entre elles et sont donc en mesure de réussir.</span><span class="sxs-lookup"><span data-stu-id="95226-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="95226-110">Dans le pire des cas, deux ou processus ou plus tentent d’apporter des modifications en conflit, et seulement un d’eux doit réussir.</span><span class="sxs-lookup"><span data-stu-id="95226-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="95226-111">Fonctionnement du contrôle d’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="95226-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="95226-112">Les propriétés configurées en tant que jetons d’accès concurrentiel sont utilisées pour implémenter un contrôle d’accès concurrentiel optimiste : chaque fois qu’une opération de mise à jour ou de suppression est effectuée pendant `SaveChanges`, la valeur du jeton d’accès concurrentiel sur la base de données est comparée à la valeur d’origine lue par EF Core.</span><span class="sxs-lookup"><span data-stu-id="95226-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="95226-113">Si les valeurs correspondent, l’opération peut s’effectuer.</span><span class="sxs-lookup"><span data-stu-id="95226-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="95226-114">Si les valeurs ne correspondent pas, EF Core suppose qu’un autre utilisateur a effectué une opération conflictuelle et abandonne la transaction en cours.</span><span class="sxs-lookup"><span data-stu-id="95226-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="95226-115">Le cas où un autre utilisateur a effectué une opération en conflit avec l’opération en cours est appelé _Conflit d’accès concurrentiel_.</span><span class="sxs-lookup"><span data-stu-id="95226-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="95226-116">Les fournisseurs de base de données sont responsables de l’implémentation de la comparaison des valeurs de jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="95226-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="95226-117">Sur les bases de données relationnelles, EF Core inclut une vérification de la valeur du jeton d’accès concurrentiel dans la clause `WHERE` de toute instruction `UPDATE` ou `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="95226-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="95226-118">Après l’exécution des instructions, EF Core lit le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95226-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="95226-119">Si aucune ligne n’est affectée, un conflit d’accès concurrentiel est détecté, et EF Core lève `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="95226-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="95226-120">Par exemple, nous pouvons choisir de configurer `LastName` sur `Person` comme jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="95226-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="95226-121">Toute opération de mise à jour sur la personne inclut alors le contrôle d’accès concurrentiel dans la clause `WHERE` :</span><span class="sxs-lookup"><span data-stu-id="95226-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="95226-122">Résolution des conflits d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="95226-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="95226-123">Dans la suite de l’exemple précédent, si un utilisateur tente d’enregistrer certaines modifications apportées à une `Person`, mais qu’un autre utilisateur a déjà modifié le `LastName`, alors une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="95226-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="95226-124">À ce stade, l’application peut simplement indiquer à l’utilisateur que la mise à jour a échoué en raison de modifications en conflit et passer à la suite.</span><span class="sxs-lookup"><span data-stu-id="95226-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="95226-125">Mais il peut être souhaitable d’inviter l’utilisateur à s’assurer que cet enregistrement représente toujours la même personne réelle et à recommencer l’opération.</span><span class="sxs-lookup"><span data-stu-id="95226-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="95226-126">Ce processus est un exemple de _résolution d’un conflit d’accès concurrentiel_.</span><span class="sxs-lookup"><span data-stu-id="95226-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="95226-127">La résolution d’un conflit d’accès concurrentiel consiste à fusionner les modifications en attente à partir du `DbContext` actuel avec les valeurs dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95226-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="95226-128">Les valeurs fusionnées varient en fonction de l’application et peuvent être contrôlées par saisie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="95226-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="95226-129">**Il existe trois ensembles de valeurs disponibles pour résoudre un conflit d’accès concurrentiel :**</span><span class="sxs-lookup"><span data-stu-id="95226-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="95226-130">Les **Valeurs actuelles** sont les valeurs que l’application a tenté d’écrire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95226-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="95226-131">Les **Valeurs d’origine** sont les valeurs qui ont été récupérées à l’origine à partir de la base de données, avant que les modifications soient apportées.</span><span class="sxs-lookup"><span data-stu-id="95226-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="95226-132">Les **Valeurs de base de données** sont les valeurs actuellement stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95226-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="95226-133">L’approche générale pour gérer les conflits d’accès concurrentiel est la suivante :</span><span class="sxs-lookup"><span data-stu-id="95226-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="95226-134">Interceptez `DbUpdateConcurrencyException` pendant `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="95226-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="95226-135">Utilisez `DbUpdateConcurrencyException.Entries` pour préparer un nouvel ensemble de modifications pour les entités concernées.</span><span class="sxs-lookup"><span data-stu-id="95226-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="95226-136">Actualisez les valeurs d’origine du jeton d’accès concurrentiel pour refléter les valeurs actuelles dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95226-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="95226-137">Recommencez le processus jusqu'à ce qu’aucun conflit ne se produise.</span><span class="sxs-lookup"><span data-stu-id="95226-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="95226-138">Dans l’exemple suivant, `Person.FirstName` et `Person.LastName` sont configurés en tant que jetons d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="95226-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="95226-139">Il existe un commentaire `// TODO:` à l’emplacement où vous incluez la logique spécifique de l’application pour choisir la valeur à enregistrer.</span><span class="sxs-lookup"><span data-stu-id="95226-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
