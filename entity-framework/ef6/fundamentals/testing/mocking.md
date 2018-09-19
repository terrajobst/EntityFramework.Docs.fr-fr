---
title: Test avec une infrastructure de simulation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 20799b55b2dffe27637c4fb84df06cee174e6dd9
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284094"
---
# <a name="testing-with-a-mocking-framework"></a>Test avec une infrastructure de simulation
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Lors de l’écriture de tests pour votre application, il est souvent souhaitable d’éviter d’atteindre la base de données.  Entity Framework vous permet de pour y parvenir en créant un contexte : avec le comportement défini par vos tests, qui utilise des données en mémoire.  

## <a name="options-for-creating-test-doubles"></a>Options de création de doubles de test  

Il existe deux approches différentes qui peuvent être utilisés pour créer une version en mémoire de votre contexte.  

- **Créer votre propre doubles de test** – cette approche implique d’écrire votre propre implémentation en mémoire de votre contexte et de DbSets. Cela vous donne un grand nombre de contrôle sur la façon dont les classes se comportent mais peuvent impliquer écriture et qui possède une quantité raisonnable de code.  
- **Utiliser une infrastructure de simulation pour créer des doubles de test** – à l’aide d’une infrastructure de simulation (par exemple, Moq) vous pouvez avoir les implémentations en mémoire de votre contexte et des jeux créés dynamiquement lors de l’exécution pour vous.  

Cet article traitera d’à l’aide d’une infrastructure de simulation. Pour créer votre propre doubles de test, consultez [test avec votre propre Doubles de Test](writing-test-doubles.md).  

Pour illustrer l’utilisation d’EF avec une infrastructure de simulation, nous allons utiliser Moq. Le moyen le plus simple d’obtenir Moq consiste à installer le [Moq un package à partir de NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Test avec les versions pre-EF6  

Le scénario illustré dans cet article dépend de certaines modifications que nous avons apporté à DbSet dans EF6. Pour le test avec EF5 et version antérieure, consultez [test avec un contexte fictif](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitations EF en mémoire des doubles de test  

Les doubles de test en mémoire peut être un bon moyen de fournir une couverture au niveau des bits de votre application qui utilisent Entity Framework de test unitaire. Toutefois, lors de cette opération vous utilisez LINQ to Objects pour exécuter des requêtes sur les données en mémoire. Cela peut entraîner un comportement différent par rapport à l’aide du fournisseur LINQ d’Entity Framework (LINQ to Entities) pour traduire des requêtes dans SQL est exécutée sur votre base de données.  

Un exemple de cette différence est le chargement des données connexes. Si vous créez une série de Blogs que chaque ont billets de blog, puis lors de l’utilisation des données en mémoire les messages liés sont toujours chargés pour chaque Blog. Toutefois, lors de l’exécution par rapport à une base de données les données sont chargées uniquement si vous utilisez la méthode Include.  

Pour cette raison, il est recommandé de toujours inclure un niveau de bout en bout à tester (en plus de vos tests unitaires) pour s’assurer de votre application fonctionne correctement sur une base de données.  

## <a name="following-along-with-this-article"></a>Poursuivez la procédure avec cet article  

Cet article donne des listes de code complet, vous pouvez copier dans Visual Studio pour suivre la procédure si vous le souhaitez. Il est plus facile de créer un **projet de Test unitaire** et vous devez à la cible **.NET Framework 4.5** pour terminer les sections qui utilisent async.  

## <a name="the-ef-model"></a>Le modèle EF  

Le service que nous allons tester utilise une EF modèle constitué de le BloggingContext et les classes de Blog et Post. Ce code peut avoir été généré par le Concepteur EF ou être un modèle Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Propriétés DbSet virtuel avec Entity Framework Designer  

Notez que les propriétés DbSet sur le contexte sont marquées comme virtuelles. Ainsi, l’infrastructure de simulation dériver à partir de notre contexte et en remplaçant ces propriétés avec une implémentation factice.  

Si vous utilisez Code First vous pouvez modifier vos classes directement. Si vous utilisez le Concepteur EF vous devrez modifier le modèle T4 qui génère votre contexte. Ouvrez le \<model_name\>. Fichier context.tt qui est imbriqué sous vous edmx le fichier, recherchez le fragment de code suivant et ajoutez le mot clé virtuel comme indiqué.  

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

## <a name="service-to-be-tested"></a>Service à tester  

Pour illustrer le test avec des doubles de test en mémoire, nous allons écrire quelques tests pour un BlogService. Le service est capable de créer de nouveaux blogs (AddBlog) et retour de tous les Blogs classés par nom (GetAllBlogs). En plus de GetAllBlogs, nous vous proposons également une méthode qui sera obtenir de façon asynchrone tous les blogs sont classés par nom (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Requête non scénarios de test  

C’est tout que nous devons faire pour commencer à tester des méthodes de requête non. Le test suivant utilise Moq pour créer un contexte. Il crée ensuite un DbSet\<Blog\> et relie à retourner à partir de la propriété de Blogs du contexte. Ensuite, le contexte est utilisé pour créer un nouveau BlogService qui est ensuite utilisé pour créer un nouveau blog – à l’aide de la méthode AddBlog. Enfin, le test vérifie que le service ajouté un nouveau Blog et appelé SaveChanges sur le contexte.  

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

## <a name="testing-query-scenarios"></a>Scénarios de requête de test  

Pour pouvoir exécuter des requêtes sur notre double de test DbSet, nous devons configurer une implémentation d’IQueryable. La première étape consiste à créer des données en mémoire – nous utilisons une liste\<Blog\>. Ensuite, nous permet de créer un contexte et le DBSet\<Blog\> puis rattachez l’implémentation d’IQueryable pour le DbSet – ils simplement délégation pour le fournisseur LINQ to Objects qui fonctionne avec la liste\<T\>.  

Nous pouvons ensuite créer un BlogService selon notre doubles de test et vous assurer que les données que nous obtenons à partir de GetAllBlogs sont triées par nom.  

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

### <a name="testing-with-async-queries"></a>Test avec des requêtes asynchrones

Entity Framework 6 a introduit un ensemble de méthodes d’extension qui peut être utilisé pour exécuter de façon asynchrone une requête. ToListAsync, FirstAsync, ForEachAsync, etc. sont des exemples de ces méthodes.  

Étant donné que les requêtes Entity Framework utiliser LINQ, les méthodes d’extension sont définies sur IQueryable et IEnumerable. Toutefois, car ils sont uniquement conçus pour être utilisé avec Entity Framework, vous pouvez recevoir l’erreur suivante si vous tentez de les utiliser sur une requête LINQ qui n’est pas une requête Entity Framework :

> La source IQueryable n’implémente pas IDbAsyncEnumerable{0}. Uniquement les sources qui implémentent IDbAsyncEnumerable peuvent être utilisées pour les opérations asynchrones d’Entity Framework. Pour plus d’informations, consultez [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Tandis que les méthodes asynchrones sont uniquement pris en charge en cours d’exécution par rapport à une requête Entity Framework, vous souhaiterez utilisez-les dans votre test unitaire en cours d’exécution par rapport à une mémoire tester double d’un DbSet.  

Pour pouvoir utiliser les méthodes asynchrones, nous devons créer un DbAsyncQueryProvider en mémoire pour traiter la requête asynchrone. Tout en il serait possible de configurer un fournisseur de requête à l’aide de Moq, il est beaucoup plus facile de créer une implémentation de doubles de test dans le code. Le code pour cette implémentation est la suivante :  

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

Maintenant que nous disposons d’un fournisseur de requête async, nous pouvons écrire un test unitaire pour notre nouvelle méthode GetAllBlogsAsync.  

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
