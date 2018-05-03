---
title: Dati - Core EF correlati di caricamento
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="a6a41-102">Caricamento dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="a6a41-102">Loading Related Data</span></span>

<span data-ttu-id="a6a41-103">Entity Framework Core consente di utilizzare le proprietà di navigazione del modello per caricare entità correlate.</span><span class="sxs-lookup"><span data-stu-id="a6a41-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="a6a41-104">Esistono tre modelli di O/RM comune utilizzati per caricare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="a6a41-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="a6a41-105">**Caricamento eager** indica che i dati correlati vengono caricati dal database come parte della query iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6a41-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="a6a41-106">**Caricamento esplicito** indica che i dati correlati vengono caricati in modo esplicito dal database in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="a6a41-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="a6a41-107">**Caricamento lazy** indica che i dati correlati vengono caricati in modo trasparente dal database quando si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="a6a41-108">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6a41-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="a6a41-109">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="a6a41-109">Eager loading</span></span>

<span data-ttu-id="a6a41-110">È possibile utilizzare il `Include` per specificare i dati correlati da includere nei risultati della query.</span><span class="sxs-lookup"><span data-stu-id="a6a41-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="a6a41-111">Nell'esempio seguente, sarà necessario il blog che vengono restituiti nei risultati della loro `Posts` proprietà popolata con i post correlati.</span><span class="sxs-lookup"><span data-stu-id="a6a41-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="a6a41-112">Entity Framework Core verrà automaticamente correzione proprietà di navigazione da qualsiasi altra entità che sono stata caricata in precedenza l'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="a6a41-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="a6a41-113">Pertanto, anche se in modo esplicito non includono i dati per una proprietà di navigazione, la proprietà comunque possibile popolare se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a6a41-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="a6a41-114">È possibile includere dati correlati tratti da più relazioni in una singola query.</span><span class="sxs-lookup"><span data-stu-id="a6a41-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="a6a41-115">Inclusi più livelli</span><span class="sxs-lookup"><span data-stu-id="a6a41-115">Including multiple levels</span></span>

<span data-ttu-id="a6a41-116">È possibile il drill-down tramite le relazioni da includere più livelli di dati correlati mediante la `ThenInclude` metodo.</span><span class="sxs-lookup"><span data-stu-id="a6a41-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="a6a41-117">Nell'esempio seguente carica tutti i blog, i post correlati e l'autore di ogni post.</span><span class="sxs-lookup"><span data-stu-id="a6a41-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="a6a41-118">Le versioni correnti di Visual Studio offrono le opzioni di completamento di codice non corretto e può causare espressioni corrette viene contrassegnata con errori di sintassi quando si utilizza il `ThenInclude` metodo dopo una proprietà di navigazione della raccolta.</span><span class="sxs-lookup"><span data-stu-id="a6a41-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="a6a41-119">Si tratta di un sintomo di un bug IntelliSense rilevato al https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="a6a41-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="a6a41-120">È possibile ignorare questi errori di sintassi di vario tipo, purché il codice sia corretto e può essere compilato correttamente.</span><span class="sxs-lookup"><span data-stu-id="a6a41-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="a6a41-121">È possibile concatenare più chiamate a `ThenInclude` continuare inclusi ulteriori livelli di dati correlati.</span><span class="sxs-lookup"><span data-stu-id="a6a41-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="a6a41-122">È possibile combinare queste per includere dati correlati tratti da più livelli e più nodi radice nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="a6a41-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="a6a41-123">È consigliabile includere più entità correlate per un'entità che è incluso.</span><span class="sxs-lookup"><span data-stu-id="a6a41-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="a6a41-124">Ad esempio, quando si eseguono query `Blog`s, includere `Posts` e si desidera includere sia il `Author` e `Tags` del `Posts`.</span><span class="sxs-lookup"><span data-stu-id="a6a41-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="a6a41-125">A tale scopo, è necessario specificare ogni percorso iniziando dalla radice di inclusione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="a6a41-126">Ad esempio, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="a6a41-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="a6a41-127">Ciò non significa che si otterranno join ridondanti, nella maggior parte dei casi che verranno consolidate EF i join durante la generazione di SQL.</span><span class="sxs-lookup"><span data-stu-id="a6a41-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="a6a41-128">Includere tipi derivati</span><span class="sxs-lookup"><span data-stu-id="a6a41-128">Include on derived types</span></span>

<span data-ttu-id="a6a41-129">È possibile includere dati correlati di spostamenti definiti solo su un tipo derivato mediante `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="a6a41-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="a6a41-130">Dato il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="a6a41-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="a6a41-131">Contenuto di `School` spostamento di tutti gli utenti sono gli studenti può essere caricato in blocco utilizzando un numero di modelli:</span><span class="sxs-lookup"><span data-stu-id="a6a41-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="a6a41-132">utilizzo di cast</span><span class="sxs-lookup"><span data-stu-id="a6a41-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="a6a41-133">utilizzando `as` (operatore)</span><span class="sxs-lookup"><span data-stu-id="a6a41-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="a6a41-134">utilizzando l'overload di `Include` che accetta parametri di tipo `string`</span><span class="sxs-lookup"><span data-stu-id="a6a41-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="a6a41-135">Ignorato include</span><span class="sxs-lookup"><span data-stu-id="a6a41-135">Ignored includes</span></span>

<span data-ttu-id="a6a41-136">Se si modifica la query in modo che non restituisca non sono più istanze del tipo di entità che la query inizia con, vengono ignorati gli operatori di inclusione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="a6a41-137">Nell'esempio seguente, si basano gli operatori di inclusione di `Blog`, tuttavia, il `Select` operatore viene usato per modificare la query per restituire un tipo anonimo.</span><span class="sxs-lookup"><span data-stu-id="a6a41-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="a6a41-138">In questo caso, gli operatori di inclusione non hanno effetto.</span><span class="sxs-lookup"><span data-stu-id="a6a41-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="a6a41-139">Per impostazione predefinita, Core EF registrerà un avviso quando includono operatori vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="a6a41-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="a6a41-140">Vedere [registrazione](../miscellaneous/logging.md) per ulteriori informazioni sulla visualizzazione dell'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="a6a41-141">È possibile modificare il comportamento quando un operatore di inclusione viene ignorato per generare o non eseguono alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="a6a41-142">Questa operazione viene eseguita quando si configura le opzioni per il contesto, in genere in `DbContext.OnConfiguring`, o in `Startup.cs` se si utilizza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6a41-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="a6a41-143">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="a6a41-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="a6a41-144">Questa funzionalità è stato introdotto in Entity Framework Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="a6a41-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="a6a41-145">È possibile caricare in modo esplicito una proprietà di navigazione tramite il `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="a6a41-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="a6a41-146">Eseguendo una query separata che restituisce le entità correlate, è possibile caricare anche in modo esplicito una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="a6a41-147">Se è abilitato il rilevamento delle modifiche, quindi durante il caricamento di un'entità, EF Core verrà automaticamente impostato le proprietà di navigazione del entitiy appena caricati per fare riferimento a qualsiasi entità già caricato e impostare le proprietà di navigazione delle entità già caricato per fare riferimento al entità appena caricati.</span><span class="sxs-lookup"><span data-stu-id="a6a41-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="a6a41-148">Esecuzione di query su entità correlate</span><span class="sxs-lookup"><span data-stu-id="a6a41-148">Querying related entities</span></span>

<span data-ttu-id="a6a41-149">È inoltre possibile ottenere una query LINQ che rappresenta il contenuto di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a6a41-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="a6a41-150">Ciò consente di eseguire operazioni quali l'esecuzione di un operatore di aggregazione senza il loro caricamento in memoria tramite le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="a6a41-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="a6a41-151">È inoltre possibile filtrare le entità correlate vengono caricate in memoria.</span><span class="sxs-lookup"><span data-stu-id="a6a41-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="a6a41-152">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="a6a41-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="a6a41-153">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a6a41-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="a6a41-154">È il modo più semplice per utilizzare il caricamento lazy installando il [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacchetto e abilitarlo con una chiamata a `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="a6a41-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="a6a41-155">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a6a41-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="a6a41-156">O quando si utilizza AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="a6a41-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="a6a41-157">Core EF verrà quindi abilitare il caricamento lazy per qualsiasi proprietà di navigazione che può essere sottoposto a override, è, deve essere `virtual` e in una classe che può essere ereditata.</span><span class="sxs-lookup"><span data-stu-id="a6a41-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="a6a41-158">Ad esempio, nelle entità seguenti, il `Post.Blog` e `Blog.Posts` le proprietà di navigazione sarà caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="a6a41-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="a6a41-159">Caricamento lazy senza proxy</span><span class="sxs-lookup"><span data-stu-id="a6a41-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="a6a41-160">Caricamento lazy proxy funziona inserendo la `ILazyLoader` servizio in un'entità, come descritto [costruttori di tipo entità](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="a6a41-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="a6a41-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a6a41-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="a6a41-162">In questo non richiede tipi di entità da cui ereditare o le proprietà di navigazione per essere virtuale e le istanze di entità create con `new` -caricamento lazy una volta collegato a un contesto.</span><span class="sxs-lookup"><span data-stu-id="a6a41-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="a6a41-163">Tuttavia, è necessario un riferimento di `ILazyLoader` servizio, che combina un sistema di tipi di entità per l'assembly principale di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a6a41-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="a6a41-164">Per evitare questa base di Entity Framework consente di `ILazyLoader.Load` metodo essere inseriti come un delegato.</span><span class="sxs-lookup"><span data-stu-id="a6a41-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="a6a41-165">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a6a41-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="a6a41-166">Il codice precedente viene usata una `Load` metodo di estensione utilizzando il delegato a un bit pulitura:</span><span class="sxs-lookup"><span data-stu-id="a6a41-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="a6a41-167">Il parametro di costruttore per il caricamento lazy delegato deve essere chiamato "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="a6a41-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="a6a41-168">Configurazione da utilizzare un nome diverso, che questo viene pianificato per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="a6a41-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="a6a41-169">Serializzazione e i dati correlati</span><span class="sxs-lookup"><span data-stu-id="a6a41-169">Related data and serialization</span></span>

<span data-ttu-id="a6a41-170">Poiché le proprietà di navigazione di correzione automaticamente verrà di Entity Framework Core, possono finire con cicli nell'oggetto grafico.</span><span class="sxs-lookup"><span data-stu-id="a6a41-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="a6a41-171">Ad esempio, il caricamento di un blog e è correlato post verrà creato un oggetto di blog che fa riferimento a un insieme di pubblicazioni.</span><span class="sxs-lookup"><span data-stu-id="a6a41-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="a6a41-172">Ognuno di tali pubblicazioni avrà un riferimento al blog.</span><span class="sxs-lookup"><span data-stu-id="a6a41-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="a6a41-173">Alcuni framework di serializzazione non consentono tali cicli.</span><span class="sxs-lookup"><span data-stu-id="a6a41-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="a6a41-174">Ad esempio, Json.NET genererà l'eccezione seguente se viene rilevato un ciclo.</span><span class="sxs-lookup"><span data-stu-id="a6a41-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="a6a41-175">Newtonsoft.Json.JsonSerializationException: Self ciclo rilevato per la proprietà 'Blog' con 'MyApplication.Models.Blog' di tipo di riferimento.</span><span class="sxs-lookup"><span data-stu-id="a6a41-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="a6a41-176">Se si utilizza ASP.NET Core, è possibile configurare Json.NET per ignorare i cicli che trova nell'oggetto grafico.</span><span class="sxs-lookup"><span data-stu-id="a6a41-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="a6a41-177">Questa operazione viene eseguita `ConfigureServices(...)` metodo `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="a6a41-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
