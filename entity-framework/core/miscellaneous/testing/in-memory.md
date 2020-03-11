---
title: Test avec InMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416508"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="51c23-102">Test avec InMemory</span><span class="sxs-lookup"><span data-stu-id="51c23-102">Testing with InMemory</span></span>

<span data-ttu-id="51c23-103">Le fournisseur d’InMemory est utile lorsque vous souhaitez tester des composants à l’aide d’un composant qui se rapproche de la connexion à la base de données réelle, sans la surcharge des opérations de base de données réelles.</span><span class="sxs-lookup"><span data-stu-id="51c23-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="51c23-104">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="51c23-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="51c23-105">La mémoire n’est pas une base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="51c23-105">InMemory is not a relational database</span></span>

<span data-ttu-id="51c23-106">EF Core les fournisseurs de base de données n’ont pas besoin d’être des bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="51c23-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="51c23-107">La mémoire est conçue comme une base de données à usage général à des fins de test et n’est pas conçue pour imiter une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="51c23-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="51c23-108">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="51c23-108">Some examples of this include:</span></span>

* <span data-ttu-id="51c23-109">InMemory vous permet d’enregistrer des données qui enfreignent les contraintes d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="51c23-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="51c23-110">Si vous utilisez DefaultValueSql (String) pour une propriété dans votre modèle, il s’agit d’une API de base de données relationnelle qui n’aura aucun effet lors de l’exécution sur InMemory.</span><span class="sxs-lookup"><span data-stu-id="51c23-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="51c23-111">La [concurrence via l’horodateur/la version de ligne](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` ou `IsRowVersion`) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="51c23-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="51c23-112">Aucune [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) n’est levée si une mise à jour est effectuée à l’aide d’un ancien jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="51c23-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="51c23-113">Dans de nombreux cas de test, ces différences n’ont pas d’importance.</span><span class="sxs-lookup"><span data-stu-id="51c23-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="51c23-114">Toutefois, si vous souhaitez tester des éléments qui se comportent davantage comme une véritable base de données relationnelle, envisagez d’utiliser [le mode en mémoire SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="51c23-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="51c23-115">Exemple de scénario de test</span><span class="sxs-lookup"><span data-stu-id="51c23-115">Example testing scenario</span></span>

<span data-ttu-id="51c23-116">Examinez le service suivant qui permet au code de l’application d’effectuer des opérations liées aux blogs.</span><span class="sxs-lookup"><span data-stu-id="51c23-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="51c23-117">En interne, elle utilise un `DbContext` qui se connecte à une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51c23-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="51c23-118">Il serait utile de permuter ce contexte pour se connecter à une base de données InMemory afin que nous puissions écrire des tests efficaces pour ce service sans avoir à modifier le code, ou faire beaucoup de travail pour créer un double de test du contexte.</span><span class="sxs-lookup"><span data-stu-id="51c23-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="51c23-119">Préparez votre contexte</span><span class="sxs-lookup"><span data-stu-id="51c23-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="51c23-120">Évitez de configurer deux fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="51c23-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="51c23-121">Dans vos tests, vous allez configurer en externe le contexte pour utiliser le fournisseur d’InMemory.</span><span class="sxs-lookup"><span data-stu-id="51c23-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="51c23-122">Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ajouter du code conditionnel pour vous assurer que vous ne configurez le fournisseur de base de données que s’il n’a pas déjà été configuré.</span><span class="sxs-lookup"><span data-stu-id="51c23-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="51c23-123">Si vous utilisez ASP.NET Core, vous n’avez pas besoin de ce code, car votre fournisseur de base de données est déjà configuré en dehors du contexte (dans Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="51c23-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="51c23-124">Ajouter un constructeur pour le test</span><span class="sxs-lookup"><span data-stu-id="51c23-124">Add a constructor for testing</span></span>

<span data-ttu-id="51c23-125">La façon la plus simple d’activer les tests sur une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="51c23-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="51c23-126">`DbContextOptions<TContext>` indique au contexte tous ses paramètres, tels que la base de données à laquelle se connecter.</span><span class="sxs-lookup"><span data-stu-id="51c23-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="51c23-127">Il s’agit du même objet que celui généré par l’exécution de la méthode OnConfiguring dans votre contexte.</span><span class="sxs-lookup"><span data-stu-id="51c23-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="51c23-128">Écrire des tests</span><span class="sxs-lookup"><span data-stu-id="51c23-128">Writing tests</span></span>

<span data-ttu-id="51c23-129">La clé à tester avec ce fournisseur est la possibilité d’indiquer au contexte d’utiliser le fournisseur d’InMemory et de contrôler l’étendue de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="51c23-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="51c23-130">En général, vous souhaitez une base de données propre pour chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="51c23-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="51c23-131">Voici un exemple de classe de test qui utilise la base de données InMemory.</span><span class="sxs-lookup"><span data-stu-id="51c23-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="51c23-132">Chaque méthode de test spécifie un nom de base de données unique, ce qui signifie que chaque méthode possède sa propre base de données InMemory.</span><span class="sxs-lookup"><span data-stu-id="51c23-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="51c23-133">Pour utiliser la méthode d’extension `.UseInMemoryDatabase()`, référencez le package NuGet [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="51c23-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
