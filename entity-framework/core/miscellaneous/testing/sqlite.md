---
title: Test avec SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417301"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="c14f5-102">Test avec SQLite</span><span class="sxs-lookup"><span data-stu-id="c14f5-102">Testing with SQLite</span></span>

<span data-ttu-id="c14f5-103">SQLite dispose d’un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests sur une base de données relationnelle, sans la surcharge des opérations de base de données réelles.</span><span class="sxs-lookup"><span data-stu-id="c14f5-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="c14f5-104">Vous pouvez consulter l' [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de cet article sur GitHub</span><span class="sxs-lookup"><span data-stu-id="c14f5-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="c14f5-105">Exemple de scénario de test</span><span class="sxs-lookup"><span data-stu-id="c14f5-105">Example testing scenario</span></span>

<span data-ttu-id="c14f5-106">Examinez le service suivant qui permet au code de l’application d’effectuer des opérations liées aux blogs.</span><span class="sxs-lookup"><span data-stu-id="c14f5-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="c14f5-107">En interne, elle utilise un `DbContext` qui se connecte à une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c14f5-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="c14f5-108">Il serait utile de permuter ce contexte pour se connecter à une base de données SQLite en mémoire afin que nous puissions écrire des tests efficaces pour ce service sans avoir à modifier le code, ou faire beaucoup de travail pour créer un double de test du contexte.</span><span class="sxs-lookup"><span data-stu-id="c14f5-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="c14f5-109">Préparez votre contexte</span><span class="sxs-lookup"><span data-stu-id="c14f5-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="c14f5-110">Évitez de configurer deux fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="c14f5-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="c14f5-111">Dans vos tests, vous allez configurer en externe le contexte pour utiliser le fournisseur d’InMemory.</span><span class="sxs-lookup"><span data-stu-id="c14f5-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="c14f5-112">Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ajouter du code conditionnel pour vous assurer que vous ne configurez le fournisseur de base de données que s’il n’a pas déjà été configuré.</span><span class="sxs-lookup"><span data-stu-id="c14f5-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="c14f5-113">Si vous utilisez ASP.NET Core, vous n’avez pas besoin de ce code, car votre fournisseur de base de données est configuré en dehors du contexte (dans Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="c14f5-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="c14f5-114">Ajouter un constructeur pour le test</span><span class="sxs-lookup"><span data-stu-id="c14f5-114">Add a constructor for testing</span></span>

<span data-ttu-id="c14f5-115">La façon la plus simple d’activer les tests sur une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c14f5-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="c14f5-116">`DbContextOptions<TContext>` indique au contexte tous ses paramètres, tels que la base de données à laquelle se connecter.</span><span class="sxs-lookup"><span data-stu-id="c14f5-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="c14f5-117">Il s’agit du même objet que celui généré par l’exécution de la méthode OnConfiguring dans votre contexte.</span><span class="sxs-lookup"><span data-stu-id="c14f5-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="c14f5-118">Écrire des tests</span><span class="sxs-lookup"><span data-stu-id="c14f5-118">Writing tests</span></span>

<span data-ttu-id="c14f5-119">La clé à tester avec ce fournisseur est la possibilité d’indiquer au contexte d’utiliser SQLite et de contrôler l’étendue de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="c14f5-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="c14f5-120">L’étendue de la base de données est contrôlée par l’ouverture et la fermeture de la connexion.</span><span class="sxs-lookup"><span data-stu-id="c14f5-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="c14f5-121">La portée de la base de données est limitée à la durée pendant laquelle la connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="c14f5-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="c14f5-122">En général, vous souhaitez une base de données propre pour chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="c14f5-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="c14f5-123">Pour utiliser `SqliteConnection()` et la méthode d’extension `.UseSqlite()`, référencez le package NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="c14f5-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
