---
title: Testabilité et Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 2a2384c7868ae3cf6af4f915c06ae9fdb622634c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251321"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="e6313-102">Testabilité et Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="e6313-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="e6313-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="e6313-103">Scott Allen</span></span>

<span data-ttu-id="e6313-104">Date de publication : Mai 2010</span><span class="sxs-lookup"><span data-stu-id="e6313-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="e6313-105">Introduction</span><span class="sxs-lookup"><span data-stu-id="e6313-105">Introduction</span></span>

<span data-ttu-id="e6313-106">Ce livre blanc décrit et montre comment écrire du code testable avec l’ADO.NET Entity Framework 4.0 et Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e6313-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="e6313-107">Ce document n’essaie pas de vous concentrer sur une méthodologie de test spécifique, telles que la conception pilotée par des tests (TDD) ou de la conception pilotée par comportement (BDD).</span><span class="sxs-lookup"><span data-stu-id="e6313-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="e6313-108">Au lieu de cela, ce document se focalise sur la façon d’écrire du code qui utilise ADO.NET Entity Framework encore reste facile à isoler et de tester de manière automatique.</span><span class="sxs-lookup"><span data-stu-id="e6313-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="e6313-109">Nous allons examiner les modèles de conception courants qui facilitent la scénarios de test dans les données access et voir comment appliquer ces modèles lors de l’utilisation de l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="e6313-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="e6313-110">Nous examinerons également les fonctionnalités spécifiques de l’infrastructure pour voir comment ces fonctionnalités peuvent fonctionnent dans le code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="e6313-111">Quel est le Code Testable ?</span><span class="sxs-lookup"><span data-stu-id="e6313-111">What Is Testable Code?</span></span>

<span data-ttu-id="e6313-112">La possibilité de vérifier un morceau de logiciel à l’aide de tests unitaires automatisés offre de nombreux avantages souhaitables.</span><span class="sxs-lookup"><span data-stu-id="e6313-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="e6313-113">Tout le monde sait que les bons tests réduira le nombre de défauts logiciels dans une application et l’augmentation de qualité de l’application - des tests unitaires en place va bien au-delà de trouver les bogues.</span><span class="sxs-lookup"><span data-stu-id="e6313-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="e6313-114">Une suite de tests unitaires corrects permet à une équipe de développement gagner du temps et gardez le contrôle des logiciels qu’ils créent.</span><span class="sxs-lookup"><span data-stu-id="e6313-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="e6313-115">Une équipe peut apporter des modifications au code existant, refactoriser, nouvelle conception, et la restructuration des logiciels pour répondre aux nouvelles exigences, ajouter de nouveaux composants dans une application tout en connaître la suite de tests peuvent vérifier le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e6313-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="e6313-116">Tests unitaires font partie d’un cycle de commentaires rapide pour faciliter la modification et de conserver la facilité de maintenance du logiciel en tant que la complexité augmente.</span><span class="sxs-lookup"><span data-stu-id="e6313-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="e6313-117">Tests unitaires est fourni avec un prix, toutefois.</span><span class="sxs-lookup"><span data-stu-id="e6313-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="e6313-118">Une équipe doit consacrer du temps pour créer et gérer des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="e6313-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="e6313-119">La quantité de l’effort requis pour créer ces tests est directement liée à la **testabilité** du logiciel sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="e6313-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="e6313-120">Simple est le logiciel à tester ?</span><span class="sxs-lookup"><span data-stu-id="e6313-120">How easy is the software to test?</span></span> <span data-ttu-id="e6313-121">Une équipe de conception de logiciels de testabilité à l’esprit créera plus rapidement que l’équipe travaillant avec le logiciel-testable tests efficaces.</span><span class="sxs-lookup"><span data-stu-id="e6313-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="e6313-122">Microsoft a conçu l’ADO.NET Entity Framework 4.0 (EF4) de testabilité à l’esprit.</span><span class="sxs-lookup"><span data-stu-id="e6313-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="e6313-123">Cela ne signifie pas que les développeurs doit écrire les tests unitaires sur le code du framework lui-même.</span><span class="sxs-lookup"><span data-stu-id="e6313-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="e6313-124">Au lieu de cela, les objectifs de testabilité de EF4 facilitent créer du code testable qui s’appuie sur l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="e6313-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="e6313-125">Avant d’examiner des exemples spécifiques, il est utile de comprendre les qualités de code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="e6313-126">Les qualités de Code Testable</span><span class="sxs-lookup"><span data-stu-id="e6313-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="e6313-127">Code qui est facile à tester présente toujours au moins deux caractéristiques.</span><span class="sxs-lookup"><span data-stu-id="e6313-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="e6313-128">Tout d’abord, testable code est facile à **observer**.</span><span class="sxs-lookup"><span data-stu-id="e6313-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="e6313-129">Étant donné un ensemble d’entrées, il devrait être facile à observer la sortie du code.</span><span class="sxs-lookup"><span data-stu-id="e6313-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="e6313-130">Par exemple, test de la méthode suivante est facile, car la méthode retourne directement le résultat d’un calcul.</span><span class="sxs-lookup"><span data-stu-id="e6313-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="e6313-131">Une méthode de test est difficile si la méthode écrit la valeur calculée dans un socket réseau, une table de base de données ou un fichier comme le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e6313-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="e6313-132">Le test doit effectuer un travail supplémentaire pour récupérer la valeur.</span><span class="sxs-lookup"><span data-stu-id="e6313-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="e6313-133">Deuxièmement, le code testable est facile de **isoler**.</span><span class="sxs-lookup"><span data-stu-id="e6313-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="e6313-134">Nous allons utiliser le pseudo-code suivant un mauvais exemple de code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="e6313-135">La méthode est facile à observer : nous pouvons transmettre d’assurance et vérifiez la valeur de retour correspond à un résultat attendu.</span><span class="sxs-lookup"><span data-stu-id="e6313-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="e6313-136">Toutefois, pour tester la méthode nous allons devoir disposer d’une base de données installée avec le schéma correct et configurer le serveur SMTP dans le cas où la méthode essaie d’envoyer un message électronique.</span><span class="sxs-lookup"><span data-stu-id="e6313-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="e6313-137">Le test unitaire uniquement besoin de vérifier la logique de calcul à l’intérieur de la méthode, mais le test peut échouer, car le serveur de messagerie est hors connexion ou de déplacer le serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="e6313-138">Les deux de ces échecs ne sont pas liées au comportement que souhaite pour vérifier que le test.</span><span class="sxs-lookup"><span data-stu-id="e6313-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="e6313-139">Le comportement est difficile à isoler.</span><span class="sxs-lookup"><span data-stu-id="e6313-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="e6313-140">Les développeurs de logiciels qui s’efforcent d’écrire du code testable souvent s’efforcent de maintenir une séparation des responsabilités dans le code qu’ils écrivent.</span><span class="sxs-lookup"><span data-stu-id="e6313-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="e6313-141">La méthode ci-dessus doit se concentrer sur les calculs d’entreprise et déléguer les détails d’implémentation de base de données et de courrier électronique à d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="e6313-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="e6313-142">Martin appelle cela le principe de responsabilité unique.</span><span class="sxs-lookup"><span data-stu-id="e6313-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="e6313-143">Un objet doit encapsuler une responsabilité unique, étroite, comme le calcul de la valeur d’une stratégie.</span><span class="sxs-lookup"><span data-stu-id="e6313-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="e6313-144">Toutes les autres tâches de base de données et de notification doit être la responsabilité d’un autre objet.</span><span class="sxs-lookup"><span data-stu-id="e6313-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="e6313-145">Le code écrit de cette manière est plus facile à isoler, car il se concentre sur une seule tâche.</span><span class="sxs-lookup"><span data-stu-id="e6313-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="e6313-146">Dans .NET, nous avons les abstractions que nous devons suivre le principe de responsabilité unique et d’isolation.</span><span class="sxs-lookup"><span data-stu-id="e6313-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="e6313-147">Nous pouvons utiliser des définitions d’interface et forcer le code à utiliser l’abstraction de l’interface au lieu d’un type concret.</span><span class="sxs-lookup"><span data-stu-id="e6313-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="e6313-148">Plus loin dans ce document, nous allons voir comment une méthode, telles que l’exemple incorrect présenté ci-dessus peut fonctionner avec les interfaces que *rechercher* comme ils parlera à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="e6313-149">Au moment du test, toutefois, nous pouvons remplacer par une implémentation factice qui ne communiquer avec la base de données, mais au lieu de cela maintient les données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e6313-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="e6313-150">Cette implémentation factice sera isoler le code à partir des problèmes non liés dans la configuration de la base de données ou un code d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="e6313-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="e6313-151">Il existe d’autres avantages à l’isolation.</span><span class="sxs-lookup"><span data-stu-id="e6313-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="e6313-152">Le calcul de l’entreprise dans la dernière méthode ne devrait prendre que quelques millisecondes pour exécuter, mais le test lui-même peut s’exécuter pendant plusieurs secondes en tant que les tronçons code autour du réseau et les discussions à divers serveurs.</span><span class="sxs-lookup"><span data-stu-id="e6313-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="e6313-153">Tests unitaires doivent s’exécuter rapidement pour faciliter les modifications mineures.</span><span class="sxs-lookup"><span data-stu-id="e6313-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="e6313-154">Tests unitaires doivent également être reproductibles et pas échouer, car un composant non lié au test a un problème.</span><span class="sxs-lookup"><span data-stu-id="e6313-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="e6313-155">Écriture de code qui est facile à observer et à isoler signifie que les développeurs bénéficient d’une heure plus facile l’écriture de tests pour le code, passez moins temps d’attente des tests à exécuter, et plus important encore, allégez les bogues qui n’existent pas.</span><span class="sxs-lookup"><span data-stu-id="e6313-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="e6313-156">J’espère que vous pouvez apprécier les avantages des tests et comprendre les qualités qui expose le code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="e6313-157">Nous sommes sur le point de quelle manière d’écrire du code qui fonctionne avec EF4 pour enregistrer les données dans une base de données tout en restant observables et facile d’isoler, mais tout d’abord nous allons affiner notre pour discuter des conceptions testables pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="e6313-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="e6313-158">Modèles de conception pour la persistance des données</span><span class="sxs-lookup"><span data-stu-id="e6313-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="e6313-159">Les deux exemples incorrects présentés précédemment avaient trop de responsabilités.</span><span class="sxs-lookup"><span data-stu-id="e6313-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="e6313-160">Le premier exemple incorrect dû effectuer un calcul *et* écrire dans un fichier.</span><span class="sxs-lookup"><span data-stu-id="e6313-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="e6313-161">Le deuxième exemple de mauvaise avait lire les données à partir d’une base de données *et* effectuent un calcul de l’entreprise *et* envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="e6313-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="e6313-162">En concevant des méthodes plus petits qui séparent les préoccupations et déléguer la responsabilité à d’autres composants, vous allez très grandes avancées vers la rédaction de code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="e6313-163">L’objectif consiste à créer des fonctionnalités en composant des actions à partir des abstractions réduites et focalisées.</span><span class="sxs-lookup"><span data-stu-id="e6313-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="e6313-164">Lorsqu’il s’agit de persistance des données le petit et abstractions ayant le focus, nous recherchons sont si communes qu’ils avoir été décrit en tant que modèles de conception.</span><span class="sxs-lookup"><span data-stu-id="e6313-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="e6313-165">Livre de Martin Fowler Patterns of Enterprise Application Architecture a été le premier travail pour décrire ces modèles à l’impression.</span><span class="sxs-lookup"><span data-stu-id="e6313-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="e6313-166">Nous vous fournirons une brève description de ces modèles dans les sections suivantes avant de vous montrer comment ces ADO.NET Entity Framework implémente et fonctionne avec ces modèles.</span><span class="sxs-lookup"><span data-stu-id="e6313-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="e6313-167">Le modèle de référentiel</span><span class="sxs-lookup"><span data-stu-id="e6313-167">The Repository Pattern</span></span>

<span data-ttu-id="e6313-168">Fowler indique qu’un référentiel » sert d’intermédiaire entre le domaine et les données des couches de mappage à l’aide d’une interface semblable à la collection pour l’accès aux objets de domaine ».</span><span class="sxs-lookup"><span data-stu-id="e6313-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="e6313-169">L’objectif du modèle dépôt est d’isoler le code à partir de la minutie et impact de l’accès aux données, et comme nous l’avons vu isolation antérieures est une caractéristique requise pour la testabilité.</span><span class="sxs-lookup"><span data-stu-id="e6313-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="e6313-170">La clé à l’isolation est comment le référentiel expose les objets à l’aide d’une interface semblable à la collection.</span><span class="sxs-lookup"><span data-stu-id="e6313-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="e6313-171">La logique que vous écrivez pour n’utiliser le référentiel ne sait aucun comment le référentiel matérialisera les objets que vous demandez.</span><span class="sxs-lookup"><span data-stu-id="e6313-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="e6313-172">Le référentiel peut communiquer avec une base de données, ou elle peut simplement retourner des objets à partir d’une collection en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e6313-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="e6313-173">Tout votre code doit connaître est que le référentiel s’affiche pour conserver la collection, et vous pouvez récupérer, ajouter et supprimer des objets de la collection.</span><span class="sxs-lookup"><span data-stu-id="e6313-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="e6313-174">Dans les applications .NET existantes un référentiel concret hérite souvent une interface générique comme suit :</span><span class="sxs-lookup"><span data-stu-id="e6313-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="e6313-175">Nous allons apporter quelques modifications à la définition d’interface lorsque nous fournir une implémentation pour EF4, mais le concept de base reste le même.</span><span class="sxs-lookup"><span data-stu-id="e6313-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="e6313-176">Code peut utiliser un référentiel concrète qui implémente cette interface pour récupérer une entité par sa valeur de clé primaire, pour récupérer une collection d’entités en fonction de la version d’évaluation d’un prédicat, ou simplement récupérer toutes les entités disponibles.</span><span class="sxs-lookup"><span data-stu-id="e6313-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="e6313-177">Le code peut également ajouter et supprimer des entités via l’interface de référentiel.</span><span class="sxs-lookup"><span data-stu-id="e6313-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="e6313-178">Étant donné un objets IRepository d’employé, code peut effectuer les opérations suivantes.</span><span class="sxs-lookup"><span data-stu-id="e6313-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="e6313-179">Étant donné que le code utilise une interface (IRepository d’employé), nous pouvons fournir le code avec des implémentations différentes de l’interface.</span><span class="sxs-lookup"><span data-stu-id="e6313-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="e6313-180">Une implémentation peut être une implémentation bénéficie d’EF4 et objets persistants dans une base de données Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e6313-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="e6313-181">Une implémentation différente (celui utilisé pendant le test) peut être sauvegardée par un objet de liste d’employés en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e6313-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="e6313-182">L’interface vous aidera à isoler le code.</span><span class="sxs-lookup"><span data-stu-id="e6313-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="e6313-183">Notez le IRepository&lt;T&gt; interface n’expose pas une opération d’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e6313-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="e6313-184">Comment nous mettre à jour les objets existants ?</span><span class="sxs-lookup"><span data-stu-id="e6313-184">How do we update existing objects?</span></span> <span data-ttu-id="e6313-185">Vous pouvez trouver des définitions de IRepository qui n’incluent pas l’opération d’enregistrement, et les implémentations de ces référentiels devront immédiatement conserver un objet dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="e6313-186">Toutefois, dans de nombreuses applications, nous ne voulons de conserver des objets individuellement.</span><span class="sxs-lookup"><span data-stu-id="e6313-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="e6313-187">Au lieu de cela, nous voulons mettre des objets à durée de vie, peut-être à partir de différents référentiels, modifier ces objets dans le cadre d’une activité d’entreprise, puis conserve tous les objets dans le cadre d’une seule opération atomique.</span><span class="sxs-lookup"><span data-stu-id="e6313-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="e6313-188">Heureusement, il existe un modèle pour permettre ce type de comportement.</span><span class="sxs-lookup"><span data-stu-id="e6313-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="e6313-189">La modèle unité de travail</span><span class="sxs-lookup"><span data-stu-id="e6313-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="e6313-190">Fowler indique qu’une unité de travail « conserve une liste d’objets affectés par une transaction commerciale et coordonne l’écriture en dehors des modifications et la résolution des problèmes d’accès concurrentiel ».</span><span class="sxs-lookup"><span data-stu-id="e6313-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="e6313-191">Il incombe à l’unité de travail pour suivre les modifications apportées aux objets nous donner vie à partir d’un référentiel et conserver toutes les modifications que nous avons apportées aux objets lorsque nous indiquons à l’unité de travail pour valider les modifications.</span><span class="sxs-lookup"><span data-stu-id="e6313-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="e6313-192">Il est également la responsabilité de l’unité de travail pour prendre les nouveaux objets que nous avons ajoutée à tous les dépôts et insérer des objets dans une base de données, ainsi que la suppression de gérer.</span><span class="sxs-lookup"><span data-stu-id="e6313-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="e6313-193">Si vous avez fait tout travail avec des jeux de données ADO.NET vous serez déjà familiarisé avec la modèle unité de travail.</span><span class="sxs-lookup"><span data-stu-id="e6313-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="e6313-194">DataSets ADO.NET eu la possibilité de suivre nos mises à jour, suppressions et l’insertion d’objets DataRow et pu rapprocher (à l’aide d’un TableAdapter) tous les changements à une base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="e6313-195">Toutefois, les objets de jeu de données modèle un sous-ensemble déconnecté de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e6313-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="e6313-196">La modèle unité de travail présente le même comportement, mais fonctionne avec des objets métier et les objets de domaine qui sont isolés à partir de code d’accès aux données et pas au courant de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="e6313-197">Une abstraction pour modéliser l’unité de travail dans le code .NET peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="e6313-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="e6313-198">En exposant les références de référentiel à partir de l’unité de travail, que nous pouvons garantir une seule unité de travail objet a la possibilité de suivre toutes les entités matérialisées pendant une transaction commerciale.</span><span class="sxs-lookup"><span data-stu-id="e6313-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="e6313-199">L’implémentation de la méthode de validation pour une véritable unité de travail est où toute la magie se produit pour harmoniser les modifications en mémoire avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="e6313-200">Étant donné une référence IUnitOfWork, le code peut apporter des modifications à des objets métier récupérées à partir d’un ou plusieurs référentiels et enregistrer toutes les modifications à l’aide de l’opération de validation atomique.</span><span class="sxs-lookup"><span data-stu-id="e6313-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="e6313-201">Le modèle de chargement différé</span><span class="sxs-lookup"><span data-stu-id="e6313-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="e6313-202">Fowler utilise le chargement différé de nom pour décrire des « un objet qui ne contient pas toutes les données vous avez besoin mais sait comment l’obtenir ».</span><span class="sxs-lookup"><span data-stu-id="e6313-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="e6313-203">Le chargement différé transparent est une fonctionnalité importante à lorsqu’ils sont l’écriture de code testable métier et l’utilisation avec une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e6313-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="e6313-204">Par exemple, prenons le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e6313-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="e6313-205">Comment la collection de cartes de pointage est remplie ?</span><span class="sxs-lookup"><span data-stu-id="e6313-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="e6313-206">Il existe deux réponses possibles.</span><span class="sxs-lookup"><span data-stu-id="e6313-206">There are two possible answers.</span></span> <span data-ttu-id="e6313-207">Une réponse est que le référentiel d’employé, lorsque vous êtes invité à extraire un employé, émet une requête pour récupérer les deux l’employé ainsi que des informations de carte de temps associée de l’employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="e6313-208">Dans les bases de données relationnelles en général une requête avec une clause JOIN et cela peut peut entraîner l’extraction de plus d’informations qu’une application a besoin.</span><span class="sxs-lookup"><span data-stu-id="e6313-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="e6313-209">Que se passe-t-il si l’application doit jamais toucher la propriété de cartes de pointage ?</span><span class="sxs-lookup"><span data-stu-id="e6313-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="e6313-210">Une deuxième réponse consiste à charger la propriété de cartes de pointage « à la demande ».</span><span class="sxs-lookup"><span data-stu-id="e6313-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="e6313-211">Le chargement différé est implicite et transparent pour la logique métier, car le code n’appelle pas des API spéciales pour récupérer des informations de carte de temps.</span><span class="sxs-lookup"><span data-stu-id="e6313-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="e6313-212">Le code suppose que les informations de carte de temps sont présentes si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e6313-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="e6313-213">Certains magique est impliqué avec le chargement différé qui implique généralement l’interception d’exécution d’appels de méthode.</span><span class="sxs-lookup"><span data-stu-id="e6313-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="e6313-214">Le code d’interception est responsable de la communication avec la base de données et récupérer des informations de carte de temps tout en laissant la logique métier en être la logique métier.</span><span class="sxs-lookup"><span data-stu-id="e6313-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="e6313-215">Cette commande magique de chargement différé permet au code de l’entreprise pour lui-même isoler des opérations d’extraction de données et les résultats dans plus de code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="e6313-216">L’inconvénient d’un chargement différé est que lorsqu’une application *est* besoin des informations de carte de temps le code s’exécute une requête supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="e6313-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="e6313-217">Ce n’est pas un critère important pour de nombreuses applications, mais pour les applications sensibles performances ou des applications effectuant une boucle dans un nombre d’objets de l’employé et l’exécution d’une requête pour récupérer des cartes de temps lors de chaque itération de la boucle (un problème souvent appelé N + 1 problème de requête), le chargement différé est une opération de glissement.</span><span class="sxs-lookup"><span data-stu-id="e6313-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="e6313-218">Dans ces scénarios une application peut souhaiter vous empresser de charger les informations de carte de l’heure de la manière la plus efficace possible.</span><span class="sxs-lookup"><span data-stu-id="e6313-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="e6313-219">Heureusement, nous allons voir comment EF4 prend en charge les deux charges tardives implicites et effectue un chargement hâtif efficace comme nous déplacer dans la section suivante et implémenter ces modèles.</span><span class="sxs-lookup"><span data-stu-id="e6313-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="e6313-220">Implémentation de modèles avec Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e6313-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="e6313-221">La bonne nouvelle est que tous les modèles de conception que nous avons décrit dans la dernière section sont faciles à implémenter avec EF4.</span><span class="sxs-lookup"><span data-stu-id="e6313-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="e6313-222">Pour illustrer, nous allons utiliser une application ASP.NET MVC simple pour modifier et afficher des employés et leurs informations de carte de temps associée.</span><span class="sxs-lookup"><span data-stu-id="e6313-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="e6313-223">Nous allons commencer à l’aide des suivante « plain old CLR objects » (OCT).</span><span class="sxs-lookup"><span data-stu-id="e6313-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="e6313-224">Ces définitions de classe change légèrement nous explorons les différentes approches et les fonctionnalités de EF4, mais l’objectif est de conserver ces classes ignorent la persistance (PI) que possible.</span><span class="sxs-lookup"><span data-stu-id="e6313-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="e6313-225">Un objet PI ne sait pas *comment*, voire *si*, l’état, sa valeur est réside dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="e6313-226">PI et oct vont de pair avec le logiciel testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="e6313-227">Objets à l’aide d’une approche POCO sont moins limités, plus flexible et plus faciles à tester, car ils peuvent traiter sans une base de données actuelle.</span><span class="sxs-lookup"><span data-stu-id="e6313-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="e6313-228">Avec les objets oct en place, nous pouvons créer un modèle EDM (Entity Data Model) dans Visual Studio (voir figure 1).</span><span class="sxs-lookup"><span data-stu-id="e6313-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="e6313-229">Nous n’utiliserons pas le modèle EDM pour générer le code pour nos entités.</span><span class="sxs-lookup"><span data-stu-id="e6313-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="e6313-230">Au lieu de cela, nous souhaitons utiliser les entités que nous affectueusement abrégé créer manuellement.</span><span class="sxs-lookup"><span data-stu-id="e6313-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="e6313-231">Nous utiliserons uniquement le modèle EDM pour générer notre schéma de base de données et fournir les métadonnées qu'ef4 a besoin de mapper des objets dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="e6313-233">**Figure 1**</span><span class="sxs-lookup"><span data-stu-id="e6313-233">**Figure 1**</span></span>

<span data-ttu-id="e6313-234">Remarque : Si vous souhaitez développer le modèle EDM tout d’abord, il est possible de générer, nettoyer, code POCO à partir du modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="e6313-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="e6313-235">Pour cela, avec une extension de Visual Studio 2010 fournie par l’équipe programmabilité des données.</span><span class="sxs-lookup"><span data-stu-id="e6313-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="e6313-236">Pour télécharger l’extension, lancez le Gestionnaire d’extensions dans le menu Outils dans Visual Studio et rechercher dans la galerie en ligne des modèles pour « POCO » (voir figure 2).</span><span class="sxs-lookup"><span data-stu-id="e6313-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="e6313-237">Il existe plusieurs modèles POCO pour EF.</span><span class="sxs-lookup"><span data-stu-id="e6313-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="e6313-238">Pour plus d’informations sur l’utilisation du modèle, consultez « [procédure pas à pas : modèle de POCO pour Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)».</span><span class="sxs-lookup"><span data-stu-id="e6313-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="e6313-240">**Figure 2**</span><span class="sxs-lookup"><span data-stu-id="e6313-240">**Figure 2**</span></span>

<span data-ttu-id="e6313-241">À partir de ce point de départ de POCO, nous allons examiner deux approches différentes pour code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="e6313-242">La première approche, j’appelle l’approche Entity Framework, car il tire parti des abstractions de l’API Entity Framework pour implémenter des unités de travail et des référentiels.</span><span class="sxs-lookup"><span data-stu-id="e6313-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="e6313-243">Dans la deuxième approche, nous créer nos propres abstractions de dépôt personnalisé et ensuite voir les avantages et inconvénients de chaque approche.</span><span class="sxs-lookup"><span data-stu-id="e6313-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="e6313-244">Nous allons commencer en explorant l’approche Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e6313-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="e6313-245">Une implémentation orientée EF</span><span class="sxs-lookup"><span data-stu-id="e6313-245">An EF Centric Implementation</span></span>

<span data-ttu-id="e6313-246">Envisagez l’action de contrôleur suivante à partir d’un projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e6313-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="e6313-247">L’action récupère un objet Employee et retourne un résultat pour afficher une vue détaillée de l’employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="e6313-248">Le code est testable ?</span><span class="sxs-lookup"><span data-stu-id="e6313-248">Is the code testable?</span></span> <span data-ttu-id="e6313-249">Il existe au moins deux tests, que nous devons vérifier le comportement de l’action.</span><span class="sxs-lookup"><span data-stu-id="e6313-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="e6313-250">Tout d’abord, nous souhaitons vérifier que l’action retourne la bonne vue – un test simple.</span><span class="sxs-lookup"><span data-stu-id="e6313-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="e6313-251">Il est également possible que nous voulons écrire un test pour vérifier l’action récupère l’employé correct, et nous souhaitons faire sans l’exécution de code pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="e6313-252">Rappelez-vous que nous souhaitons isoler le code sous test.</span><span class="sxs-lookup"><span data-stu-id="e6313-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="e6313-253">Isolation garantit que le test n’échoue en raison d’un bogue dans le code d’accès aux données ou la configuration de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="e6313-254">Si le test échoue, nous saura que nous avons un bogue dans la logique du contrôleur et non dans un composant de système de niveau inférieur.</span><span class="sxs-lookup"><span data-stu-id="e6313-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="e6313-255">Pour isoler nous devrons certaines abstractions telles que les interfaces que nous avons présenté précédemment pour les référentiels et les unités de travail.</span><span class="sxs-lookup"><span data-stu-id="e6313-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="e6313-256">N’oubliez pas que le modèle dépôt vise à servant d’intermédiaire entre les objets de domaine et de la couche de mappage de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="e6313-257">Dans ce scénario EF4 *est* les mappage des données de couche et fournit déjà une abstraction de type de référentiel nommée IObjectSet&lt;T&gt; (à partir de l’espace de noms System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="e6313-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="e6313-258">La définition d’interface est similaire à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="e6313-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="e6313-259">IObjectSet&lt;T&gt; répond à la configuration requise pour un référentiel, car il ressemble à une collection d’objets (par le biais de IEnumerable&lt;T&gt;) et fournit des méthodes pour ajouter et supprimer des objets de la collection simulée.</span><span class="sxs-lookup"><span data-stu-id="e6313-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="e6313-260">Les méthodes d’attachement et détachement exposent des fonctionnalités supplémentaires de l’API EF4.</span><span class="sxs-lookup"><span data-stu-id="e6313-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="e6313-261">Pour utiliser IObjectSet&lt;T&gt; en tant que l’interface pour les dépôts nous avons besoin d’une unité de travail abstraction pour lier ensemble des dépôts.</span><span class="sxs-lookup"><span data-stu-id="e6313-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="e6313-262">Une implémentation concrète de cette interface parlera à SQL Server et il est facile de créer en utilisant la classe ObjectContext EF4.</span><span class="sxs-lookup"><span data-stu-id="e6313-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="e6313-263">La classe ObjectContext est la véritable unité de travail dans l’API EF4.</span><span class="sxs-lookup"><span data-stu-id="e6313-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="e6313-264">Mettre un IObjectSet&lt;T&gt; à durée de vie est aussi simple que l’appel de la méthode CreateObjectSet de l’objet ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="e6313-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="e6313-265">Dans les coulisses, l’infrastructure utilisera les métadonnées nous fourni dans le modèle EDM pour produire un ObjectSet concrète&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="e6313-266">Nous nous en tiendrons avec retour le IObjectSet&lt;T&gt; interface, car elle vous aide à préserver la testabilité dans le code client.</span><span class="sxs-lookup"><span data-stu-id="e6313-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="e6313-267">Cette implémentation concrète est utile dans la production, mais nous devons nous concentrer sur la façon dont nous allons utiliser notre abstraction IUnitOfWork pour faciliter les tests.</span><span class="sxs-lookup"><span data-stu-id="e6313-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="e6313-268">Les Doubles de Test</span><span class="sxs-lookup"><span data-stu-id="e6313-268">The Test Doubles</span></span>

<span data-ttu-id="e6313-269">Pour isoler l’action du contrôleur, nous devrons la possibilité de basculer entre l’unité réelle de travail (pris en charge par un ObjectContext) et une unité de double ou « faux » de test de travail (en effectuant les opérations en mémoire).</span><span class="sxs-lookup"><span data-stu-id="e6313-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="e6313-270">L’approche courante pour effectuer ce type de basculement de doit ne pas laisser le contrôleur MVC instancier une unité de travail, mais passe l’unité de travail dans le contrôleur en tant que paramètre de constructeur.</span><span class="sxs-lookup"><span data-stu-id="e6313-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="e6313-271">Le code ci-dessus est un exemple d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e6313-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="e6313-272">Nous n’autorisons le contrôleur pour créer sa dépendance (l’unité de travail), mais injecter la dépendance dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e6313-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="e6313-273">Dans un projet MVC, il est courant d’utiliser une fabrique de contrôleur personnalisée en association avec un conteneur d’inversion de contrôle (IoC) pour automatiser l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e6313-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="e6313-274">Ces rubriques sont dépasse le cadre de cet article, mais vous pouvez en savoir plus en suivant les références à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="e6313-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="e6313-275">Une unité factice d’implémentation de travail que nous pouvons utiliser pour le test peut se présenter comme suit.</span><span class="sxs-lookup"><span data-stu-id="e6313-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="e6313-276">Notez que la fausse unité de travail expose une propriété de validation.</span><span class="sxs-lookup"><span data-stu-id="e6313-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="e6313-277">Il est parfois utile d’ajouter des fonctionnalités à une classe factice qui facilitent le test.</span><span class="sxs-lookup"><span data-stu-id="e6313-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="e6313-278">Dans ce cas, il est facile à observer si le code valide une unité de travail en vérifiant la propriété validée.</span><span class="sxs-lookup"><span data-stu-id="e6313-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="e6313-279">Nous aurons également besoin d’un faux IObjectSet&lt;T&gt; pour contenir les objets Employee et TimeCard en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e6313-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="e6313-280">Nous pouvons fournir une implémentation unique de l’utilisation de génériques.</span><span class="sxs-lookup"><span data-stu-id="e6313-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="e6313-281">Cette double de test délègue la majorité de son travail à un HashSet sous-jacent&lt;T&gt; objet.</span><span class="sxs-lookup"><span data-stu-id="e6313-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="e6313-282">Notez que IObjectSet&lt;T&gt; nécessite une contrainte générique de l’application T en tant que classe (type référence) et également nous oblige à implémenter IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="e6313-283">Il est facile de faire une collection en mémoire apparaître comme un IQueryable&lt;T&gt; à l’aide de l’opérateur LINQ standard AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="e6313-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="e6313-284">Les Tests</span><span class="sxs-lookup"><span data-stu-id="e6313-284">The Tests</span></span>

<span data-ttu-id="e6313-285">Tests unitaires traditionnels utilisera une seule classe de test pour contenir tous les tests pour toutes les actions dans un seul contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="e6313-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="e6313-286">Nous pouvons écrire ces tests, ou n’importe quel type de test unitaire, à l’aide de la mémoire dans fakes que nous avons créé.</span><span class="sxs-lookup"><span data-stu-id="e6313-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="e6313-287">Toutefois, pour cet article, nous pouvons éviter le monolithiques approche de la classe de test et de groupe au lieu de cela nos tests se concentrer sur une partie spécifique des fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="e6313-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="e6313-288">Par exemple, « créer nouvel employé » peut être la fonctionnalité que nous voulons tester, et nous allons utiliser une seule classe de test pour vérifier l’action de contrôleur unique responsable de la création d’un nouvel employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="e6313-289">Il existe du code le programme d’installation courants, que nous avons besoin pour toutes ces classes de test affinée.</span><span class="sxs-lookup"><span data-stu-id="e6313-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="e6313-290">Par exemple, nous devons toujours créer nos référentiels d’en mémoire et d’un fausse unité de travail.</span><span class="sxs-lookup"><span data-stu-id="e6313-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="e6313-291">Nous devons également une instance du contrôleur employé avec l’unité factice de travail injecté.</span><span class="sxs-lookup"><span data-stu-id="e6313-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="e6313-292">Nous vous présenterons ce code de programme d’installation commun entre les classes de test à l’aide d’une classe de base.</span><span class="sxs-lookup"><span data-stu-id="e6313-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="e6313-293">La « mère d’objet « nous utilisons dans la classe de base est un modèle courant pour la création de données de test.</span><span class="sxs-lookup"><span data-stu-id="e6313-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="e6313-294">Une mère de l’objet contient des méthodes de fabrique pour instancier des entités de test à utiliser dans plusieurs contextes de test.</span><span class="sxs-lookup"><span data-stu-id="e6313-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="e6313-295">Nous pouvons utiliser le EmployeeControllerTestBase comme classe de base pour un nombre de contextes de test (voir figure 3).</span><span class="sxs-lookup"><span data-stu-id="e6313-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="e6313-296">Chaque contexte de test teste une action de contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="e6313-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="e6313-297">Par exemple, un contexte de test se concentrera sur test de l’action de création utilisée lors d’une demande HTTP GET (pour afficher la vue pour la création d’un employé) et une fixture différentes se concentrera sur l’action de création utilisée dans une requête HTTP POST (pour tirer des informations fournies par le utilisateur de créer un employé).</span><span class="sxs-lookup"><span data-stu-id="e6313-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="e6313-298">Chaque classe dérivée est uniquement responsable de la configuration requise dans son contexte spécifique et de fournir les assertions que nécessaires pour vérifier les résultats pour son contexte de test spécifique.</span><span class="sxs-lookup"><span data-stu-id="e6313-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="e6313-300">**Figure 3**</span><span class="sxs-lookup"><span data-stu-id="e6313-300">**Figure 3**</span></span>

<span data-ttu-id="e6313-301">Le style de test et de la convention d’affectation de noms présenté ici n’est pas requis pour le code testable : il s’agit simplement d’une approche.</span><span class="sxs-lookup"><span data-stu-id="e6313-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="e6313-302">Figure 4 montre les tests en cours d’exécution dans le Resharper cerveaux Jet testent le plug-in de test runner pour Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e6313-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="e6313-304">**Figure 4**</span><span class="sxs-lookup"><span data-stu-id="e6313-304">**Figure 4**</span></span>

<span data-ttu-id="e6313-305">Avec une classe de base pour gérer le code de programme d’installation partagé, les tests unitaires pour chaque action du contrôleur sont petits et facile à écrire.</span><span class="sxs-lookup"><span data-stu-id="e6313-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="e6313-306">Les tests seront exécute rapidement (étant donné que nous exécutons des opérations en mémoire) et ne doit pas échouer en raison d’infrastructure indépendant ou des préoccupations environnementales (étant donné que nous avons isolé le test unitaire).</span><span class="sxs-lookup"><span data-stu-id="e6313-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="e6313-307">Dans ces tests, la classe de base effectue l’essentiel du travail d’installation.</span><span class="sxs-lookup"><span data-stu-id="e6313-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="e6313-308">N’oubliez pas que le constructeur de classe de base crée le référentiel en mémoire, une fausse unité de travail et une instance de la classe EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="e6313-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="e6313-309">La classe de test dérive de cette classe de base et se concentre sur les spécificités de la méthode de création de tests.</span><span class="sxs-lookup"><span data-stu-id="e6313-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="e6313-310">Dans ce cas les spécificités résument les étapes « organiser, agir et assert » s’affiche dans l’unité de procédure de test :</span><span class="sxs-lookup"><span data-stu-id="e6313-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="e6313-311">Créez un objet de nouvel employé pour simuler des données entrantes.</span><span class="sxs-lookup"><span data-stu-id="e6313-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="e6313-312">Appeler l’action de création de la EmployeeController et transmettez le nouvel employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="e6313-313">Vérifier que l’action de création génère les résultats attendus (l’employé s’affiche dans le référentiel).</span><span class="sxs-lookup"><span data-stu-id="e6313-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="e6313-314">Nous avons créé permet de tester toutes les actions EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="e6313-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="e6313-315">Par exemple, lorsque nous écrire des tests pour l’action de l’Index du contrôleur employé nous pouvons hériter de la classe de base de test pour établir la même configuration de base pour nos tests.</span><span class="sxs-lookup"><span data-stu-id="e6313-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="e6313-316">La classe de base créera une nouvelle fois le référentiel en mémoire, la fausse unité de travail et une instance de la EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="e6313-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="e6313-317">Les tests pour l’action Index suffit de vous concentrer sur l’appel de l’action de l’Index et les qualités du modèle de l’action de test retourne.</span><span class="sxs-lookup"><span data-stu-id="e6313-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="e6313-318">Les tests, nous allons créer avec InMemory fakes sont orientées vers test la *état* du logiciel.</span><span class="sxs-lookup"><span data-stu-id="e6313-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="e6313-319">Par exemple, lorsque vous testez l’action Create que nous souhaitons examiner l’état du référentiel après l’exécution de l’action create : le référentiel contient le nouvel employé ?</span><span class="sxs-lookup"><span data-stu-id="e6313-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="e6313-320">Nous examinerons plus tard interaction en fonction de test.</span><span class="sxs-lookup"><span data-stu-id="e6313-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="e6313-321">Interaction en fonction de test vous demande si le code testé appelé les méthodes appropriées sur nos objets et passé les paramètres corrects.</span><span class="sxs-lookup"><span data-stu-id="e6313-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="e6313-322">Pour l’instant, nous allons passer en couverture un autre modèle de conception : le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="e6313-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="e6313-323">Le chargement hâtif et le chargement différé</span><span class="sxs-lookup"><span data-stu-id="e6313-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="e6313-324">À un moment donné dans le site web ASP.NET MVC application que nous pourrions souhaitez afficher les informations d’un employé et inclure l’employé associé à des cartes de temps.</span><span class="sxs-lookup"><span data-stu-id="e6313-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="e6313-325">Par exemple, nous pourrions avoir un affichage du résumé des temps carte qui affiche le nom de l’employé et le nombre total de cartes de temps dans le système.</span><span class="sxs-lookup"><span data-stu-id="e6313-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="e6313-326">Il existe plusieurs approches pour implémenter cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="e6313-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="e6313-327">Projection</span><span class="sxs-lookup"><span data-stu-id="e6313-327">Projection</span></span>

<span data-ttu-id="e6313-328">Une approche facile de créer la synthèse consiste à construire un modèle dédié aux informations que nous souhaitons afficher dans la vue.</span><span class="sxs-lookup"><span data-stu-id="e6313-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="e6313-329">Dans ce scénario, le modèle peut se présenter comme suit.</span><span class="sxs-lookup"><span data-stu-id="e6313-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="e6313-330">Notez que le EmployeeSummaryViewModel n’est pas une entité – en d’autres termes qu’il n’est pas quelque chose que nous souhaitons conserver dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="e6313-331">Nous allons utiliser cette classe pour réorganiser les données dans la vue d’une manière fortement typée.</span><span class="sxs-lookup"><span data-stu-id="e6313-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="e6313-332">Le modèle de vue est comme un transfert de données (DTO) d’objet, car il ne contient aucun comportement (méthodes aucun) : seules les propriétés.</span><span class="sxs-lookup"><span data-stu-id="e6313-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="e6313-333">Les propriétés contiendra les données que nous devons déplacer.</span><span class="sxs-lookup"><span data-stu-id="e6313-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="e6313-334">Il est facile d’instancier ce modèle de vue à l’aide d’opérateur de projection standard de LINQ : l’opérateur Select.</span><span class="sxs-lookup"><span data-stu-id="e6313-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="e6313-335">Il existe deux fonctions importantes pour le code ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="e6313-335">There are two notable features to the above code.</span></span> <span data-ttu-id="e6313-336">Tout d’abord, le code est facile à tester car il est toujours facile à observer et à isoler.</span><span class="sxs-lookup"><span data-stu-id="e6313-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="e6313-337">L’opérateur Select fonctionne aussi bien par rapport à notre fakes en mémoire comme il le fait par rapport à l’unité de travail réelle.</span><span class="sxs-lookup"><span data-stu-id="e6313-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="e6313-338">La deuxième fonctionnalité notable est comment le code permet d’EF4 générer une requête unique et efficace pour assembler les informations employé et carte de temps entre eux.</span><span class="sxs-lookup"><span data-stu-id="e6313-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="e6313-339">Nous avons chargé des informations sur les employés et les informations de carte de temps dans le même objet sans utiliser des API spéciales.</span><span class="sxs-lookup"><span data-stu-id="e6313-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="e6313-340">Le code exprimé simplement les informations nécessaires à l’aide des opérateurs LINQ standard qui fonctionnent sur les sources de données en mémoire, ainsi que des sources de données distantes.</span><span class="sxs-lookup"><span data-stu-id="e6313-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="e6313-341">EF4 a été capable de convertir les arborescences d’expression générées par la requête LINQ et C\# compilateur dans une requête T-SQL unique et efficace.</span><span class="sxs-lookup"><span data-stu-id="e6313-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="e6313-342">Il existe parfois lorsque nous ne souhaitons pas travailler avec un modèle de vue ou d’un objet DTO, mais avec des entités réelles.</span><span class="sxs-lookup"><span data-stu-id="e6313-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="e6313-343">Lorsque nous savons que nous avons besoin d’un employé *et* fiches de pointage de l’employé, nous pouvons charger dynamiquement les données associées de manière efficace et discrète.</span><span class="sxs-lookup"><span data-stu-id="e6313-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="e6313-344">Chargement hâtif Explicit</span><span class="sxs-lookup"><span data-stu-id="e6313-344">Explicit Eager Loading</span></span>

<span data-ttu-id="e6313-345">Lorsque vous souhaitez charger dynamiquement des informations sur les entités connexes nous avons besoin d’un mécanisme pour la logique métier (ou dans ce scénario, la logique d’action de contrôleur) pour exprimer sa volonté dans le référentiel.</span><span class="sxs-lookup"><span data-stu-id="e6313-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="e6313-346">L’ObjectQuery EF4&lt;T&gt; classe définit une méthode Include pour spécifier les objets connexes à récupérer durant une requête.</span><span class="sxs-lookup"><span data-stu-id="e6313-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="e6313-347">N’oubliez pas l’ObjectContext EF4 expose des entités par le biais de l’ObjectSet concrète&lt;T&gt; classe qui hérite d’ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="e6313-348">Si nous utilisions ObjectSet&lt;T&gt; références dans notre action de contrôleur que nous pourrions écrire le code suivant pour spécifient un chargement hâtif des informations de carte de temps pour chaque employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="e6313-349">Toutefois, étant donné que nous essayons de garder notre code testable, nous ne sommes pas exposer ObjectSet&lt;T&gt; à partir de la véritable classe d’unité de travail à l’extérieur.</span><span class="sxs-lookup"><span data-stu-id="e6313-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="e6313-350">Au lieu de cela, nous nous appuyons sur le IObjectSet&lt;T&gt; interface qui est plus facile à falsifier, mais IObjectSet&lt;T&gt; ne définit pas une méthode Include.</span><span class="sxs-lookup"><span data-stu-id="e6313-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="e6313-351">L’avantage de LINQ est que nous pouvons créer notre propre opérateur Include.</span><span class="sxs-lookup"><span data-stu-id="e6313-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="e6313-352">Notez cet opérateur Include est défini comme une méthode d’extension pour IQueryable&lt;T&gt; au lieu de IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="e6313-353">Cela nous donne la possibilité d’utiliser la méthode avec un éventail plus large de types possibles, y compris IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;et ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="e6313-354">En cas de la séquence sous-jacente n’est pas une véritable ObjectQuery EF4&lt;T&gt;, il n’existe aucun risque effectuée et l’opérateur Include est une absence d’opération.</span><span class="sxs-lookup"><span data-stu-id="e6313-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="e6313-355">Si sous-jacent séquence *est* ObjectQuery&lt;T&gt; (ou dérivé d’ObjectQuery&lt;T&gt;), EF4 sera voir notre besoin pour les données supplémentaires et formuler SQL appropriée requête.</span><span class="sxs-lookup"><span data-stu-id="e6313-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="e6313-356">Avec ce nouvel opérateur en place, nous pouvons demander explicitement un chargement hâtif des informations de carte de temps à partir du référentiel.</span><span class="sxs-lookup"><span data-stu-id="e6313-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="e6313-357">Lors de l’exécuter par rapport à un ObjectContext réel, le code génère la requête unique.</span><span class="sxs-lookup"><span data-stu-id="e6313-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="e6313-358">La requête collecte suffisamment d’informations à partir de la base de données dans un voyage pour matérialiser les objets de l’employé et entièrement remplir leur propriété de cartes de pointage.</span><span class="sxs-lookup"><span data-stu-id="e6313-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="e6313-359">La bonne nouvelle est que le code à l’intérieur de la méthode d’action peut encore être entièrement testée.</span><span class="sxs-lookup"><span data-stu-id="e6313-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="e6313-360">Nous n’avez pas besoin de fournir des fonctionnalités supplémentaires pour notre fakes prendre en charge l’opérateur Include.</span><span class="sxs-lookup"><span data-stu-id="e6313-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="e6313-361">La mauvaise nouvelle est que nous avons dû utiliser l’opérateur d’inclure dans le code que nous souhaitons conserver persistance ignorant la.</span><span class="sxs-lookup"><span data-stu-id="e6313-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="e6313-362">Il s’agit d’un bon exemple du type de compromis, que vous devrez évaluer lors de la génération de code testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="e6313-363">Il est parfois lorsque vous avez besoin pour vous permettre de fuite de problèmes de persistance en dehors de l’abstraction de référentiel pour répondre aux objectifs de performances.</span><span class="sxs-lookup"><span data-stu-id="e6313-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="e6313-364">Le chargement hâtif consiste à chargement différé.</span><span class="sxs-lookup"><span data-stu-id="e6313-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="e6313-365">Le moyen de chargement différé que nous faisons *pas* devez notre code d’entreprise d’annoncer explicitement la configuration requise pour les données associées.</span><span class="sxs-lookup"><span data-stu-id="e6313-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="e6313-366">Au lieu de cela, nous utilisons nos entités dans l’application et si les données supplémentaires sont nécessaires Entity Framework charge les données à la demande.</span><span class="sxs-lookup"><span data-stu-id="e6313-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="e6313-367">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="e6313-367">Lazy Loading</span></span>

<span data-ttu-id="e6313-368">Il est facile d’imaginer un scénario où nous ne savons pas ce que devez données auxquelles un élément de logique métier.</span><span class="sxs-lookup"><span data-stu-id="e6313-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="e6313-369">Nous pourrions savons la logique a besoin d’un objet employee, mais nous pouvons créer une branche dans les différents chemins d’exécution où certaines de ces chemins d’accès nécessitent des informations de carte de temps à partir de l’employé, et d’autres pas.</span><span class="sxs-lookup"><span data-stu-id="e6313-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="e6313-370">Scénarios comme celui-ci sont parfaits pour implicite le chargement différé, car les données apparaissent comme par magie dans une en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="e6313-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="e6313-371">Le chargement différé, également appelé différé du chargement, place certaines exigences sur nos objets entité.</span><span class="sxs-lookup"><span data-stu-id="e6313-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="e6313-372">Oct avec ignorance de persistance true pas sont confrontées les exigences à partir de la couche de persistance, mais ignorance de persistance true est pratiquement impossible à atteindre.</span><span class="sxs-lookup"><span data-stu-id="e6313-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="e6313-373">Au lieu de cela, nous évaluons ignorance de persistance en degrés relatifs.</span><span class="sxs-lookup"><span data-stu-id="e6313-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="e6313-374">Il est malheureux s’il est nécessaire pour hériter d’une classe de base de persistance orientée services, ou utiliser une collection spécialisée pour effectuer le chargement différé dans oct.</span><span class="sxs-lookup"><span data-stu-id="e6313-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="e6313-375">Heureusement, EF4 dispose d’une solution moins intrusive.</span><span class="sxs-lookup"><span data-stu-id="e6313-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="e6313-376">Pratiquement indétectables</span><span class="sxs-lookup"><span data-stu-id="e6313-376">Virtually Undetectable</span></span>

<span data-ttu-id="e6313-377">Lorsque vous utilisez des objets POCO, EF4 peut générer dynamiquement des proxys de runtime pour les entités.</span><span class="sxs-lookup"><span data-stu-id="e6313-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="e6313-378">Ces proxys invisible encapsulent les objets oct matérialisées et fournissent des services supplémentaires en interceptant chaque propriété obtiennent et définir l’opération à effectuer du travail supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="e6313-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="e6313-379">Un de ces services est la fonctionnalité de chargement différé que nous recherchons.</span><span class="sxs-lookup"><span data-stu-id="e6313-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="e6313-380">Un autre service est un mécanisme qui peut enregistrer quand le programme modifie les valeurs de propriété d’une entité de suivi efficace.</span><span class="sxs-lookup"><span data-stu-id="e6313-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="e6313-381">La liste des modifications est utilisée par l’ObjectContext pendant la méthode SaveChanges pour conserver toutes les entités modifiées à l’aide des commandes de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="e6313-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="e6313-382">Pour ces proxies de fonctionner, cependant, dont ils ont besoin d’un moyen auquel se raccorder get de propriété et les opérations de jeu sur une entité et les proxys atteignent cet objectif en substituant les membres virtuels.</span><span class="sxs-lookup"><span data-stu-id="e6313-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="e6313-383">Par conséquent, si nous voulons qu’un chargement différé implicit et suivi des modifications efficace nous devons revenir à notre définitions de classe POCO et marquer les propriétés comme virtuels.</span><span class="sxs-lookup"><span data-stu-id="e6313-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="e6313-384">Nous pouvons toujours indiquer que l’entité Employee est principalement ignorant la persistance.</span><span class="sxs-lookup"><span data-stu-id="e6313-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="e6313-385">La seule exigence consiste à utiliser les membres virtuels, et cela n’affecte pas la testabilité du code.</span><span class="sxs-lookup"><span data-stu-id="e6313-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="e6313-386">Nous n’avez pas besoin de dériver à partir de n’importe quelle classe de base particulière, ou même utiliser une collection spéciale dédiée à chargement différé.</span><span class="sxs-lookup"><span data-stu-id="e6313-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="e6313-387">Comme illustré dans le code, toute classe qui implémente ICollection&lt;T&gt; est disponible pour contenir des entités associées.</span><span class="sxs-lookup"><span data-stu-id="e6313-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="e6313-388">Il existe également une modification mineure, que nous devons apporter à l’intérieur de notre unité de travail.</span><span class="sxs-lookup"><span data-stu-id="e6313-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="e6313-389">Le chargement différé est *hors* par défaut lorsque vous travaillez directement avec un objet ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="e6313-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="e6313-390">Il existe une propriété que nous pouvons définir sur la propriété ContextOptions pour activer le chargement différé, et nous pouvons définir cette propriété à l’intérieur de notre unité réelle de travail si vous souhaitez activer le chargement différé partout.</span><span class="sxs-lookup"><span data-stu-id="e6313-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="e6313-391">Avec implicit chargement différé est activé, le code de l’application peut utiliser un employé et l’employé associé des cartes de temps tout en restant ignorent du travail requis pour Entity Framework charger les données supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="e6313-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="e6313-392">Le chargement différé rend le code d’application plus facile à écrire, et avec la magie de proxy, le code reste complètement testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="e6313-393">Fakes en mémoire de l’unité de travail peuvent précharger simplement fausses entités avec des données associées si nécessaire pendant un test.</span><span class="sxs-lookup"><span data-stu-id="e6313-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="e6313-394">À ce stade nous porter notre attention de la création de référentiels à l’aide de IObjectSet&lt;T&gt; et examinez les abstractions pour masquer tous les signes de l’infrastructure de persistance.</span><span class="sxs-lookup"><span data-stu-id="e6313-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="e6313-395">Dépôts personnalisés</span><span class="sxs-lookup"><span data-stu-id="e6313-395">Custom Repositories</span></span>

<span data-ttu-id="e6313-396">Lorsque nous avons présenté en premier l’unité de travail du modèle de design dans cet article, que nous avons fourni un exemple de code de l’unité de travail qui peut se présenter comme.</span><span class="sxs-lookup"><span data-stu-id="e6313-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="e6313-397">Nous allons présenter cette idée d’origine à l’aide de l’employé et le scénario de carte de temps d’employé avec que nous avons travaillé.</span><span class="sxs-lookup"><span data-stu-id="e6313-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="e6313-398">La principale différence entre cette unité de travail et l’unité de travail, nous avons créé dans la dernière section est la façon dont cette unité de travail n’utilise pas les abstractions à partir de l’infrastructure d’EF4 (il n’existe aucun IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="e6313-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="e6313-399">IObjectSet&lt;T&gt; fonctionne également comme une interface de référentiel, mais l’API qu’il expose ne peut pas aligner parfaitement avec les besoins de notre application.</span><span class="sxs-lookup"><span data-stu-id="e6313-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="e6313-400">Dans cette approche à venir, nous représentera référentiels à l’aide d’un IRepository personnalisé&lt;T&gt; abstraction.</span><span class="sxs-lookup"><span data-stu-id="e6313-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="e6313-401">De nombreux développeurs qui suivent la conception pilotée par les tests, conception pilotée par comportement et la conception de méthodologies axée sur le domaine préfèrent le IRepository&lt;T&gt; approche pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="e6313-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="e6313-402">Tout d’abord, le IRepository&lt;T&gt; interface représente une couche « lutte contre la corruption ».</span><span class="sxs-lookup"><span data-stu-id="e6313-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="e6313-403">Comme décrit par Eric Evans dans son livre de conception pilotée par domaine une couche de lutte contre la corruption conserve votre code de domaine en dehors de l’infrastructure API, comme une API de persistance.</span><span class="sxs-lookup"><span data-stu-id="e6313-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="e6313-404">Deuxièmement, les développeurs peuvent créer des méthodes dans le référentiel qui répondent aux besoins exacts d’une application (comme il est détecté lors de l’écriture de tests).</span><span class="sxs-lookup"><span data-stu-id="e6313-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="e6313-405">Par exemple, il faut souvent localiser une entité unique à l’aide d’une valeur d’ID, donc nous pouvons ajouter une méthode FindById à l’interface de référentiel.</span><span class="sxs-lookup"><span data-stu-id="e6313-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="e6313-406">Notre IRepository&lt;T&gt; définition doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="e6313-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="e6313-407">Notez que nous allons déposer revenir à l’aide d’un IQueryable&lt;T&gt; interface à exposer des collections d’entités.</span><span class="sxs-lookup"><span data-stu-id="e6313-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="e6313-408">IQueryable&lt;T&gt; permet des arborescences d’expression LINQ de flux dans le fournisseur d’EF4 et de donner le fournisseur d’une vue holistique de la requête.</span><span class="sxs-lookup"><span data-stu-id="e6313-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="e6313-409">Une seconde option consisterait à retourner IEnumerable&lt;T&gt;, ce qui signifie que le fournisseur LINQ d’EF4 ne voient que les expressions intégrées à l’intérieur du référentiel.</span><span class="sxs-lookup"><span data-stu-id="e6313-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="e6313-410">Tout regroupement, de classement et de projection effectuée en dehors du référentiel ne seront pas composées dans la commande SQL envoyée à la base de données, ce qui peut nuire aux performances.</span><span class="sxs-lookup"><span data-stu-id="e6313-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="e6313-411">En revanche, un référentiel retournant uniquement IEnumerable&lt;T&gt; résultats jamais vous étonnera avec une nouvelle commande SQL.</span><span class="sxs-lookup"><span data-stu-id="e6313-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="e6313-412">Les deux approches ne fonctionnent pas, et les deux approches restent faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="e6313-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="e6313-413">Il est simple de fournir une implémentation unique de la IRepository&lt;T&gt; interface à l’aide de génériques et l’API ObjectContext EF4.</span><span class="sxs-lookup"><span data-stu-id="e6313-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="e6313-414">Le IRepository&lt;T&gt; approche nous donne un contrôle supplémentaire sur nos requêtes, car un client doit appeler une méthode pour accéder à une entité.</span><span class="sxs-lookup"><span data-stu-id="e6313-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="e6313-415">À l’intérieur de la méthode nous pouvons vous fournir des vérifications supplémentaires et des opérateurs LINQ pour appliquer des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="e6313-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="e6313-416">Notez que l’interface a deux contraintes sur le paramètre de type générique.</span><span class="sxs-lookup"><span data-stu-id="e6313-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="e6313-417">La première contrainte est le goût inconvénients classe requis par l’ObjectSet&lt;T&gt;, et la seconde contrainte force nos entités pour implémenter IEntity – une abstraction créée pour l’application.</span><span class="sxs-lookup"><span data-stu-id="e6313-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="e6313-418">L’interface IEntity force les entités aient une propriété Id lisible, et nous pouvons ensuite utiliser cette propriété dans la méthode FindById.</span><span class="sxs-lookup"><span data-stu-id="e6313-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="e6313-419">IEntity est défini par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e6313-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="e6313-420">IEntity pourrait être considéré comme une violation de petites d’ignorance de la persistance dans la mesure où notre entités sont requises pour implémenter cette interface.</span><span class="sxs-lookup"><span data-stu-id="e6313-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="e6313-421">N’oubliez pas d’ignorance de la persistance est compromis, et pour de nombreuses fonctionnalités FindById va à compenser la contrainte imposée par l’interface.</span><span class="sxs-lookup"><span data-stu-id="e6313-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="e6313-422">L’interface n’a aucun impact sur la testabilité.</span><span class="sxs-lookup"><span data-stu-id="e6313-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="e6313-423">Instanciation d’un live IRepository&lt;T&gt; nécessite un ObjectContext EF4, donc une unité concrète de l’implémentation de travail doit gérer l’instanciation.</span><span class="sxs-lookup"><span data-stu-id="e6313-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="e6313-424">À l’aide du référentiel personnalisé</span><span class="sxs-lookup"><span data-stu-id="e6313-424">Using the Custom Repository</span></span>

<span data-ttu-id="e6313-425">À l’aide de notre référentiel personnalisé n’est pas fondamentalement différent d’utiliser le référentiel selon IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="e6313-426">Au lieu d’appliquer des opérateurs LINQ directement à une propriété, nous devons tout d’abord un appeler des méthodes du référentiel pour capter IQueryable&lt;T&gt; référence.</span><span class="sxs-lookup"><span data-stu-id="e6313-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="e6313-427">Notez que l’opérateur Include personnalisé, que nous avons implémenté précédemment fonctionnera sans modification.</span><span class="sxs-lookup"><span data-stu-id="e6313-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="e6313-428">Méthode de FindById du référentiel supprime logique en double à partir d’actions tente de récupérer une entité unique.</span><span class="sxs-lookup"><span data-stu-id="e6313-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="e6313-429">Il n’existe aucune différence significative dans la testabilité des deux approches, que nous avons examiné.</span><span class="sxs-lookup"><span data-stu-id="e6313-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="e6313-430">Nous pouvons vous fournir des implémentations factices à IRepository&lt;T&gt; en créant des classes concrètes soutenus par HashSet&lt;employé&gt; , tout comme nous l’avons fait dans la dernière section.</span><span class="sxs-lookup"><span data-stu-id="e6313-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="e6313-431">Toutefois, certains développeurs préfèrent utiliser des objets fictifs et simuler les infrastructures d’objets au lieu de créer des substituts.</span><span class="sxs-lookup"><span data-stu-id="e6313-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="e6313-432">Nous allons examiner à l’aide d’objets fictifs pour tester notre implémentation et traite des différences entre les objets fictifs et aux éléments fictifs dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e6313-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="e6313-433">Tests avec les simulacres</span><span class="sxs-lookup"><span data-stu-id="e6313-433">Testing with Mocks</span></span>

<span data-ttu-id="e6313-434">Il existe différentes approches pour créer les appels de Martin Fowler un « double de test ».</span><span class="sxs-lookup"><span data-stu-id="e6313-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="e6313-435">Un test en double (par exemple, un film de phares double) est un objet que vous générez pour « remplacer » pour le real, les objets de production pendant les tests.</span><span class="sxs-lookup"><span data-stu-id="e6313-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="e6313-436">Les dépôts en mémoire que nous avons créé sont des doubles de test pour les référentiels de communiquer avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e6313-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="e6313-437">Nous avons vu comment utiliser ces doubles de test pendant les tests unitaires pour isoler le code et conserver les tests en cours d’exécution rapide.</span><span class="sxs-lookup"><span data-stu-id="e6313-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="e6313-438">Les doubles de test que nous avons créé ont des implémentations réelles, l’utilisation.</span><span class="sxs-lookup"><span data-stu-id="e6313-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="e6313-439">Dans les coulisses, chacune d’elles stocke une collection d’objets concrets, et ils seront ajouter et supprimer des objets de cette collection comme nous manipuler le référentiel pendant un test.</span><span class="sxs-lookup"><span data-stu-id="e6313-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="e6313-440">Certains développeurs préfèrent créer leurs doubles de test ainsi – avec le code réel et les implémentations de travail.</span><span class="sxs-lookup"><span data-stu-id="e6313-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="e6313-441">Ces doubles de test sont ce que nous appelons *fakes*.</span><span class="sxs-lookup"><span data-stu-id="e6313-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="e6313-442">Ils ont des implémentations de travail, mais ils ne sont pas suffisamment réels pour la production.</span><span class="sxs-lookup"><span data-stu-id="e6313-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="e6313-443">Le référentiel factice n’écrit pas vraiment à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e6313-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="e6313-444">Le serveur SMTP faux n’envoyer un message électronique sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="e6313-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="e6313-445">Objets fictifs et Fakes</span><span class="sxs-lookup"><span data-stu-id="e6313-445">Mocks versus Fakes</span></span>

<span data-ttu-id="e6313-446">Il existe un autre type de test appelé double un *simuler*.</span><span class="sxs-lookup"><span data-stu-id="e6313-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="e6313-447">Tandis que fakes ont des implémentations de travail, les simulacres sont fournis avec aucune implémentation.</span><span class="sxs-lookup"><span data-stu-id="e6313-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="e6313-448">À l’aide d’une infrastructure de l’objet factice nous construire ces objets fictifs au moment de l’exécution et les utiliser comme des doubles de test.</span><span class="sxs-lookup"><span data-stu-id="e6313-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="e6313-449">Dans cette section, nous allons utiliser open source framework Moq de simulation.</span><span class="sxs-lookup"><span data-stu-id="e6313-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="e6313-450">Voici un exemple simple d’utilisation Moq pour créer dynamiquement un test double pour un référentiel de l’employé.</span><span class="sxs-lookup"><span data-stu-id="e6313-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="e6313-451">Nous demandons un IRepository Moq&lt;employé&gt; implémentation et il génère un dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="e6313-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="e6313-452">Nous pouvons obtenir à l’objet implémentant IRepository&lt;employé&gt; en accédant à la propriété d’objet de l’objet factice&lt;T&gt; objet.</span><span class="sxs-lookup"><span data-stu-id="e6313-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="e6313-453">Il est cet objet interne, que nous pouvons passer dans nos contrôleurs, et ils ne sont pas savoir s’il s’agit d’un double de test ou dans le référentiel réel.</span><span class="sxs-lookup"><span data-stu-id="e6313-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="e6313-454">Comme nous serait appeler des méthodes sur un objet avec une implémentation réelle, nous pouvons appeler des méthodes sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="e6313-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="e6313-455">Vous devez vous demandez peut-être ce que le référentiel factice faire lorsque nous appelons la méthode Add.</span><span class="sxs-lookup"><span data-stu-id="e6313-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="e6313-456">Dans la mesure où il n’existe aucune implémentation derrière l’objet factice, ajouter ne fait rien.</span><span class="sxs-lookup"><span data-stu-id="e6313-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="e6313-457">Il n’existe aucune collection concrets dans les coulisses tels que nous avions avec les fakes que nous avons écrit, l’employé est supprimé.</span><span class="sxs-lookup"><span data-stu-id="e6313-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="e6313-458">Qu’en est-il de la valeur de retour de FindById ?</span><span class="sxs-lookup"><span data-stu-id="e6313-458">What about the return value of FindById?</span></span> <span data-ttu-id="e6313-459">Dans ce cas l’objet factice effectue la seule chose que faire, c'est-à-dire en retournée une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="e6313-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="e6313-460">Étant donné que nous allons retourner un type référence (un employé), la valeur de retour est une valeur null.</span><span class="sxs-lookup"><span data-stu-id="e6313-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="e6313-461">Objets fictifs peuvent sembler inutiles ; Toutefois, il existe de nouvelles fonctionnalités de simulacres, que nous n’avons pas parlé.</span><span class="sxs-lookup"><span data-stu-id="e6313-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="e6313-462">Tout d’abord, le framework Moq enregistre tous les appels effectués sur l’objet factice.</span><span class="sxs-lookup"><span data-stu-id="e6313-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="e6313-463">Plus loin dans le code, nous pouvons poser Moq si tout le monde appelé la méthode Add, ou si tout le monde a appelé la méthode FindById.</span><span class="sxs-lookup"><span data-stu-id="e6313-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="e6313-464">Nous verrons plus tard comment nous pouvons utiliser cette fonctionnalité d’enregistrement de « boîte noire » dans les tests.</span><span class="sxs-lookup"><span data-stu-id="e6313-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="e6313-465">La deuxième fonctionnalité intéressante est comment nous pouvons utiliser Moq à programmer un objet factice avec *attentes*.</span><span class="sxs-lookup"><span data-stu-id="e6313-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="e6313-466">Une attente indique à l’objet factice comment répondre à toute interaction donnée.</span><span class="sxs-lookup"><span data-stu-id="e6313-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="e6313-467">Par exemple, nous pouvons programmer une attente dans notre simulacre et lui dire de retourner un objet employee lorsque quelqu'un appelle FindById.</span><span class="sxs-lookup"><span data-stu-id="e6313-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="e6313-468">Le framework Moq utilise une API d’installation et les expressions lambda à programmer ces attentes.</span><span class="sxs-lookup"><span data-stu-id="e6313-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="e6313-469">Dans cet exemple, nous demandons Moq pour générer dynamiquement un référentiel, puis nous utilisons le référentiel avec une attente.</span><span class="sxs-lookup"><span data-stu-id="e6313-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="e6313-470">L’attente indique à l’objet factice pour retourner un nouvel objet employé avec une valeur d’Id de 5 lorsque quelqu'un appelle la méthode FindById en passant une valeur de 5.</span><span class="sxs-lookup"><span data-stu-id="e6313-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="e6313-471">Ce test réussisse, et nous n’avions pas besoin générer une implémentation complète à faux IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="e6313-472">Nous allons revoir les tests que nous avons écrit précédemment et reprise du travail pour utiliser les simulacres au lieu de fakes.</span><span class="sxs-lookup"><span data-stu-id="e6313-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="e6313-473">Comme avant, nous allons utiliser une classe de base pour configurer les éléments courants de l’infrastructure que nous avons besoin pour tous les tests du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e6313-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="e6313-474">Le code d’installation reste essentiellement la même.</span><span class="sxs-lookup"><span data-stu-id="e6313-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="e6313-475">Au lieu d’utiliser fakes, nous allons utiliser Moq pour construire des objets fictifs.</span><span class="sxs-lookup"><span data-stu-id="e6313-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="e6313-476">La classe de base fait en sorte que la simulacre unité de travail retourner un référentiel factice lorsque le code appelle la propriété d’employés.</span><span class="sxs-lookup"><span data-stu-id="e6313-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="e6313-477">Le reste de l’Assistant Déploiement aura lieu dans les contextes de test dédié à chaque scénario spécifique.</span><span class="sxs-lookup"><span data-stu-id="e6313-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="e6313-478">Par exemple, le contexte de test pour l’action Index va configurer le référentiel factice pour retourner une liste des employés lors de l’action appelle la méthode FindAll du référentiel fictif.</span><span class="sxs-lookup"><span data-stu-id="e6313-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="e6313-479">Nos tests ressembler à l’exception des prévisions, pour les tests que nous avions auparavant.</span><span class="sxs-lookup"><span data-stu-id="e6313-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="e6313-480">Toutefois, avec la possibilité de l’enregistrement d’une infrastructure factice nous pouvons approche du test à partir d’un angle différent.</span><span class="sxs-lookup"><span data-stu-id="e6313-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="e6313-481">Nous allons examiner cette nouvelle perspective dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e6313-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="e6313-482">État par rapport à l’Interaction de test</span><span class="sxs-lookup"><span data-stu-id="e6313-482">State versus Interaction Testing</span></span>

<span data-ttu-id="e6313-483">Il existe différentes techniques que vous pouvez utiliser pour tester les logiciels avec des objets fictifs.</span><span class="sxs-lookup"><span data-stu-id="e6313-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="e6313-484">Une approche consiste à utiliser l’état en fonction de test, qui est ce que nous avons fait dans ce document.</span><span class="sxs-lookup"><span data-stu-id="e6313-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="e6313-485">État en fonction des assertions de test rend sur l’état du logiciel.</span><span class="sxs-lookup"><span data-stu-id="e6313-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="e6313-486">Dans le dernier test nous appelé une méthode d’action sur le contrôleur et effectué une assertion sur le modèle, qu'il doit être généré.</span><span class="sxs-lookup"><span data-stu-id="e6313-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="e6313-487">Voici quelques autres exemples d’état de test :</span><span class="sxs-lookup"><span data-stu-id="e6313-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="e6313-488">Vérifiez que le référentiel contient le nouvel objet employé après l’exécution de Create.</span><span class="sxs-lookup"><span data-stu-id="e6313-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="e6313-489">Vérifiez que le modèle conserve une liste de tous les employés après l’exécution de l’Index.</span><span class="sxs-lookup"><span data-stu-id="e6313-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="e6313-490">Vérifiez que le référentiel ne contient pas un employé donné après l’exécution de la suppression.</span><span class="sxs-lookup"><span data-stu-id="e6313-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="e6313-491">Une autre approche, vous verrez par des objets fictifs consiste à vérifier *interactions*.</span><span class="sxs-lookup"><span data-stu-id="e6313-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="e6313-492">Alors que l’état en fonction des assertions de test rend sur l’état des objets, interaction en fonction des assertions de test rend sur la façon dont les objets interagissent.</span><span class="sxs-lookup"><span data-stu-id="e6313-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="e6313-493">Exemple :</span><span class="sxs-lookup"><span data-stu-id="e6313-493">For example:</span></span>

-   <span data-ttu-id="e6313-494">Vérifiez que le contrôleur appelle méthode Add du référentiel lors de l’exécution de Create.</span><span class="sxs-lookup"><span data-stu-id="e6313-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="e6313-495">Vérifiez que le contrôleur appelle la méthode FindAll du référentiel lors de l’Index s’exécute.</span><span class="sxs-lookup"><span data-stu-id="e6313-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="e6313-496">Vérifiez que le contrôleur appelle l’unité de la méthode de validation du travail pour enregistrer les modifications lors de la modification s’exécute.</span><span class="sxs-lookup"><span data-stu-id="e6313-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="e6313-497">Test de l’interaction nécessite souvent moins de données test, car nous ne l’exploration à l’intérieur des collections et la vérification des nombres.</span><span class="sxs-lookup"><span data-stu-id="e6313-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="e6313-498">Par exemple, si nous savons que l’action détails appelle la méthode de FindById d’un référentiel avec la valeur correcte, puis l’action est probablement qu’elles comportent correctement.</span><span class="sxs-lookup"><span data-stu-id="e6313-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="e6313-499">Nous pouvons vérifier ce comportement sans configurer les données de test à retourner à partir de FindById.</span><span class="sxs-lookup"><span data-stu-id="e6313-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="e6313-500">Le programme d’installation uniquement requis dans le contexte de test ci-dessus est le programme d’installation fourni par la classe de base.</span><span class="sxs-lookup"><span data-stu-id="e6313-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="e6313-501">Lorsque nous appelons l’action du contrôleur, Moq enregistre les interactions avec le référentiel factice.</span><span class="sxs-lookup"><span data-stu-id="e6313-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="e6313-502">À l’aide de l’API de Moq vérifier, nous pouvons poser Moq si le contrôleur appelé FindById avec la valeur d’ID appropriée.</span><span class="sxs-lookup"><span data-stu-id="e6313-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="e6313-503">Si le contrôleur n’a pas appelé la méthode, ou a appelé la méthode avec une valeur de paramètre inattendu, la méthode Vérifiez lève une exception et le test échoue.</span><span class="sxs-lookup"><span data-stu-id="e6313-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="e6313-504">Voici un autre exemple pour vérifier que l’action Create appelle la validation sur l’unité de travail en cours.</span><span class="sxs-lookup"><span data-stu-id="e6313-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="e6313-505">L’un des risques avec les tests d’interaction sont la tendance à sur spécifient les interactions.</span><span class="sxs-lookup"><span data-stu-id="e6313-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="e6313-506">La capacité de l’objet factice pour enregistrer et de vérifier chaque interaction avec l’objet factice ne signifie pas que le test doit tenter de vérifier chaque interaction.</span><span class="sxs-lookup"><span data-stu-id="e6313-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="e6313-507">Certaines interactions sont des détails d’implémentation et vous devez vérifier seulement les interactions *requis* pour satisfaire le test actuel.</span><span class="sxs-lookup"><span data-stu-id="e6313-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="e6313-508">Le choix entre les objets fictifs ou fakes dépend en grande partie du système que vous testez et votre personnel (ou de l’équipe) Préférences.</span><span class="sxs-lookup"><span data-stu-id="e6313-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="e6313-509">Objets fictifs peuvent réduire considérablement la quantité de code, que vous devez implémenter des doubles de test, mais pas tout le monde est à l’aise attentes en matière de programmation et la vérification des interactions.</span><span class="sxs-lookup"><span data-stu-id="e6313-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="e6313-510">Conclusions</span><span class="sxs-lookup"><span data-stu-id="e6313-510">Conclusions</span></span>

<span data-ttu-id="e6313-511">Dans ce document, nous avons présenté plusieurs approches de création de code testable lors de l’utilisation d’ADO.NET Entity Framework pour la persistance des données.</span><span class="sxs-lookup"><span data-stu-id="e6313-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="e6313-512">Nous pouvons tirer parti des abstractions intégrées comme IObjectSet&lt;T&gt;, ou créer nos propres abstractions comme IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="e6313-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="e6313-513">Dans les deux cas, la prise en charge POCO dans ADO.NET Entity Framework 4.0 permet les consommateurs de ces abstractions de rester persistant ignorant l’et hautement testable.</span><span class="sxs-lookup"><span data-stu-id="e6313-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="e6313-514">Fonctionnalités d’EF4 supplémentaires telles que le chargement différé implicit permet d’entreprise et d’application de service du code de fonctionner sans se préoccuper de savoir les détails d’un magasin de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="e6313-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="e6313-515">Enfin, les abstractions que nous créons sont faciles à fictifs ou faux à l’intérieur des tests unitaires, et nous pouvons utiliser ces doubles de test pour atteindre rapides en cours d’exécution, hautement isolée et les tests fiables.</span><span class="sxs-lookup"><span data-stu-id="e6313-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="e6313-516">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e6313-516">Additional Resources</span></span>

-   <span data-ttu-id="e6313-517">Martin, « [le principe de responsabilité unique](http://www.objectmentor.com/resources/articles/srp.pdf)»</span><span class="sxs-lookup"><span data-stu-id="e6313-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="e6313-518">Martin Fowler, [catalogue de modèles](http://www.martinfowler.com/eaaCatalog/index.html) de *modèles d’Architecture d’Application Enterprise*</span><span class="sxs-lookup"><span data-stu-id="e6313-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="e6313-519">Caprio Griffin, « [l’Injection de dépendances](https://msdn.microsoft.com/magazine/cc163739.aspx)»</span><span class="sxs-lookup"><span data-stu-id="e6313-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="e6313-520">Blog de programmabilité de données, « [procédure pas à pas : développement avec Entity Framework 4.0 piloté par Test](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)».</span><span class="sxs-lookup"><span data-stu-id="e6313-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="e6313-521">Blog de programmabilité de données, « [des modèles à l’aide de référentiel et unité de travail avec Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)»</span><span class="sxs-lookup"><span data-stu-id="e6313-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="e6313-522">Dave Astels, « [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)»</span><span class="sxs-lookup"><span data-stu-id="e6313-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="e6313-523">Aaron Jensen, « [introduisant des spécifications Machine](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)»</span><span class="sxs-lookup"><span data-stu-id="e6313-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="e6313-524">Eric Lee, « [BDD avec MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)»</span><span class="sxs-lookup"><span data-stu-id="e6313-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="e6313-525">Eric Evans, « [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)»</span><span class="sxs-lookup"><span data-stu-id="e6313-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="e6313-526">Martin Fowler, « [Mocks aren ' t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)»</span><span class="sxs-lookup"><span data-stu-id="e6313-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="e6313-527">Martin Fowler, « [Double de Test](http://martinfowler.com/bliki/TestDouble.html)»</span><span class="sxs-lookup"><span data-stu-id="e6313-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="e6313-528">Jeremy Miller, « [état par rapport à l’Interaction test](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)»</span><span class="sxs-lookup"><span data-stu-id="e6313-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="e6313-529">Moq</span><span class="sxs-lookup"><span data-stu-id="e6313-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="e6313-530">Biographie</span><span class="sxs-lookup"><span data-stu-id="e6313-530">Biography</span></span>

<span data-ttu-id="e6313-531">Scott Allen est un membre du personnel technique chez Pluralsight et fondateur de OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="e6313-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="e6313-532">Dans les 15 années de développement de logiciels commerciaux, Scott a travaillé sur des solutions pour tous les éléments à partir de périphériques intégrés de 8 bits pour les applications web ASP.NET hautement évolutives.</span><span class="sxs-lookup"><span data-stu-id="e6313-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="e6313-533">Vous pouvez contacter Scott sur son blog à l’adresse OdeToCode ou sur Twitter à l’adresse [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="e6313-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
