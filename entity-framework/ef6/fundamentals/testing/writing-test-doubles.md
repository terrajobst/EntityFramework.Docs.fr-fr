---
title: Test avec votre propre doubles de test - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997215"
---
# <a name="testing-with-your-own-test-doubles"></a>Test avec votre propre doubles de test
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Lors de l’écriture de tests pour votre application, il est souvent souhaitable d’éviter d’atteindre la base de données.  Entity Framework vous permet de pour y parvenir en créant un contexte : avec le comportement défini par vos tests, qui utilise des données en mémoire.  

## <a name="options-for-creating-test-doubles"></a>Options de création de doubles de test  

Il existe deux approches différentes qui peuvent être utilisés pour créer une version en mémoire de votre contexte.  

- **Créer votre propre doubles de test** – cette approche implique d’écrire votre propre implémentation en mémoire de votre contexte et de DbSets. Cela vous donne un grand nombre de contrôle sur la façon dont les classes se comportent mais peuvent impliquer écriture et qui possède une quantité raisonnable de code.  
- **Utiliser une infrastructure de simulation pour créer des doubles de test** – à l’aide d’une infrastructure de simulation (par exemple, Moq) vous pouvez avoir les implémentations en mémoire de votre contexte et des jeux créés dynamiquement lors de l’exécution pour vous.  

Cet article traitera de création de votre propre test double. Pour plus d’informations sur l’utilisation d’une infrastructure de simulation consultez [test avec une infrastructure de simulation](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Test avec les versions pre-EF6  

Le code présenté dans cet article est compatible avec EF6. Pour le test avec EF5 et version antérieure, consultez [test avec un contexte fictif](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitations EF en mémoire des doubles de test  

Les doubles de test en mémoire peut être un bon moyen de fournir une couverture au niveau des bits de votre application qui utilisent Entity Framework de test unitaire. Toutefois, lors de cette opération vous utilisez LINQ to Objects pour exécuter des requêtes sur les données en mémoire. Cela peut entraîner un comportement différent par rapport à l’aide du fournisseur LINQ d’Entity Framework (LINQ to Entities) pour traduire des requêtes dans SQL est exécutée sur votre base de données.  

Un exemple de cette différence est le chargement des données connexes. Si vous créez une série de Blogs que chaque ont billets de blog, puis lors de l’utilisation des données en mémoire les messages liés sont toujours chargés pour chaque Blog. Toutefois, lors de l’exécution par rapport à une base de données les données sont chargées uniquement si vous utilisez la méthode Include.  

Pour cette raison, il est recommandé de toujours inclure un niveau de bout en bout à tester (en plus de vos tests unitaires) pour s’assurer de votre application fonctionne correctement sur une base de données.  

## <a name="following-along-with-this-article"></a>Poursuivez la procédure avec cet article  

Cet article donne des listes de code complet, vous pouvez copier dans Visual Studio pour suivre la procédure si vous le souhaitez. Il est plus facile de créer un **projet de Test unitaire** et vous devez à la cible **.NET Framework 4.5** pour terminer les sections qui utilisent async.  

## <a name="creating-a-context-interface"></a>Création d’une interface de contexte  

Nous allons examiner le test d’un service qui utilise un EF modèle. Pour pouvoir remplacer notre contexte EF avec une version en mémoire pour le test, nous allons définir une interface que notre contexte EF (et son double de mémoire) seront imeplement.  

Le service que nous allons tester interroger et modifier des données en utilisant les propriétés DbSet de notre contexte et également appeler SaveChanges pour transmettre des modifications à la base de données. Par conséquent, nous incluons ces membres sur l’interface.  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a>Le modèle EF  

Le service que nous allons tester utilise une EF modèle constitué de le BloggingContext et les classes de Blog et Post. Ce code peut avoir été généré par le Concepteur EF ou être un modèle Code First.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implémentation de l’interface de contexte avec le Concepteur EF  

Notez que notre contexte implémente l’interface IBloggingContext.  

Si vous utilisez Code First vous pouvez modifier votre contexte directement pour implémenter l’interface. Si vous utilisez le Concepteur EF vous devrez modifier le modèle T4 qui génère votre contexte. Ouvrez le \<model_name\>. Fichier context.tt qui est imbriqué sous vous edmx le fichier, recherchez le fragment de code suivant et ajoutez dans l’interface comme indiqué.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
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

<a name="creating-the-in-memory-test-doubles"/> ## Création le test en mémoire double  

Maintenant que nous avons le modèle EF réel et le service qui peut l’utiliser, il est temps de créer le test en mémoire double que nous pouvons utiliser pour le test. Nous avons créé un test TestContext double pour notre contexte. Dans les doubles de test que consiste à choisir le comportement souhaité pour prendre en charge les tests, nous allons exécuter. Dans cet exemple, nous allons simplement capture le nombre de fois que SaveChanges est appelée, mais vous pouvez inclure la logique est nécessaire pour vérifier le scénario que vous testez.  

Nous avons également créé un TestDbSet qui fournit une implémentation en mémoire de DbSet. Nous vous avons fourni une implémentation complète pour toutes les méthodes sur un DbSet (sauf pour les rechercher), mais vous devez uniquement implémenter les membres de votre scénario de test doit utiliser.  

TestDbSet utilise d’autres classes d’infrastructure que nous avons inclus pour vous assurer que les requêtes asynchrones peuvent être traités.  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

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

### <a name="implementing-find"></a>Implémentation de recherche  

La méthode Find est difficile à implémenter de manière générique. Si vous avez besoin tester le code qui rend utiliser de la méthode Find, il est plus facile de créer un test trouver DbSet pour chacun des types d’entités qui doivent prendre en charge. Vous pouvez ensuite écrire la logique permettant de rechercher ce type particulier d’entité, comme indiqué ci-dessous.  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a>Écriture de tests  

C’est tout que nous devons faire pour commencer à tester. Le test suivant crée un TestContext, puis un service basé sur ce contexte. Le service est ensuite utilisé pour créer un nouveau blog – à l’aide de la méthode AddBlog. Enfin, le test vérifie que le service ajouté un nouveau Blog à la propriété de Blogs du contexte et appelé SaveChanges sur le contexte.  

C’est juste un exemple de types de choses que vous pouvez tester avec un double de test en mémoire et vous pouvez ajuster la logique des doubles de test et la vérification pour répondre à vos besoins.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

Voici un autre exemple d’un test - instant qui effectue une requête. Le test démarre en créant un contexte de test avec des données dans sa propriété Blog - Notez que les données ne sont pas dans l’ordre alphabétique. Nous pouvons ensuite créer un BlogService basé sur notre contexte de test et vous assurer que les données que nous obtenons à partir de GetAllBlogs sont triées par nom.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

Enfin, nous allons écrire un test supplémentaire qui utilise notre méthode async pour vous assurer que l’infrastructure d’async que nous avons inclus dans [TestDbSet](#creating-the-in-memory-test-doubles) fonctionne.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
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
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
