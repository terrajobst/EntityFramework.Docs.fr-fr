---
title: Test de SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052699"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="eb7d1-102">Test de SQLite</span><span class="sxs-lookup"><span data-stu-id="eb7d1-102">Testing with SQLite</span></span>

<span data-ttu-id="eb7d1-103">SQLite a un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests par rapport à une base de données relationnelle, sans la surcharge des opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="eb7d1-104">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub</span><span class="sxs-lookup"><span data-stu-id="eb7d1-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="eb7d1-105">Exemple de scénario de test</span><span class="sxs-lookup"><span data-stu-id="eb7d1-105">Example testing scenario</span></span>

<span data-ttu-id="eb7d1-106">Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées à des blogs.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="eb7d1-107">Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="eb7d1-108">Il peut être utile d’échange de ce contexte pour se connecter à une base de données en mémoire SQLite afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code ou effectuer beaucoup de travail pour créer un test double du contexte.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="eb7d1-109">Préparer votre contexte</span><span class="sxs-lookup"><span data-stu-id="eb7d1-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="eb7d1-110">Évitez de configurer les deux fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="eb7d1-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="eb7d1-111">Dans vos tests, vous vous apprêtez à l’extérieur de configurer le contexte pour utiliser le fournisseur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="eb7d1-112">Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si une n’a pas déjà été configurée.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="eb7d1-113">Si vous utilisez ASP.NET Core, puis vous devez pas ce code depuis votre fournisseur de base de données est configurée en dehors du contexte (dans Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="eb7d1-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="eb7d1-114">Ajoutez un constructeur pour le test</span><span class="sxs-lookup"><span data-stu-id="eb7d1-114">Add a constructor for testing</span></span>

<span data-ttu-id="eb7d1-115">La méthode la plus simple pour activer le test par rapport à une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="eb7d1-116">`DbContextOptions<TContext>`Indique le contexte de tous ses paramètres, tels que la base de données pour se connecter à.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="eb7d1-117">Il s’agit de l’objet qui est généré par la méthode OnConfiguring en cours d’exécution dans votre contexte.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="eb7d1-118">Écriture de tests</span><span class="sxs-lookup"><span data-stu-id="eb7d1-118">Writing tests</span></span>

<span data-ttu-id="eb7d1-119">La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte à utiliser SQLite, contrôler l’étendue de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="eb7d1-120">L’étendue de la base de données est contrôlé par l’ouverture et de fermeture de la connexion.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="eb7d1-121">La portée de la base de données est limitée à la durée pendant laquelle la connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="eb7d1-122">En règle générale, vous souhaitez une nouvelle base de données pour chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="eb7d1-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
