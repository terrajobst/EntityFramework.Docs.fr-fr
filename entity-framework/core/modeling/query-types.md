---
title: "Types de requêtes - EF Core"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: d03c4b1d5635530e63b93e051cb69583718deb4e
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="query-types"></a><span data-ttu-id="ca0e6-102">Types de requêtes</span><span class="sxs-lookup"><span data-stu-id="ca0e6-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="ca0e6-103">Cette fonctionnalité est une nouveauté dans EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ca0e6-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="ca0e6-104">Types de requêtes sont des types de résultats de requête en lecture seule qui peuvent être ajoutés au modèle EF principal.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="ca0e6-105">Types de requêtes activer l’interrogation d’ad hoc (par exemple, les types anonymes), mais sont plus flexibles, car ils peuvent avoir la configuration de mappage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="ca0e6-106">Ils sont conceptuellement semblables aux Types d’entités qui :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="ca0e6-107">Ils sont des types POCO c# qui sont ajoutés au modèle, soit dans ```OnModelCreating``` à l’aide de la ```ModelBuilder.Query``` (méthode), ou via une propriété DbContext « set » (pour les types de requête une telle propriété est typée en tant que ```DbQuery<T>``` plutôt que ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="ca0e6-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="ca0e6-108">Ils prennent en charge une grande partie des mêmes fonctionnalités de mappage en tant que types d’entité normale.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="ca0e6-109">Par exemple, mappage d’héritage navigations (voir limitiations ci-dessous) et, sur des magasins relationnels, la possibilité de configurer les objets de schéma de base de données cible via ```ToTable```, ```HasColumn``` les méthodes api fluent (ou des annotations de données).</span><span class="sxs-lookup"><span data-stu-id="ca0e6-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="ca0e6-110">Types de requêtes sont différents d’entité types dans ce qu’ils :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="ca0e6-111">Ne nécessitent pas une clé à définir.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="ca0e6-112">Ne sont jamais suivies par le dispositif de suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="ca0e6-113">Ne sont jamais détectés par convention.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="ca0e6-114">Prennent en charge uniquement un sous-ensemble des fonctionnalités de mappage de navigation - spécifiquement, ils ne peuvent jamais agir en tant que l’extrémité principale d’une relation.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="ca0e6-115">Peut être mappé à un _définition de requête_ -une requête est une requête secondaire qui fait Office de source de données pour un Type de requête.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="ca0e6-116">Parmi les principaux scénarios d’utilisation pour les types de requêtes, citons :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="ca0e6-117">Mappage de vues de base de données.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-117">Mapping to database views.</span></span>
- <span data-ttu-id="ca0e6-118">Mappage des tables qui n’ont pas de clé primaire définie.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="ca0e6-119">Agissant en tant que type de retour pour ad hoc ```FromSql()``` requêtes.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="ca0e6-120">Mappage pour les requêtes définies dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="ca0e6-121">Mappage d’un type de requête à une vue de base de données est obtenue grâce à la ```ToTable``` API fluent.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="ca0e6-122">Exemple</span><span class="sxs-lookup"><span data-stu-id="ca0e6-122">Example</span></span>

<span data-ttu-id="ca0e6-123">L’exemple suivant montre comment utiliser le Type de requête pour interroger une vue de base de données.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="ca0e6-124">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="ca0e6-125">Tout d’abord, nous définissons un modèle simple de Blog et Post :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="ca0e6-126">Ensuite, nous définissons une vue de base de données simple qui permet d’interroger le nombre de messages associée à chaque blog :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="ca0e6-127">Ensuite, nous configurons le type de requête dans _OnModelCreating_ à l’aide de la ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-127">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="ca0e6-128">API de configuration fluent standard nous permet de configurer le mappage pour le Type de requête :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-128">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="ca0e6-129">Enfin, nous pouvons interrogez la vue de base de données de la façon standard :</span><span class="sxs-lookup"><span data-stu-id="ca0e6-129">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="ca0e6-130">Notez que nous avons également de définir une propriété de niveau de requête (DbQuery) d’agir en tant que racine pour les requêtes sur ce type de contexte.</span><span class="sxs-lookup"><span data-stu-id="ca0e6-130">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
