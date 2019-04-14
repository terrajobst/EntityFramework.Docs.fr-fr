---
title: Test avec InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 8aaea52f22954ef6a2b7d9b9c5627597c61ac644
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562544"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="de511-102">Test avec InMemory</span><span class="sxs-lookup"><span data-stu-id="de511-102">Testing with InMemory</span></span>

<span data-ttu-id="de511-103">Le fournisseur en mémoire est utile lorsque vous souhaitez tester les composants à l’aide d’un élément qui est une approximation de la connexion à la base de données réelle, sans la surcharge des opérations de base de données réelle.</span><span class="sxs-lookup"><span data-stu-id="de511-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="de511-104">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="de511-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="de511-105">En mémoire n’est pas une base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="de511-105">InMemory is not a relational database</span></span>

<span data-ttu-id="de511-106">Fournisseurs de base de données EF Core n’ont pas à être des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="de511-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="de511-107">InMemory est conçu pour être une base de données à usage général pour le test et n’est pas conçu pour imiter une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="de511-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="de511-108">Voici quelques exemples de ce sont :</span><span class="sxs-lookup"><span data-stu-id="de511-108">Some examples of this include:</span></span>

* <span data-ttu-id="de511-109">InMemory vous permettra enregistrer les données qui risque de violer les contraintes d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="de511-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="de511-110">Si vous utilisez DefaultValueSql(string) pour une propriété dans votre modèle, cela est une API de base de données relationnelle et n’a aucun effet lors de l’exécution par rapport à InMemory.</span><span class="sxs-lookup"><span data-stu-id="de511-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="de511-111">[Accès concurrentiel via la version de l’horodatage/ligne](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` ou `IsRowVersion`) n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="de511-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="de511-112">Ne [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) sera levée si une mise à jour est effectuée à l’aide d’un ancien jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="de511-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="de511-113">Pour autant à des fins de test ces différences ne seront pas important.</span><span class="sxs-lookup"><span data-stu-id="de511-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="de511-114">Toutefois, si vous souhaitez tester par rapport à quelque chose qui se comporte davantage comme une véritable base de données relationnelle, envisagez d’utiliser [mode in-memory de SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="de511-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="de511-115">Exemple de scénario de test</span><span class="sxs-lookup"><span data-stu-id="de511-115">Example testing scenario</span></span>

<span data-ttu-id="de511-116">Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées aux blogs.</span><span class="sxs-lookup"><span data-stu-id="de511-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="de511-117">Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="de511-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="de511-118">Il serait utile pour le remplacement de ce contexte pour se connecter à une base de données en mémoire afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code, ou vous pouvez faire beaucoup de travail pour créer un test double du contexte.</span><span class="sxs-lookup"><span data-stu-id="de511-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="de511-119">Préparer votre contexte</span><span class="sxs-lookup"><span data-stu-id="de511-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="de511-120">Évitez de configurer deux fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="de511-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="de511-121">Dans vos tests, vous vous apprêtez à configurer en externe le contexte pour utiliser le fournisseur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="de511-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="de511-122">Si vous configurez un fournisseur de base de données en substituant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si elle n’a pas déjà été configurée.</span><span class="sxs-lookup"><span data-stu-id="de511-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="de511-123">Si vous utilisez ASP.NET Core, vous devez inutile ce code dans la mesure où votre fournisseur de base de données est déjà configuré en dehors du contexte (dans Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="de511-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="de511-124">Ajoutez un constructeur pour le test</span><span class="sxs-lookup"><span data-stu-id="de511-124">Add a constructor for testing</span></span>

<span data-ttu-id="de511-125">Pour activer le test par rapport à une autre base de données le plus simple consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="de511-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="de511-126">`DbContextOptions<TContext>` Indique le contexte à tous ses paramètres, tels que la base de données pour se connecter à.</span><span class="sxs-lookup"><span data-stu-id="de511-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="de511-127">Il s’agit de l’objet qui est généré en exécutant la méthode OnConfiguring dans votre contexte.</span><span class="sxs-lookup"><span data-stu-id="de511-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="de511-128">Écriture de tests</span><span class="sxs-lookup"><span data-stu-id="de511-128">Writing tests</span></span>

<span data-ttu-id="de511-129">La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte d’utiliser le fournisseur en mémoire et de contrôler l’étendue de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="de511-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="de511-130">En général, vous souhaitez une nouvelle base de données pour chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="de511-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="de511-131">Voici un exemple d’une classe de test qui utilise la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="de511-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="de511-132">Chaque méthode de test spécifie un nom de base de données unique, ce qui signifie que chaque méthode possède sa propre base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="de511-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="de511-133">Pour utiliser le `.UseInMemoryDatabase()` référence le package NuGet, méthode d’extension [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="de511-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
