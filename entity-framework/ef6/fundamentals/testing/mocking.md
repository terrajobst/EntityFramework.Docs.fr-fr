---
title: Test avec une infrastructure de simulation - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
caps.latest.revision: 3
ms.openlocfilehash: 7529929a3ed3906e1201c0899f2fb8b959ec9ed2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121343"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="12ecc-102">Test avec une infrastructure de simulation</span><span class="sxs-lookup"><span data-stu-id="12ecc-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="12ecc-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="12ecc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="12ecc-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="12ecc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="12ecc-105">Lors de l’écriture de tests pour votre application, il est souvent souhaitable d’éviter d’atteindre la base de données.</span><span class="sxs-lookup"><span data-stu-id="12ecc-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="12ecc-106">Entity Framework vous permet de pour y parvenir en créant un contexte : avec le comportement défini par vos tests, qui utilise des données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="12ecc-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="12ecc-107">Options de création de doubles de test</span><span class="sxs-lookup"><span data-stu-id="12ecc-107">Options for creating test doubles</span></span>  

<span data-ttu-id="12ecc-108">Il existe deux approches différentes qui peuvent être utilisés pour créer une version en mémoire de votre contexte.</span><span class="sxs-lookup"><span data-stu-id="12ecc-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="12ecc-109">**Créer votre propre doubles de test** – cette approche implique d’écrire votre propre implémentation en mémoire de votre contexte et de DbSets.</span><span class="sxs-lookup"><span data-stu-id="12ecc-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="12ecc-110">Cela vous donne un grand nombre de contrôle sur la façon dont les classes se comportent mais peuvent impliquer écriture et qui possède une quantité raisonnable de code.</span><span class="sxs-lookup"><span data-stu-id="12ecc-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="12ecc-111">**Utiliser une infrastructure de simulation pour créer des doubles de test** – à l’aide d’une infrastructure de simulation (par exemple, Moq) vous pouvez avoir les implémentations en mémoire de votre contexte et des jeux créés dynamiquement lors de l’exécution pour vous.</span><span class="sxs-lookup"><span data-stu-id="12ecc-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="12ecc-112">Cet article traitera d’à l’aide d’une infrastructure de simulation.</span><span class="sxs-lookup"><span data-stu-id="12ecc-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="12ecc-113">Pour créer votre propre doubles de test, consultez [test avec votre propre Doubles de Test](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="12ecc-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="12ecc-114">Pour illustrer l’utilisation d’EF avec une infrastructure de simulation, nous allons utiliser Moq.</span><span class="sxs-lookup"><span data-stu-id="12ecc-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="12ecc-115">Le moyen le plus simple d’obtenir Moq consiste à installer le [Moq un package à partir de NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="12ecc-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="12ecc-116">Test avec les versions pre-EF6</span><span class="sxs-lookup"><span data-stu-id="12ecc-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="12ecc-117">Le scénario illustré dans cet article dépend de certaines modifications que nous avons apporté à DbSet dans EF6.</span><span class="sxs-lookup"><span data-stu-id="12ecc-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="12ecc-118">Pour le test avec EF5 et version antérieure, consultez [test avec un contexte fictif](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="12ecc-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="12ecc-119">Limitations EF en mémoire des doubles de test</span><span class="sxs-lookup"><span data-stu-id="12ecc-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="12ecc-120">Les doubles de test en mémoire peut être un bon moyen de fournir une couverture au niveau des bits de votre application qui utilisent Entity Framework de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="12ecc-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="12ecc-121">Toutefois, lors de cette opération vous utilisez LINQ to Objects pour exécuter des requêtes sur les données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="12ecc-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="12ecc-122">Cela peut entraîner un comportement différent par rapport à l’aide du fournisseur LINQ d’Entity Framework (LINQ to Entities) pour traduire des requêtes dans SQL est exécutée sur votre base de données.</span><span class="sxs-lookup"><span data-stu-id="12ecc-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="12ecc-123">Un exemple de cette différence est le chargement des données connexes.</span><span class="sxs-lookup"><span data-stu-id="12ecc-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="12ecc-124">Si vous créez une série de Blogs que chaque ont billets de blog, puis lors de l’utilisation des données en mémoire les messages liés sont toujours chargés pour chaque Blog.</span><span class="sxs-lookup"><span data-stu-id="12ecc-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="12ecc-125">Toutefois, lors de l’exécution par rapport à une base de données les données sont chargées uniquement si vous utilisez la méthode Include.</span><span class="sxs-lookup"><span data-stu-id="12ecc-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="12ecc-126">Pour cette raison, il est recommandé de toujours inclure un niveau de bout en bout à tester (en plus de vos tests unitaires) pour s’assurer de votre application fonctionne correctement sur une base de données.</span><span class="sxs-lookup"><span data-stu-id="12ecc-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="12ecc-127">Poursuivez la procédure avec cet article</span><span class="sxs-lookup"><span data-stu-id="12ecc-127">Following along with this article</span></span>  

<span data-ttu-id="12ecc-128">Cet article donne des listes de code complet, vous pouvez copier dans Visual Studio pour suivre la procédure si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="12ecc-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="12ecc-129">Il est plus facile de créer un **projet de Test unitaire** et vous devez à la cible **.NET Framework 4.5** pour terminer les sections qui utilisent async.</span><span class="sxs-lookup"><span data-stu-id="12ecc-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="12ecc-130">Le modèle EF</span><span class="sxs-lookup"><span data-stu-id="12ecc-130">The EF model</span></span>  

<span data-ttu-id="12ecc-131">Le service que nous allons tester utilise une EF modèle constitué de le BloggingContext et les classes de Blog et Post.</span><span class="sxs-lookup"><span data-stu-id="12ecc-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="12ecc-132">Ce code peut avoir été généré par le Concepteur EF ou être un modèle Code First.</span><span class="sxs-lookup"><span data-stu-id="12ecc-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="12ecc-133">Propriétés DbSet virtuel avec Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="12ecc-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="12ecc-134">Notez que les propriétés DbSet sur le contexte sont marquées comme virtuelles.</span><span class="sxs-lookup"><span data-stu-id="12ecc-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="12ecc-135">Ainsi, l’infrastructure de simulation dériver à partir de notre contexte et en remplaçant ces propriétés avec une implémentation factice.</span><span class="sxs-lookup"><span data-stu-id="12ecc-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="12ecc-136">Si vous utilisez Code First vous pouvez modifier vos classes directement.</span><span class="sxs-lookup"><span data-stu-id="12ecc-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="12ecc-137">Si vous utilisez le Concepteur EF vous devrez modifier le modèle T4 qui génère votre contexte.</span><span class="sxs-lookup"><span data-stu-id="12ecc-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="12ecc-138">Ouvrez le \<model_name\>. Fichier context.tt qui est imbriqué sous vous edmx le fichier, recherchez le fragment de code suivant et ajoutez le mot clé virtuel comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="12ecc-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="12ecc-139">Service à tester</span><span class="sxs-lookup"><span data-stu-id="12ecc-139">Service to be tested</span></span>  

<span data-ttu-id="12ecc-140">Pour illustrer le test avec des doubles de test en mémoire, nous allons écrire quelques tests pour un BlogService.</span><span class="sxs-lookup"><span data-stu-id="12ecc-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="12ecc-141">Le service est capable de créer de nouveaux blogs (AddBlog) et retour de tous les Blogs classés par nom (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="12ecc-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="12ecc-142">En plus de GetAllBlogs, nous vous proposons également une méthode qui sera obtenir de façon asynchrone tous les blogs sont classés par nom (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="12ecc-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="12ecc-143">Requête non scénarios de test</span><span class="sxs-lookup"><span data-stu-id="12ecc-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="12ecc-144">C’est tout que nous devons faire pour commencer à tester des méthodes de requête non.</span><span class="sxs-lookup"><span data-stu-id="12ecc-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="12ecc-145">Le test suivant utilise Moq pour créer un contexte.</span><span class="sxs-lookup"><span data-stu-id="12ecc-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="12ecc-146">Il crée ensuite un DbSet\<Blog\> et relie à retourner à partir de la propriété de Blogs du contexte.</span><span class="sxs-lookup"><span data-stu-id="12ecc-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="12ecc-147">Ensuite, le contexte est utilisé pour créer un nouveau BlogService qui est ensuite utilisé pour créer un nouveau blog – à l’aide de la méthode AddBlog.</span><span class="sxs-lookup"><span data-stu-id="12ecc-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="12ecc-148">Enfin, le test vérifie que le service ajouté un nouveau Blog et appelé SaveChanges sur le contexte.</span><span class="sxs-lookup"><span data-stu-id="12ecc-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="12ecc-149">Scénarios de requête de test</span><span class="sxs-lookup"><span data-stu-id="12ecc-149">Testing query scenarios</span></span>  

<span data-ttu-id="12ecc-150">Pour pouvoir exécuter des requêtes sur notre double de test DbSet, nous devons configurer une implémentation d’IQueryable.</span><span class="sxs-lookup"><span data-stu-id="12ecc-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="12ecc-151">La première étape consiste à créer des données en mémoire – nous utilisons une liste\<Blog\>.</span><span class="sxs-lookup"><span data-stu-id="12ecc-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="12ecc-152">Ensuite, nous permet de créer un contexte et le DBSet\<Blog\> puis rattachez l’implémentation d’IQueryable pour le DbSet – ils simplement délégation pour le fournisseur LINQ to Objects qui fonctionne avec la liste\<T\>.</span><span class="sxs-lookup"><span data-stu-id="12ecc-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="12ecc-153">Nous pouvons ensuite créer un BlogService selon notre doubles de test et vous assurer que les données que nous obtenons à partir de GetAllBlogs sont triées par nom.</span><span class="sxs-lookup"><span data-stu-id="12ecc-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="12ecc-154">Test avec des requêtes asynchrones</span><span class="sxs-lookup"><span data-stu-id="12ecc-154">Testing with async queries</span></span>

<span data-ttu-id="12ecc-155">Entity Framework 6 a introduit un ensemble de méthodes d’extension qui peut être utilisé pour exécuter de façon asynchrone une requête.</span><span class="sxs-lookup"><span data-stu-id="12ecc-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="12ecc-156">ToListAsync, FirstAsync, ForEachAsync, etc. sont des exemples de ces méthodes.</span><span class="sxs-lookup"><span data-stu-id="12ecc-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="12ecc-157">Étant donné que les requêtes Entity Framework utiliser LINQ, les méthodes d’extension sont définies sur IQueryable et IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="12ecc-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="12ecc-158">Toutefois, car ils sont uniquement conçus pour être utilisé avec Entity Framework, vous pouvez recevoir l’erreur suivante si vous tentez de les utiliser sur une requête LINQ qui n’est pas une requête Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="12ecc-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="12ecc-159">La source IQueryable n’implémente pas IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="12ecc-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="12ecc-160">Uniquement les sources qui implémentent IDbAsyncEnumerable peuvent être utilisées pour les opérations asynchrones d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12ecc-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="12ecc-161">Pour plus d’informations, consultez [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="12ecc-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="12ecc-162">Tandis que les méthodes asynchrones sont uniquement pris en charge en cours d’exécution par rapport à une requête Entity Framework, vous souhaiterez utilisez-les dans votre test unitaire en cours d’exécution par rapport à une mémoire tester double d’un DbSet.</span><span class="sxs-lookup"><span data-stu-id="12ecc-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="12ecc-163">Pour pouvoir utiliser les méthodes asynchrones, nous devons créer un DbAsyncQueryProvider en mémoire pour traiter la requête asynchrone.</span><span class="sxs-lookup"><span data-stu-id="12ecc-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="12ecc-164">Tout en il serait possible de configurer un fournisseur de requête à l’aide de Moq, il est beaucoup plus facile de créer une implémentation de doubles de test dans le code.</span><span class="sxs-lookup"><span data-stu-id="12ecc-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="12ecc-165">Le code pour cette implémentation est la suivante :</span><span class="sxs-lookup"><span data-stu-id="12ecc-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

<span data-ttu-id="12ecc-166">Maintenant que nous disposons d’un fournisseur de requête async, nous pouvons écrire un test unitaire pour notre nouvelle méthode GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="12ecc-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
