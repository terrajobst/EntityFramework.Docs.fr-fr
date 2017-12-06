---
title: "Jetons d’accès concurrentiel - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="e6435-102">Jetons d'accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="e6435-102">Concurrency Tokens</span></span>

<span data-ttu-id="e6435-103">Si une propriété est configurée comme un jeton d’accès concurrentiel EF Vérifiez qu’aucun autre utilisateur n’a modifié cette valeur dans la base de données lors de l’enregistrement des modifications apportées à cet enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e6435-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="e6435-104">EF utilise un modèle d’accès concurrentiel optimiste, ce qui signifie qu’il sera Supposons que la valeur n’a pas changé et essayez d’enregistrer les données, mais s’il détecte que la valeur a été modifiée de lever.</span><span class="sxs-lookup"><span data-stu-id="e6435-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="e6435-105">Nous pouvons par exemple vouloir configurer `LastName` sur `Person` un jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="e6435-106">Cela signifie que si un utilisateur tente d’enregistrer certaines modifications apportées à un `Person`, mais un autre utilisateur a modifié la `LastName` alors une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="e6435-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="e6435-107">Cela peut être souhaitable afin que votre application peut inviter l’utilisateur pour vous assurer de que cet enregistrement représente toujours la même personne réelle avant d’enregistrer leurs modifications.</span><span class="sxs-lookup"><span data-stu-id="e6435-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="e6435-108">Cette page explique comment configurer des jetons d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="e6435-109">Consultez [concurrence de la gestion des](../saving/concurrency.md) pour obtenir des exemples d’utilisation de l’accès concurrentiel optimiste dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e6435-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="e6435-110">Fonctionnement des jetons d’accès concurrentiel dans EF</span><span class="sxs-lookup"><span data-stu-id="e6435-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="e6435-111">Magasins de données peuvent appliquer des jetons d’accès concurrentiel en vérifiant que tout enregistrement en cours de mise à jour ou supprimé toujours a la même valeur pour le jeton d’accès concurrentiel qui a été attribué lorsque le contexte de chargée des données à l’origine à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6435-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="e6435-112">Par exemple, bases de données relationnelles y parvenir en incluant le jeton d’accès concurrentiel dans le `WHERE` clause de n’importe quel `UPDATE` ou `DELETE` commandes et en vérifiant le nombre de lignes qui ont été affectés.</span><span class="sxs-lookup"><span data-stu-id="e6435-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="e6435-113">Si le jeton d’accès concurrentiel correspond toujours à une ligne mettra à jour.</span><span class="sxs-lookup"><span data-stu-id="e6435-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="e6435-114">Si la valeur dans la base de données a changé, aucune ligne n’est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e6435-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="e6435-115">Conventions</span><span class="sxs-lookup"><span data-stu-id="e6435-115">Conventions</span></span>

<span data-ttu-id="e6435-116">Par convention, les propriétés ne sont jamais configurées en tant que jetons d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e6435-117">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="e6435-117">Data Annotations</span></span>

<span data-ttu-id="e6435-118">Vous pouvez utiliser les Annotations de données pour configurer une propriété comme un jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="e6435-119">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e6435-119">Fluent API</span></span>

<span data-ttu-id="e6435-120">Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="e6435-121">Version de ligne / l’horodatage</span><span class="sxs-lookup"><span data-stu-id="e6435-121">Timestamp/row version</span></span>

<span data-ttu-id="e6435-122">Un horodatage est une propriété où une nouvelle valeur est générée par la base de données chaque fois qu’une ligne est insérée ou mise à jour.</span><span class="sxs-lookup"><span data-stu-id="e6435-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="e6435-123">La propriété est également traitée comme un jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="e6435-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="e6435-124">Ainsi, que vous obtiendrez une exception si un autre utilisateur a modifié une ligne que vous essayez de mettre à jour, car il est interrogée pour les données.</span><span class="sxs-lookup"><span data-stu-id="e6435-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="e6435-125">Cette opération est le fournisseur de base de données utilisé.</span><span class="sxs-lookup"><span data-stu-id="e6435-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="e6435-126">Pour SQL Server, timestamp est généralement utilisé sur un *byte []* propriété, qui sera le programme d’installation en tant qu’un *ROWVERSION* colonne dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6435-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="e6435-127">Conventions</span><span class="sxs-lookup"><span data-stu-id="e6435-127">Conventions</span></span>

<span data-ttu-id="e6435-128">Par convention, les propriétés ne sont jamais configurées en tant que les horodatages.</span><span class="sxs-lookup"><span data-stu-id="e6435-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="e6435-129">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="e6435-129">Data Annotations</span></span>

<span data-ttu-id="e6435-130">Vous pouvez utiliser des Annotations de données pour configurer une propriété comme un horodatage.</span><span class="sxs-lookup"><span data-stu-id="e6435-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="e6435-131">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e6435-131">Fluent API</span></span>

<span data-ttu-id="e6435-132">Vous pouvez utiliser l’API Fluent pour configurer une propriété comme un horodatage.</span><span class="sxs-lookup"><span data-stu-id="e6435-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
