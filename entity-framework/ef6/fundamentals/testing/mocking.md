---
title: Test con un framework di simulazione - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 80fd97073744be40d66c09706d3513dba18e724d
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834721"
---
# <a name="testing-with-a-mocking-framework"></a>Test con un framework di simulazione
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Quando si scrivono test per l'applicazione è spesso preferibile evitare di raggiungere il database.  Entity Framework consente di ottenere questo risultato mediante la creazione di un contesto: con comportamento definito per il test, che usa i dati in memoria.  

## <a name="options-for-creating-test-doubles"></a>Opzioni per la creazione di copie di test  

Esistono due diversi approcci che possono essere utilizzati per creare una versione in memoria del contesto.  

- **Creare il proprio test Double** – questo approccio richiede di scrivere la propria implementazione in memoria del contesto e DbSet. Ciò offre un notevole controllo sul modo in cui le classi si comportano ma possono comportare la scrittura e proprietario di una quantità ragionevole di codice.  
- **Usare un framework di simulazione per creare copie di test** – usando un framework di simulazione (ad esempio Moq) è possibile avere le implementazioni in memoria del contesto e i set creati dinamicamente in fase di esecuzione per l'utente.  

Questo articolo gestirà usando un framework di simulazione. Per la creazione di copie di test personalizzati, vedere [test con copie di Test Your proprio](writing-test-doubles.md).  

Per illustrare l'uso di EF con un framework di simulazione si intende usare Moq. Il modo più semplice per ottenere Moq consiste nell'installare il [Moq pacchetto da NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Test con le versioni di pre-EF6  

Lo scenario illustrato in questo articolo è dipendente da alcune modifiche sono state apportate a DbSet in EF6. Per i test con EF5 e versione precedente, vedere [test con un contesto di Fake](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitazioni delle copie di test in memoria di Entity Framework  

Copie di test in memoria possono essere un buon metodo per fornire la copertura a livello di bit dell'applicazione che usa Entity Framework di unit test. Tuttavia, quando in questo modo si utilizza LINQ to Objects per eseguire query su dati in memoria. Ciò può comportare un comportamento diverso rispetto all'uso di provider LINQ di Entity Framework (LINQ to Entities) per tradurre le query SQL che viene eseguita per il database.  

Un esempio di questo tipo una differenza consiste nel caricare i dati correlati. Se si crea una serie di blog post che ogni sono correlati, quindi quando si usano dati in memoria i post correlati verranno caricati sempre per ogni Blog. Tuttavia, durante l'esecuzione di un database i dati solo essere caricati se si usa il metodo Include.  

Per questo motivo, è consigliabile includere sempre un certo livello di test end-to-end (oltre a unit test) per garantire che l'applicazione funzioni correttamente su un database.  

## <a name="following-along-with-this-article"></a>Seguendo insieme a questo articolo  

Questo articolo riporta i listati di codice completo che è possibile copiare in Visual Studio per seguire la procedura se si desidera. È più semplice creare un **progetto di Unit Test** e sarà necessario alla destinazione **.NET Framework 4.5** per completare le sezioni che usino la modalità asincrona.  

## <a name="the-ef-model"></a>Il modello di Entity Framework  

Il servizio dobbiamo testare si avvale di un Entity Framework model costituita dal BloggingContext e le classi di Blog e Post. Questo codice potrebbe essere stato generato dalla finestra di progettazione di Entity Framework o da un modello Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Proprietà DbSet virtuali con Entity Framework Designer  

Si noti che le proprietà DbSet sul contesto siano contrassegnate come virtuali. In questo modo il framework di comportamento fittizio da cui derivare il contesto e si esegue l'override di queste proprietà con un'implementazione fittizia.  

Se si utilizza Code First è possibile modificare le classi direttamente. Se si usa la finestra di progettazione di Entity Framework è necessario modificare il modello T4 che genera il contesto. Aprire il \<model_name\>. File Context.tt è annidato sotto si file edmx, trovare il seguente frammento di codice e aggiungere la parola chiave virtuale, come illustrato.  

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

## <a name="service-to-be-tested"></a>Servizio da sottoporre a test  

Per illustrare il test mediante copie di test in memoria si intende scrivere un paio di test per un BlogService. Il servizio è in grado di creare nuovi blog (AddBlog) e la restituzione di tutti i blog ordinati per nome (GetAllBlogs). Oltre a GetAllBlogs, è stato fornito anche un metodo che verrà visualizzato in modo asincrono tutti i blog ordinati per nome (GetAllBlogsAsync).  

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

## <a name="testing-non-query-scenarios"></a>Scenari non di query di test  

Questo è tutto che è necessario eseguire per avviare il test di metodi non di query. Il test seguente usa Moq per creare un contesto. Viene quindi creato un elemento DbSet\<Blog\> e lo collega deve essere restituito dalla proprietà di blog del contesto. Successivamente, il contesto viene usato per creare un nuovo BlogService che viene quindi usato per creare un nuovo post di blog: usando il metodo AddBlog. Infine, il test verifica che il servizio aggiunto un nuovo post di Blog e chiamato SaveChanges su contesto.  

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

## <a name="testing-query-scenarios"></a>Scenari di query di test  

Per essere in grado di eseguire query su test DbSet double è necessario configurare un'implementazione di IQueryable. Il primo passaggio consiste nel creare alcuni dati in memoria – si usa un elenco\<Blog\>. Quindi, creiamo un contesto e un elemento DBSet\<Blog\> associare quindi l'implementazione di IQueryable per alla classe DbSet – sono semplicemente delega per il provider LINQ to Objects che funziona con elenco\<T\>.  

È quindi possibile creare un BlogService basato sul nostro copie di test e assicurarsi che i dati ottenuti da GetAllBlogs vengono ordinati in base al nome.  

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
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

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

### <a name="testing-with-async-queries"></a>Test con le query asincrone

Entity Framework 6 è stato introdotto un set di metodi di estensione che può essere utilizzato per eseguire in modo asincrono una query. Esempi di questi metodi includono ToListAsync FirstAsync, ForEachAsync, e così via.  

Poiché le query Entity Framework prevedono l'utilizzo di LINQ, vengono definiti i metodi di estensione per interfaccia IQueryable e IEnumerable. Tuttavia, poiché solo sono progettate per essere usato con Entity Framework si potrebbe ricevere l'errore seguente se si prova a utilizzarli su una query LINQ che non sia una query Entity Framework:

> L'origine IQueryable non implementa IDbAsyncEnumerable{0}. Solo le origini che implementano IDbAsyncEnumerable possono essere utilizzate per le operazioni asincrone di Entity Framework. Per altre informazioni, vedere [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Mentre i metodi asincroni sono supportati solo durante l'esecuzione di una query Entity Framework, è possibile usarli nello unit test quando esegue il test in esecuzione su una in-memoria doppia di un elemento DbSet.  

Per usare i metodi asincroni è necessario creare un DbAsyncQueryProvider in memoria per l'elaborazione di query asincrona. Mentre è possibile configurare un provider di query con Moq, risulta molto più semplice creare un'implementazione di doppia test nel codice. Il codice per questa implementazione è come segue:  

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

Ora che abbiamo un provider di query async è possibile scrivere uno unit test per il nuovo metodo GetAllBlogsAsync.  

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
