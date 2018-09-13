---
title: Test con un framework di simulazione - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: b50d0afb52ae1c496f2734ecc015cdaaa060aff7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489973"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="64182-102">Test con un framework di simulazione</span><span class="sxs-lookup"><span data-stu-id="64182-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="64182-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="64182-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="64182-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="64182-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="64182-105">Quando si scrivono test per l'applicazione è spesso preferibile evitare di raggiungere il database.</span><span class="sxs-lookup"><span data-stu-id="64182-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="64182-106">Entity Framework consente di ottenere questo risultato mediante la creazione di un contesto: con comportamento definito per il test, che usa i dati in memoria.</span><span class="sxs-lookup"><span data-stu-id="64182-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="64182-107">Opzioni per la creazione di copie di test</span><span class="sxs-lookup"><span data-stu-id="64182-107">Options for creating test doubles</span></span>  

<span data-ttu-id="64182-108">Esistono due diversi approcci che possono essere utilizzati per creare una versione in memoria del contesto.</span><span class="sxs-lookup"><span data-stu-id="64182-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="64182-109">**Creare il proprio test Double** – questo approccio richiede di scrivere la propria implementazione in memoria del contesto e DbSet.</span><span class="sxs-lookup"><span data-stu-id="64182-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="64182-110">Ciò offre un notevole controllo sul modo in cui le classi si comportano ma possono comportare la scrittura e proprietario di una quantità ragionevole di codice.</span><span class="sxs-lookup"><span data-stu-id="64182-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="64182-111">**Usare un framework di simulazione per creare copie di test** – usando un framework di simulazione (ad esempio Moq) è possibile avere le implementazioni in memoria del contesto e i set creati dinamicamente in fase di esecuzione per l'utente.</span><span class="sxs-lookup"><span data-stu-id="64182-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="64182-112">Questo articolo gestirà usando un framework di simulazione.</span><span class="sxs-lookup"><span data-stu-id="64182-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="64182-113">Per la creazione di copie di test personalizzati, vedere [test con copie di Test Your proprio](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="64182-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="64182-114">Per illustrare l'uso di EF con un framework di simulazione si intende usare Moq.</span><span class="sxs-lookup"><span data-stu-id="64182-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="64182-115">Il modo più semplice per ottenere Moq consiste nell'installare il [Moq pacchetto da NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="64182-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="64182-116">Test con le versioni di pre-EF6</span><span class="sxs-lookup"><span data-stu-id="64182-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="64182-117">Lo scenario illustrato in questo articolo è dipendente da alcune modifiche sono state apportate a DbSet in EF6.</span><span class="sxs-lookup"><span data-stu-id="64182-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="64182-118">Per i test con EF5 e versione precedente, vedere [test con un contesto di Fake](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="64182-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="64182-119">Limitazioni delle copie di test in memoria di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="64182-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="64182-120">Copie di test in memoria possono essere un buon metodo per fornire la copertura a livello di bit dell'applicazione che usa Entity Framework di unit test.</span><span class="sxs-lookup"><span data-stu-id="64182-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="64182-121">Tuttavia, quando in questo modo si utilizza LINQ to Objects per eseguire query su dati in memoria.</span><span class="sxs-lookup"><span data-stu-id="64182-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="64182-122">Ciò può comportare un comportamento diverso rispetto all'uso di provider LINQ di Entity Framework (LINQ to Entities) per tradurre le query SQL che viene eseguita per il database.</span><span class="sxs-lookup"><span data-stu-id="64182-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="64182-123">Un esempio di questo tipo una differenza consiste nel caricare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="64182-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="64182-124">Se si crea una serie di blog post che ogni sono correlati, quindi quando si usano dati in memoria i post correlati verranno caricati sempre per ogni Blog.</span><span class="sxs-lookup"><span data-stu-id="64182-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="64182-125">Tuttavia, durante l'esecuzione di un database i dati solo essere caricati se si usa il metodo Include.</span><span class="sxs-lookup"><span data-stu-id="64182-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="64182-126">Per questo motivo, è consigliabile includere sempre un certo livello di test end-to-end (oltre a unit test) per garantire che l'applicazione funzioni correttamente su un database.</span><span class="sxs-lookup"><span data-stu-id="64182-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="64182-127">Seguendo insieme a questo articolo</span><span class="sxs-lookup"><span data-stu-id="64182-127">Following along with this article</span></span>  

<span data-ttu-id="64182-128">Questo articolo riporta i listati di codice completo che è possibile copiare in Visual Studio per seguire la procedura se si desidera.</span><span class="sxs-lookup"><span data-stu-id="64182-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="64182-129">È più semplice creare un **progetto di Unit Test** e sarà necessario alla destinazione **.NET Framework 4.5** per completare le sezioni che usino la modalità asincrona.</span><span class="sxs-lookup"><span data-stu-id="64182-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="64182-130">Il modello di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="64182-130">The EF model</span></span>  

<span data-ttu-id="64182-131">Il servizio dobbiamo testare si avvale di un Entity Framework model costituita dal BloggingContext e le classi di Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="64182-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="64182-132">Questo codice potrebbe essere stato generato dalla finestra di progettazione di Entity Framework o da un modello Code First.</span><span class="sxs-lookup"><span data-stu-id="64182-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="64182-133">Proprietà DbSet virtuali con Entity Framework Designer</span><span class="sxs-lookup"><span data-stu-id="64182-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="64182-134">Si noti che le proprietà DbSet sul contesto siano contrassegnate come virtuali.</span><span class="sxs-lookup"><span data-stu-id="64182-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="64182-135">In questo modo il framework di comportamento fittizio da cui derivare il contesto e si esegue l'override di queste proprietà con un'implementazione fittizia.</span><span class="sxs-lookup"><span data-stu-id="64182-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="64182-136">Se si utilizza Code First è possibile modificare le classi direttamente.</span><span class="sxs-lookup"><span data-stu-id="64182-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="64182-137">Se si usa la finestra di progettazione di Entity Framework è necessario modificare il modello T4 che genera il contesto.</span><span class="sxs-lookup"><span data-stu-id="64182-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="64182-138">Aprire il \<model_name\>. File Context.tt è annidato sotto si file edmx, trovare il seguente frammento di codice e aggiungere la parola chiave virtuale, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="64182-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="64182-139">Servizio da sottoporre a test</span><span class="sxs-lookup"><span data-stu-id="64182-139">Service to be tested</span></span>  

<span data-ttu-id="64182-140">Per illustrare il test mediante copie di test in memoria si intende scrivere un paio di test per un BlogService.</span><span class="sxs-lookup"><span data-stu-id="64182-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="64182-141">Il servizio è in grado di creare nuovi blog (AddBlog) e la restituzione di tutti i blog ordinati per nome (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="64182-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="64182-142">Oltre a GetAllBlogs, è stato fornito anche un metodo che verrà visualizzato in modo asincrono tutti i blog ordinati per nome (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="64182-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="64182-143">Scenari non di query di test</span><span class="sxs-lookup"><span data-stu-id="64182-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="64182-144">Questo è tutto che è necessario eseguire per avviare il test di metodi non di query.</span><span class="sxs-lookup"><span data-stu-id="64182-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="64182-145">Il test seguente usa Moq per creare un contesto.</span><span class="sxs-lookup"><span data-stu-id="64182-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="64182-146">Viene quindi creato un elemento DbSet\<Blog\> e lo collega deve essere restituito dalla proprietà di blog del contesto.</span><span class="sxs-lookup"><span data-stu-id="64182-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="64182-147">Successivamente, il contesto viene usato per creare un nuovo BlogService che viene quindi usato per creare un nuovo post di blog: usando il metodo AddBlog.</span><span class="sxs-lookup"><span data-stu-id="64182-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="64182-148">Infine, il test verifica che il servizio aggiunto un nuovo post di Blog e chiamato SaveChanges su contesto.</span><span class="sxs-lookup"><span data-stu-id="64182-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="64182-149">Scenari di query di test</span><span class="sxs-lookup"><span data-stu-id="64182-149">Testing query scenarios</span></span>  

<span data-ttu-id="64182-150">Per essere in grado di eseguire query su test DbSet double è necessario configurare un'implementazione di IQueryable.</span><span class="sxs-lookup"><span data-stu-id="64182-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="64182-151">Il primo passaggio consiste nel creare alcuni dati in memoria – si usa un elenco\<Blog\>.</span><span class="sxs-lookup"><span data-stu-id="64182-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="64182-152">Quindi, creiamo un contesto e un elemento DBSet\<Blog\> associare quindi l'implementazione di IQueryable per alla classe DbSet – sono semplicemente delega per il provider LINQ to Objects che funziona con elenco\<T\>.</span><span class="sxs-lookup"><span data-stu-id="64182-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="64182-153">È quindi possibile creare un BlogService basato sul nostro copie di test e assicurarsi che i dati ottenuti da GetAllBlogs vengono ordinati in base al nome.</span><span class="sxs-lookup"><span data-stu-id="64182-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="64182-154">Test con le query asincrone</span><span class="sxs-lookup"><span data-stu-id="64182-154">Testing with async queries</span></span>

<span data-ttu-id="64182-155">Entity Framework 6 è stato introdotto un set di metodi di estensione che può essere utilizzato per eseguire in modo asincrono una query.</span><span class="sxs-lookup"><span data-stu-id="64182-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="64182-156">Esempi di questi metodi includono ToListAsync FirstAsync, ForEachAsync, e così via.</span><span class="sxs-lookup"><span data-stu-id="64182-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="64182-157">Poiché le query Entity Framework prevedono l'utilizzo di LINQ, vengono definiti i metodi di estensione per interfaccia IQueryable e IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="64182-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="64182-158">Tuttavia, poiché solo sono progettate per essere usato con Entity Framework si potrebbe ricevere l'errore seguente se si prova a utilizzarli su una query LINQ che non sia una query Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="64182-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="64182-159">L'origine IQueryable non implementa IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="64182-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="64182-160">Solo le origini che implementano IDbAsyncEnumerable possono essere utilizzate per le operazioni asincrone di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="64182-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="64182-161">Per altre informazioni, vedere [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="64182-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="64182-162">Mentre i metodi asincroni sono supportati solo durante l'esecuzione di una query Entity Framework, è possibile usarli nello unit test quando esegue il test in esecuzione su una in-memoria doppia di un elemento DbSet.</span><span class="sxs-lookup"><span data-stu-id="64182-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="64182-163">Per usare i metodi asincroni è necessario creare un DbAsyncQueryProvider in memoria per l'elaborazione di query asincrona.</span><span class="sxs-lookup"><span data-stu-id="64182-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="64182-164">Mentre è possibile configurare un provider di query con Moq, risulta molto più semplice creare un'implementazione di doppia test nel codice.</span><span class="sxs-lookup"><span data-stu-id="64182-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="64182-165">Il codice per questa implementazione è come segue:</span><span class="sxs-lookup"><span data-stu-id="64182-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="64182-166">Ora che abbiamo un provider di query async è possibile scrivere uno unit test per il nuovo metodo GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="64182-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
