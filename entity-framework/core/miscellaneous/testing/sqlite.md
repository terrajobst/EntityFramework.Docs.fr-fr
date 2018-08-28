---
title: Test avec SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996866"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="f691f-102">Test avec SQLite</span><span class="sxs-lookup"><span data-stu-id="f691f-102">Testing with SQLite</span></span>

<span data-ttu-id="f691f-103">SQLite a un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests par rapport à une base de données relationnelle, sans la surcharge des opérations de base de données réelle.</span><span class="sxs-lookup"><span data-stu-id="f691f-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="f691f-104">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub</span><span class="sxs-lookup"><span data-stu-id="f691f-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="f691f-105">Exemple de scénario de test</span><span class="sxs-lookup"><span data-stu-id="f691f-105">Example testing scenario</span></span>

<span data-ttu-id="f691f-106">Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées aux blogs.</span><span class="sxs-lookup"><span data-stu-id="f691f-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="f691f-107">Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f691f-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="f691f-108">Il serait utile pour le remplacement de ce contexte pour se connecter à une base de données en mémoire SQLite afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code, ou vous pouvez faire beaucoup de travail pour créer un test double du contexte.</span><span class="sxs-lookup"><span data-stu-id="f691f-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="f691f-109">Préparer votre contexte</span><span class="sxs-lookup"><span data-stu-id="f691f-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="f691f-110">Évitez de configurer deux fournisseurs de base de données</span><span class="sxs-lookup"><span data-stu-id="f691f-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="f691f-111">Dans vos tests, vous vous apprêtez à configurer en externe le contexte pour utiliser le fournisseur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f691f-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="f691f-112">Si vous configurez un fournisseur de base de données en substituant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si elle n’a pas déjà été configurée.</span><span class="sxs-lookup"><span data-stu-id="f691f-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="f691f-113">Si vous utilisez ASP.NET Core, vous devez inutile ce code dans la mesure où votre fournisseur de base de données est configuré en dehors du contexte (dans Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="f691f-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="f691f-114">Ajoutez un constructeur pour le test</span><span class="sxs-lookup"><span data-stu-id="f691f-114">Add a constructor for testing</span></span>

<span data-ttu-id="f691f-115">Pour activer le test par rapport à une autre base de données le plus simple consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="f691f-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="f691f-116">`DbContextOptions<TContext>` Indique le contexte à tous ses paramètres, tels que la base de données pour se connecter à.</span><span class="sxs-lookup"><span data-stu-id="f691f-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="f691f-117">Il s’agit de l’objet qui est généré en exécutant la méthode OnConfiguring dans votre contexte.</span><span class="sxs-lookup"><span data-stu-id="f691f-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="f691f-118">Écriture de tests</span><span class="sxs-lookup"><span data-stu-id="f691f-118">Writing tests</span></span>

<span data-ttu-id="f691f-119">La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte à utiliser SQLite et contrôler la portée de la base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f691f-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="f691f-120">L’étendue de la base de données est contrôlé par l’ouverture et fermeture de la connexion.</span><span class="sxs-lookup"><span data-stu-id="f691f-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="f691f-121">La base de données est limitée à la durée pendant laquelle la connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="f691f-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="f691f-122">En général, vous souhaitez une nouvelle base de données pour chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="f691f-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
