---
title: Test con il proprio copie di test - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 2158dc73585c2720e7293096b0478c73edf522d9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490909"
---
# <a name="testing-with-your-own-test-doubles"></a>Test con il proprio copie di test
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Quando si scrivono test per l'applicazione è spesso preferibile evitare di raggiungere il database.  Entity Framework consente di ottenere questo risultato mediante la creazione di un contesto: con comportamento definito per il test, che usa i dati in memoria.  

## <a name="options-for-creating-test-doubles"></a>Opzioni per la creazione di copie di test  

Esistono due diversi approcci che possono essere utilizzati per creare una versione in memoria del contesto.  

- **Creare il proprio test Double** – questo approccio richiede di scrivere la propria implementazione in memoria del contesto e DbSet. Ciò offre un notevole controllo sul modo in cui le classi si comportano ma possono comportare la scrittura e proprietario di una quantità ragionevole di codice.  
- **Usare un framework di simulazione per creare copie di test** – usando un framework di simulazione (ad esempio Moq) è possibile avere le implementazioni in memoria del contesto e i set creati dinamicamente in fase di esecuzione per l'utente.  

Questo articolo gestirà la creazione di propri test double. Per informazioni sull'uso di un framework di simulazione, vedere [test con un Framework di simulazione](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Test con le versioni di pre-EF6  

Il codice illustrato in questo articolo è compatibile con Entity Framework 6. Per i test con EF5 e versione precedente, vedere [test con un contesto di Fake](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitazioni delle copie di test in memoria di Entity Framework  

Copie di test in memoria possono essere un buon metodo per fornire la copertura a livello di bit dell'applicazione che usa Entity Framework di unit test. Tuttavia, quando in questo modo si utilizza LINQ to Objects per eseguire query su dati in memoria. Ciò può comportare un comportamento diverso rispetto all'uso di provider LINQ di Entity Framework (LINQ to Entities) per tradurre le query SQL che viene eseguita per il database.  

Un esempio di questo tipo una differenza consiste nel caricare i dati correlati. Se si crea una serie di blog post che ogni sono correlati, quindi quando si usano dati in memoria i post correlati verranno caricati sempre per ogni Blog. Tuttavia, durante l'esecuzione di un database i dati solo essere caricati se si usa il metodo Include.  

Per questo motivo, è consigliabile includere sempre un certo livello di test end-to-end (oltre a unit test) per garantire che l'applicazione funzioni correttamente su un database.  

## <a name="following-along-with-this-article"></a>Seguendo insieme a questo articolo  

Questo articolo riporta i listati di codice completo che è possibile copiare in Visual Studio per seguire la procedura se si desidera. È più semplice creare un **progetto di Unit Test** e sarà necessario alla destinazione **.NET Framework 4.5** per completare le sezioni che usino la modalità asincrona.  

## <a name="creating-a-context-interface"></a>Creazione di un'interfaccia di contesto  

Dobbiamo esaminare test di un servizio che utilizza un Entity Framework model. Per poter essere in grado di sostituire il contesto di Entity Framework con una versione in memoria per il test, verranno definiti un'interfaccia che il contesto di Entity Framework (e relativo valore double in memoria) verrà imeplement.  

Il servizio che si intende testare query e modificare i dati usando le proprietà DbSet del contesto e anche chiamare il metodo SaveChanges per eseguire il push delle modifiche al database. Quindi, verrà incluso questi membri sull'interfaccia.  

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

## <a name="the-ef-model"></a>Il modello di Entity Framework  

Il servizio dobbiamo testare si avvale di un Entity Framework model costituita dal BloggingContext e le classi di Blog e Post. Questo codice potrebbe essere stato generato dalla finestra di progettazione di Entity Framework o da un modello Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementazione dell'interfaccia contesto con Entity Framework Designer  

Si noti che il contesto implementi l'interfaccia IBloggingContext.  

Se si utilizza Code First è possibile modificare il contesto direttamente per implementare l'interfaccia. Se si usa la finestra di progettazione di Entity Framework è necessario modificare il modello T4 che genera il contesto. Aprire il \<model_name\>. File Context.tt è annidato sotto si file edmx, trovare il seguente frammento di codice e aggiungere nell'interfaccia, come illustrato.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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

<a name="creating-the-in-memory-test-doubles"/> # # Creazione di test in memoria raddoppiato  

Ora che abbiamo il modello di EF reale e il servizio che può usarle, è possibile creare il test in memoria che è possibile usare per i test double. È stato creato un test TestContext double per il contesto. In copie di test che è possibile scegliere il comportamento desiderato per supportare i test verrà eseguito. In questo esempio si sta semplicemente acquisizione il numero di volte in cui che viene chiamato SaveChanges, ma è possibile includere qualsiasi logica è necessario per verificare lo scenario in cui che si sta testando.  

Abbiamo creato anche un TestDbSet che fornisce un'implementazione in memoria di DbSet. È stata fornita un'implementazione completa per tutti i metodi su DbSet (ad eccezione di ricerca), ma è sufficiente implementare i membri che verrà usato lo scenario di test.  

TestDbSet Usa alcune altre classi di infrastruttura che è stato incluso per garantire che le query asincrone possono essere elaborate.  

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

### <a name="implementing-find"></a>Implementazione della ricerca  

Il metodo Find è difficile da implementare in modo generico. Se è necessario testare codice che consente di usare il metodo Find è più semplice creare un test trovare DbSet per ognuno dei tipi di entità devono supportare. È quindi possibile scrivere la logica per trovare il particolare tipo di entità, come illustrato di seguito.  

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

## <a name="writing-some-tests"></a>Scrittura di alcuni test  

Questo è tutto che è necessario eseguire per avviare il test. Il test seguente crea un oggetto TestContext e quindi un servizio basato su questo contesto. Il servizio viene quindi utilizzato per creare un nuovo post di blog: usando il metodo AddBlog. Infine, il test verifica che il servizio aggiunto un nuovo post di Blog di proprietà di contesto blog e chiamato SaveChanges su contesto.  

Questo è solo un esempio dei tipi di operazioni che è possibile testare con una copia di test in memoria ed è possibile modificare la logica di copie di test e la verifica per soddisfare i requisiti.  

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

Ecco un altro esempio di test - questa volta uno che esegue una query. Il test viene avviato tramite la creazione di un contesto di test con alcuni dati nella relativa proprietà Blog - si noti che i dati non in ordine alfabetico. È quindi possibile creare un BlogService il contesto di test in base e assicurarsi che i dati ottenuti da GetAllBlogs vengono ordinati in base al nome.  

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

Infine, verrà scritto un test più che usa il metodo asincrono per assicurarsi che l'infrastruttura di async è incluso nella [TestDbSet](#creating-the-in-memory-test-doubles) funzioni.  

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
